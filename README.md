Redmine sql custom field
==================
This plugin add two sql format for custom fields
* **sql** - format for simple sql-expression.
* **sql_search** - format for search sql query with form parameters

Compatibility
-------------
* Redmine 5.0 or higher

Installation
----------------------
* Clone or [download](https://github.com/apsmir/custom_field_sql/archive/main.zip) this repo into your **redmine_root/plugins/** folder

```
$ git clone https://github.com/apsmir/custom_field_sql.git
```
* If you downloaded this repo, make sure to rename the extracted folder to `custom_field_sql`
* Restart Redmine

Usage
----------------------
1) Visit **Administration->Custom fields**. 
2) Press the button **New custom field**. Select format **Sql** or **Sql search**.
3) Enter sql query 

SQL fields and parameters
----------------------
You can use parameters for sql expression.
This may be id of issue %{issue_id} or id of project %{project_id}

You can use any form  values as query parameter.
`p0='%'+$('#issue_custom_field_values_31').val()+'%'`

where

p0 - parameter name

%'+$('#issue_custom_field_values_31').val()+'% - any jquery expression to calculate parameter value

**sql_search** 
Query must have field 'value'. This field used be as field value.
format: support multiply forms parameters. Parameters must be written in jquery. 

----------------------
Example 1:

 "sql expression": 
 
 `select subject as value, description as label from issues where subject like '%{p0}' and description like '%{p1}'`
 
 "sql form params":
 
`p0='%'+$('#issue_custom_field_values_31').val()+'%'`
`p1='%'+$('#issue_custom_field_values_30').val()+'%'`

----------------------
Simple 2 (for MySQL):

 "sql expression": 
 
 `select subject as value from issues where id = if( ? ='new', id, ?);`
 
 
 "sql form params":
 
`p0=window.location.toString().split('/').pop()`

`p1=window.location.toString().split('/').pop()`


This expression `window.location.toString().split('/').pop()` calculate **issue id** on form. For new issues calculated value = 'new'.

----------------------

Query in **sql search** field can be executed by mouse click. Use parameter "search by click" in settings page.

Default value
----------------------
**sql_search** -this  format support sql-query for calculate  default value . This query select initial custom field value for new issue from database.

Query can use parameters
* %{tracker_id}
* %{project_id}

Scripts
----------------------
view_customize/custom_field_autselect_first_value.js
It is script for plugin "view customize" https://www.redmine.org/plugins/view_customize
The script allows you to automatically select the first value for a custom field (drop-down list) 

Uninstall
----------------------
1) Delete all custom fields with format Sql.
2) Remove folder **redmine_root/plugins/custom_field_sql**
3) Restart Redmine
