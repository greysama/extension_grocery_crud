extension_grocery_crud
======================

The proper way of extending Grocery CRUD

Quick Documentation: 
 
function field_type_ext( $field, $type, [$extras]):
 - Basically is the same as field_type but let's you set your own types without editing the GC library. For now, the only new type is 'yes_no' that is a dropdown.
 The idea it's to provide a clean and easy way to include personalized and recurrent field_types for the community... maybe join all the field_type extensions and plugins in just one...
 
 
function set_soft_delete([$database_field], [$deteled_value]): 
 - Overrides the delete function with a soft delete function (by default: table column 'deleted' setted to 1).
   If $database_field is set, it uses that one instead of 'deleted'
   If $deleted_value is set, it uses that insead of 1.

 
function append_fields($field, [$field2, ...]):
 - Same as fields() from GC with the difference that this one adds the fields passed by parameter to the end of the fields list (auto-eliminates duplicates).
 
function append_add_fields($field, [$field2, ...]):
 - Same as add_fields() from GC with the difference that this one adds the fields passed by parameter to the end of the fields list (auto-eliminates duplicates).
 
function append_edit_fields($field, [$field2, ...]):
 - Same as edit_fields() from GC with the difference that this one adds the fields passed by parameter to the end of the fields list (auto-eliminates duplicates).
 
 
 
function prepend_fields($field, [$field2, ...]):
 - Same as fields() from GC with the difference that this one adds the fields passed by parameter to the beginning of the fields list (auto-eliminates duplicates).
 
function prepend_add_fields($field, [$field2, ...]):
 - Same as add_fields() from GC with the difference that this one adds the fields passed by parameter to the beginning of the fields list (auto-eliminates duplicates).
 
function prepend_edit_fields($field, [$field2, ...]):
 - Same as edit_fields() from GC with the difference that this one adds the fields passed by parameter to the beginning of the fields list (auto-eliminates duplicates).
 
 
 
function append_fields_after($field, $field2, [$field3, ...]):
 - Appends the field2, [field3, ...] fields to the field list (add+edit) after $field. If $field doesn't exist it just appends them at the end of the list (auto-eliminates duplicates).
 
function append_add_fields_after($field, [$field2, ...]):
 - Appends the field2, [field3, ...] fields to the field list (add) after $field. If $field doesn't exist it just appends them at the end of the list (auto-eliminates duplicates).
 
function append_edit_fields_after($field, [$field2, ...]):
 - Appends the field2, [field3, ...] fields to the field list (edit) after $field. If $field doesn't exist it just appends them at the end of the list (auto-eliminates duplicates).
 
 
 
function remove_fields($field, [$field2, ...]):
 - Removes the fields passed as parameters from the fields list (add+edit) (if they are setted).
 
function remove_add_fields($field, [$field2, ...]):
 - Removes the fields passed as parameters from the add fields list(if they are setted).
 
function remove_edit_fields($field, [$field2, ...]):
 - Removes the fields passed as parameters from the edit fields list (if they are setted).
 
 
 
function append_columns($columns, [$columns2, ...]):
 - Appends columns passed as parameters at the end of the existing columns list (auto-eliminates duplicates).
 
function prepend_columns($columns, [$columns2, ...]):
 - Appends columns passed as parameters at the beginning of the existing columns list (auto-eliminates duplicates).
 
function remove_columns($columns, [$columns2, ...]):
 - Removes the columns passed as parameters from the columns list (if they are setted).
 

function callback_before_insert(array($this,'function_name'),[$override_all=0]):
- Same as GC original if override_all is set to 1. Else (default) adds the function to a callback array. The setted callbacks run one after the other in the order they where setted.

function callback_after_insert(array($this,'function_name'),[$override_all=0]):
- Same as GC original if override_all is set to 1. Else (default) adds the function to a callback array. The setted callbacks run one after the other in the order they where setted.

