# Overriding a Field in a Form Using JFrom #

So if you are generating you forms from XML files using JForm, you may want a point to reuse a form. Perfectly possible. But what if you need a small change, let's say, making a field mandatory? Or reducing some options in a list?

JForm provides a nice way to do that. You just need to load create a new XML form that matches the field you want to override.

I am going to illustrate this trick directly on a Joomla! core component, just for the sake of example. 

Inside `root/administrator/components/com_banners/models/forms` you will find `banner.xml`. Make a copy of it and name it `banner_override.xml`.

Now we are going to override the field `state`, so the xml will look like:

    <?xml version="1.0" encoding="utf-8"?>
    <form>
      <fieldset name="details"
    		addfieldpath="/administrator/components/com_banners/models/fields"
    	>
    		<field name="state" type="list"
    			label="JSTATUS" description="COM_BANNERS_FIELD_STATE_DESC"
    			class="inputbox span12 small" size="1" default="1" >
    			<option value="1">JPUBLISHED</option>
    			<option value="0">JUNPUBLISHED</option>
    		</field>
    	</fieldset>
    </form>
  
with the difference that it contains fewer options. Of course, changing other options are possible as well.

Now we need to alter the model, to take this change into account. We need to override the method `preprocessForm(...)` in `JModelForm`.

    /**
  	 * Method to preprocess the form.
  	 *
  	 * @param   JForm   $form    A JForm object.
  	 * @param   mixed   $data    The data expected for the form.
  	 * @param   string  $group   The name of the plugin group to import.
  	 *
  	 * @return  void
  	 *
  	 * @see     JFormField
  	 * @since   1.6
  	 * @throws  Exception if there is an error in the form event.
  	 */
  	protected function preprocessForm(JForm $form, $data, $group = 'content')
  	{
  		$path = JPath::clean(JPATH_ADMINISTRATOR.'/components/com_banners/models/forms/banner_override.xml');
  		
  		if (file_exists($path)) {
  			if (!$form->loadFile($path, true)) {
  				throw new Exception(JText::_('JERROR_LOADFILE_FAILED'));
  			}
  		}		
  		
  		parent::preprocessForm($form, $data, $group);
  	}
    
And that's about it. Load try to create a new banner and the form will have a field Status with just two options.

*Please note* I am showing this example in a core component just for the sake of example. Use this in your own custom components but do not modify core components using this method. 
