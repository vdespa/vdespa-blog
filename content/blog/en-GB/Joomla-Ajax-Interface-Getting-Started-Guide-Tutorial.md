# Joomla Ajax Interface - Getting started #

## Introduction to the Ajax Interface ##

Joomla 3.2 introduces for developers, among other juicy features, a new  ajax interface, called "Joomla Ajax Interface". 

This interface is **implemented as a component `com_ajax`** and the purpose of this new feature is to easily make Ajax request in Joomla! without spending to much on the technical part.

There are **two ways to work with the Joomla Ajax Interface:**

- using a module
- using a plugin

A simple request URL can look like this:

`http://www.example.com/index.php?option=com_ajax&plugin=mycontent&format=json`

## Request parameters explained ##

Below are explained the parameters available.

- **option=com_ajax** (required) is the entry point for the request and is mandatory you will always need it
- **[module|plugin]=name** (required) is the point where you select what extension type to call (it can only be a module or a plugin) and what is the name of that extension (without mod_ or plg_ prefix). This option is mandatory as well.
- **format=[json|debug|raw]** (optional) The default is a raw output. Using `format=json` will encode your result as JSON and will set the `Content-Type` to `application/json`. The debug option will return a user friendly / easy to read result for debugging purposes.
- **method=[name]** (optional and just for modules). The default is `get` and this results in the method `getAjax` being called. The suffix `Ajax` will always be added.
- s 