function callback_before_update(array($this,'function_name'),[$override_all=0]):
- Same as GC original if override_all is set to 1. Else (default) adds the function to a callback array. The setted callbacks run one after the other in the order they where setted.

function callback_after_update(array($this,'function_name'),[$override_all=0]):
- Same as GC original if override_all is set to 1. Else (default) adds the function to a callback array. The setted callbacks run one after the other in the order they where setted.

function callback_before_delete(array($this,'function_name'),[$override_all=0]):
- Same as GC original if override_all is set to 1. Else (default) adds the function to a callback array. The setted callbacks run one after the other in the order they where setted.

function callback_after_delete(array($this,'function_name'),[$override_all=0]):
- Same as GC original if override_all is set to 1. Else (default) adds the function to a callback array. The setted callbacks run one after the other in the order they where setted.


function callback_post_render(array($this,'function_name'),[$override_all=0]):
- Adds a callback function that runs right after the render() function.
 - Example: 
 	
<code>
	$gc->callback_post_render(array($this,'edit_body'));

	public function edit_body($output) {
		$output->output.='Lorem ipsum dolor sit amet, 
			consectetur adipisicing elit, sed do 
			eiusmod tempor incididunt ut labore et 
			dolore magna aliqua.'; 
		//This just adds the text to the end of the generated html
		return $output;
	}
</code>


-------------------------------------
 
Why should I use this?
 
So... why all this functions? The idea behind this is to allow the users to create some kind-of extensible configurations. I called them 'basic setups' but it's just a name.
Why a basic setup?
Let's say that almost all of the tables you use, have a creation date, a public field (if the content is or not visible in the public part of the site),a name and use soft_delete. 
 
You just go to your extension_grocery_crud library file (or another that inherits from it) and create something like this:
 
/* EXAMPLE OF BASIC SETUPS USE*/
 
    public function basic_gc_config($table_name, $content_public_name, $template='twitter-bootstrap'){
        $this->set_theme($template);
        
        $this->set_table($table_name)
            ->set_subject($content_public_name);
        $this->set_soft_delete();
        $this->columns('name','created','public');    
        $this->field_type_ext('public','yes_no');
        $this->required_fields('name');
        $this->fields(
            'name',
            'public'
        );
 
    }
 
(Don't worry it's in the attached file to)
 
So, when you go to your controller, you just call the library, make an instance (let's call it $c) and then $c->basic_gc_config('table_name', 'my_content');
After that you use the functions that the extension includes to add, move, remove, or adapt the columns and fields you need for that specific content type.
Of course, this is just a very minimal example of what you could pre-set in this "basic setups".
 
Also, fell completely free to add stuff to it, modify it, remove stuff from it and whatever you like. You may (if you want) post the modifications here or send them to my mail (greysama86@gmail.com - Only English or Spanish plz) and I promise to include them (as long as they are not repeated ) in future updates.
You may also publish your on "basic setups" if you want.
 
I will try to update it at least once or twice a month (if there is something to update). 
 
---------------------------------------------------------------------------------
 
How can I add stuff?
 
Well, you can use anything available at the GC documentation. You should also be able to use many of the internal GC library functions and variables, since this is an extension to the library itself.
 
---------------------------------------------------------------------------------
 
Instalation:
 
Just copy this file in your libraries folder and call it on your controller after GC.
 
Example:
 
$this->load->library('grocery_crud');
$this->load->library('extension_grocery_CRUD');


$crud = new Extension_grocery_CRUD(); //This goes INSTEAD of the "new Grocery_CRUD();"
 
With this, you will have all the GC functions plus the ones listed above.
 
---------------------------------------------------------------------------------
 
Hope you like this little extension. I am open to suggestions for any functionality you may want to add, modify or whatever.
 
Bye!
 
VERY IMPORTANT: This works "out of the box" with the last GitHub version of GC. If you want to use it with 1.3.3 (stable) you only need to change the $columns variable from private to protected on the GC library file. You may use this extension even without that, but all the columns related functions will not work and even crash 
