# Vtiger 6 - Webservices for Products with VAT and Sales Tax  #

In case you are interacting with Vtiger 6 via it's web services interface, you might have noticed that the entity `Products` not not expose it's VAT, service or sales tax (which are referenced from another table).

<!--BREAK-->

Tax information is stored in the table `vtiger_producttaxrel` and there is also a new entity exposed via web services called `ProductTaxes` and also `Taxes`. Unfortunately, because this table is relational, web services cannot work with this table in a proper way. At least from the research I did, if you need to import / export this information via web services, there is no way to do this at this point, because existing classes cannot directly work with this table. Any efforts to retrieve or insert information will fail (largely because this table does not implement an `id` field as primary key as other entities do).

## Working solution for importing / exporting / insert / update / delete Product Tax via Web Services ##

While ideally you wound want just to work just with `Products`, this solution overrides the core class that handles `ProductTaxes`. Any insert / update / retrieve will have to be done in two or more steps, and all will be using the `create` operation, even if semantically it does not make sense.

All examples shown rely on the library `vtwsclib` and only relevant parts of the code are presented. Also the examples not not include error handling. You SHOULD add them to your code.

### Retrieving a new product with VAT or other taxes ###

This needs to be done in two or more steps (if you require more than one tax).

	$recordId = '14x19';
	$recordInfo = $client->doRetrieve($recordId);

	$params = array(
		"operation" => "retrieve",
		"productid" => "14x23",
		"taxid" => "33x1"
	);
	$recordTaxesInfo = $client->doCreate("ProductTaxes", $params);

A tax for a product will look like:

	Array
	(
	    [productid] => 14x19
	    [taxid] => 33x1
	    [taxpercentage] => 17.000
	    [id] => 
	)

### Inserting a new product with VAT ###

	// Create product
	$params = array(
		"productname" => "My product with VAT",
	);
	$createResult = $client->doCreate("Products", $params);

	// Add tax to created product
	$params = array(
		"operation" => "create",
		"productid" => $createResult['id'],
		"taxid" => "33x1",
		"taxpercentage" => "19"
	);
	$createResult = $client->doCreate("ProductTaxes", $params);

### Updating a product with VAT ###

Updating is handled by `doCreate` as well.

	// Updating tax value for existing product
	$params = array(
		"operation" => "update",
		"productid" => "14x16",
		"taxid" => "33x1",
		"taxpercentage" => "16"
	);
	$createResult = $client->doCreate("ProductTaxes", $params);


### Deleting a product ###

No matter how you created the taxes (manually or via webservices), it looks like invoking delete on `Products` will NOT delete it's references in the table `vtiger_producttaxrel`.

	// Delete product with id
	$params = array(
		"id" => "14x17",
	);

	$deleteResult = $client->doInvoke("delete", $params, "POST");

The code above will move the product to the Recycle bin. Emptying it, will not delete data from `vtiger_producttaxrel`. This looks like a bug in Vtiger and I am not addressing this scenario with a fix.

### Deleting just the tax for a product ###

While normally this should be done via invoking delete, to implement / get it to work that way, it would mean a couple more classes and methods that need to be overwritten. I opted for using the `doCreate` do handle the delete process as well.

	$params = array(
		"operation" => "delete",
		"productid" => "14x18",
		"taxid" => "33x1",
	);

	$deleteResult = $client->doCreate("ProductTaxes", $params);

## How to Install the Solution ##

1. Download file `VtigerProductTaxesOperation.php` from [github](https://github.com/vdespa/Vtiger-Web-Services-ProductTaxes). 
2. Copy file `VtigerProductTaxesOperation.php` to `VTIGER_ROOT/include/Webservices/Custom/VtigerProductTaxesOperation.php`.
3. Execute following MySQL query to replace the handler path for the `ProductTaxes` entity (assumption is made that the entry `ProductTaxes` already exists in the table).
`UPDATE vtiger_ws_entity SET handler_path = 'include/Webservices/Custom/VtigerProductTaxesOperation.php' WHERE vtiger_ws_entity.name = "ProductTaxes";`
4. All set!

## Technical Details of the Implementation ##

Querying the `Products` entity does not show the product relationships. There is no join on related tables. From what I understand (https://discussions.vtiger.com/index.php?p=/discussion/171789) this is on the development roadmap, but not yet available in Vtiger 6.0.0. So if you need it now, bummer.

After doing some research / testing, I started overriding methods from the class `VtigerProductTaxesOperation` which itself extends `VtigerActorOperation`. 

`doCreate` is the only operation for `ProductTaxes` that manages to bypass the Vtiger call logic. Any other methods will fail as they rely on you providing a valid id, which for `ProductTaxes` is simply not possible. So I am using `doCreate` as a controller in order to route all operations to the proper method. Semantically bad, but better as overriding tons of classes.

This solution is just a workaround. It's not architecturally sound, but (at least for my needs) it does the job.

Vtiger will need to implement the Products in a similar manner on how the Inventory is handled (https://wiki.vtiger.com/index.php/ServerAPI_reference_manual#Inventory_Record_Create). Meanwhile this is the only solution I have found for Vtiger 6.0.0.

Use with caution and keep on eye on developments on this subject.

## Word of Caution ##

- This is a non-standard implementation. I cannot guarantee that this will work on future versions. It's best to check in version 6.1 or later if this functionality does not already exist.
- Test in advance if this fulfills your authorization needs. I only tested it against an administrator account.  
- Do not use this solution on production environments until you tested in thoroughly and are certain it works as you need it to work and no data corruption or any other issues occur. 
- I decline any responsibility for damages to your installation and database.
- This may conflict with the Inventory creation over web services - test your inventory handling as well.

## Feedback & Contributions ##

The code for this solution if freely available on github. Fell free to fork it and to send a PR is something is broken. If you find something wrong or you want to further improve it, raise a issue or leave a comment in the section below. 


