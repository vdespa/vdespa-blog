# Different pagination style in frontend vs. backend using JPagination #

If you use the method `getListFooter()` from the `JPagination` class, you may encounter a problem if you use this method also in the frontend of your website, most likely inside a component.

Actually Joomla calls different HTML templates to generate the HTML do display the pagination. In the backend it calls the `pagination.php` file from the admin template (e.g. `administrator/templates/bluestork/html/pagination.php`) while in frontend it will call the `/templates/CURRENT_TEMPLATE/html/pagination.php` and if it does not find it, it will just display some html. If it cannot find it, it will use `_list_footer()` method, from the same class.

**To check which pagination.php file is calling**, have a look inside:

`/libraries/joomla/html/pagination.php inside the method: getListFooter()`

and `echo` / `var_dump` the variable `$chromePath` to see the value of it.

**If you are looking to get the same style from backend**, just copy the `pagination.php` file from the admin template and put it in your site template. Don’t forget to include the admin template css file (e.g. `administrator/templates/bluestork/css/template.css`).
