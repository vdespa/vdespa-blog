# Creating a Simple Extension Module in Vtiger 6 #

Technically what I am trying to do it to **build an "Extension Module"** as per vTiger [definition](https://wiki.vtiger.com/index.php/DevelopingModule#Extension_Module), that can be installed via the Module Manager in the GUI.

<!--BREAK-->

I must say from the start that I am not an expert in vTiger custom development. The reason for writing this is that I had lots of errors like:

> dUnzip2: File 'manifest.xml' is not compressed in the zip

> Invalid File provided for module import! Try Again.

While I won't try to explain the cause of the errors above, I will try to explain how to build your module, sharing all the code necessary.

I've tested the code below in vTiger 6.0.0 beta. All the code is freely available on [github](https://github.com/vdespa/vtiger-simple-extension-module/tree/vtiger-6.0). Fell free to fork it and to send a PR is something is broken.

<div align="center"><!-- Blog Image Ads --> <ins class="adsbygoogle" style="display: inline-block; width: 728px; height: 90px;" data-ad-client="ca-pub-4303200597734966" data-ad-slot="2401911150"></ins></div>

## What will this module do? ##

This plugin will install, register an event handler and later when an Account item is saved, it will catch the data that was saved. If you don't need this functionality, just remove the relevant code from the `SimpleExtension.php` and `SimpleExtensionHandler.php`.

Additionally it will be displayed under Tools, and it will display a basic view.

## Can I use it in Vtiger 5.4? ##

NO, this works just under Vtiger 6.

## Step 0 - Make sure this is for you ##

"Heads up!" First [download the install kit (v.1.1)](https://github.com/vdespa/vtiger-simple-extension-module/raw/extra-module-code/install-kits/simple-extension.1.1.zip) and test if this code works for you. Stop now if this is not working for you.

If the module installed fine, than you are ready to go. I will try to give some basic steps on how to build this. Follow the steps but also the code showed on github.

## Step 1 - The manifest file ##

Create an empty folder what will hold all the files. Generically I will refer to this folder as root.

First thing we need is the `manifest.xml` file. This file is required. I will name my module `SimpleExtension`. Feel free to give it whatever name you like. 

## Step 2 - The module class ##

Create a folder named `Modules` in the root folder and inside it put the file `SimpleExtension.php` and if you need to handle [Vtiger events](https://wiki.vtiger.com/index.php/Eventing "Vtiger Events"), also add `SimpleExtensionHandler.php`.

## Step 3 - Adding the language file ##

Next step is to add the language file. Create a folder named `languages` and put it inside the root folder. Inside `languages` put another folder named `en_us` and place inside it the file `SimpleExtension.php`. It is required to make the `en_us` folder. For other languages use the specific code, eg. `de_de`, `en_gb`, `es_es`, `ro_ro` etc.

## Step 4 - Creating a view ##

In the module folder (/root/modules/SimpleExtension/), add a new folder called `views`. Inside you put the file `List.php` which will be our default view (triggered from top-level Menu). The view will be calling a module template, which we will define later.

Creating the view / template (layout) is not mandatory; the implementation of the MVC architecture provides a fallback to a core class, when a module class has not been found. This means that if Vtiger won't find your view definition, it will just display a default view.

## Step 5 - Adding a module template (layout) ##

You need to create a certain file structure, `\root\layouts\vlayout\modules\SimpleExtension`. Inside the last folder there will be to files: `IndexViewPreProcess.tpl` and `Index.tpl`.

**Note for Vtiger 6.0.0 beta** - I am not sure that is the reason, but it seems that when installing the module, Vtiger is not copying the `layouts` folder into the system. I don't know why (if it's a bug or something I did wrong), but in order to get Step 5 to work, you need to manually merge the `layout` folder with the existing files in the system.

## Step 6 - Creating the zip package ##

Now that we have all the files needed, let's just create the final package. When inside the root folder, select all the files and create an archive. 

When viewing the archive contents, `manifest.xml` and the other folders need to be directly visible (no other folders in between). Do NOT archive the root folder itself.

## Step 7 - See in in action ##

Create a new organization (or change an existing one). When the event `vtiger.entity.aftersave` is triggered, the data will be passed to the event handler in the module.

When this happens, the data will be dumped with `var_dump()` and the script will end. Ending the script here is just done for testing purposes. In "real life" you would do something quietly.


## Step 8 - Uninstalling and re-installing the module ##

If you try to re-install the module, this will not be permitted. In Vtiger 6.0.0 beta I haven't figure out how to upgrade an module. Just changing the version and trying to install it does not work; but I haven't digged to much into this issue.

Basically I run the script below to uninstall the module (put in inside a file in the root folder of the Vtiger installation):

	include_once('vtlib/Vtiger/Module.php');

	/*
	 * Delete a module
	 */
	$module = Vtiger_Module::getInstance('SimpleExtension');
	if($module)
	{
	    // Delete from system
	    $module->delete();
	    echo "Module deleted!";
	} else {
	    echo "Module was not found and could not be deleted!";
	}

Run the file via CLI or browser.

## Why your installation may fail ##

At the beginning of the install process there are several checks being made, inside `Vtiger_PackageImport::checkZip()` method, to see if the package uploaded is a valid package. Basically a couple of rules need to be respected:

- manifest.xml needs to be present 
- Vtiger version in the manifest file must meet the minimum criteria defined by the Vtiger version you are running
- language file needs to be present
- files inside the package must have a certain structure in folders; not having the expected structure leads to confusing error messages
- language file name must fit the module name

## Pro tip  ##

If you can still figure out what is going wrong with your module install, get into the vTiger code. Set your breakpoints for the debugger at the method `Settings_ModuleManager_ModuleImport_View::importUserModuleStep2()` inside the file `/modules/Settings/ModuleManager/views/ModuleImport.php`.

## Did I say or do something wrong? ##

Feel free to leave a comment or correct the problem yourself by sending a PR.

**References and useful links:**

- [Vtiger 6 Developer Guide](https://wiki.vtiger.com/index.php/Vtiger_6_Developer_Guide)
- [Creating Extension Module - wiki.vtiger.com](https://wiki.vtiger.com/index.php/CreatingExtensionModule)
- [Create a simple module for Vtiger CRM - zbeanztech.com](http://www.zbeanztech.com/blog/create-simple-module-vtiger-crm)
