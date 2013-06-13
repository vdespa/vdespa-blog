# Translating the default value in a field (JForm / JField) #

When creating forms with JForm, you already know that attributes such as `label` or `description` get automatically a translation from Joomla.

But if you set for your form also the `default` value and it's nothing numerical and which is a language string, guess what will happen ... Yes, it will not be translated automatically. 

The trick is adding another property to your field, called `translate_default` and setting it's value to `1` or `true`. 

Here is a quick test (snippet is a hacked version of client.xml from the Banners component:

  	<field 
      name="name" 
      type="text" 
      class="inputbox"
			size="40" 
      label="COM_BANNERS_FIELD_CLIENT_NAME_LABEL"
			description="COM_BANNERS_FIELD_CLIENT_NAME_DESC"
			translate_default="true"
			default="JYES"
			required="true" />

If you replace the code above in `/administrator/components/com_banners/models/forms/client.xml` and go to Banners > Clients, you will get the field `Client name` already populated with the translated value `YES`.

This "core hack" is just a quick way to show you how this works. Now feel free to you this thing in your forms.




