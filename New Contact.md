**_Code:_**
	
```
$session_id =  $login_result['id'];

$param_array =array('session'=>$session_id,);
$user_id = $client->call("get_user_id", $param_array);

```

#### Step 9:

Creating the parameters and passing the parameters to WSDL call **set_contacts_acc**.

- Here log time reports the end time in the log sheet.

**_Code:_**
	
```
$params=array(
								array('name'=>'ContactId', 'value'=>''),   
                                array('name'=>'first_name', 'value'=>'MaheshTest'),     
								array('name'=>'last_name', 'value'=>'Test111'),
								array('name'=>'fax_home', 'value'=>''),   
								array('name'=>'email', 'value'=>'mahesh.pattepu@skynetappdev.com'),
								array('name'=>'Description', 'value'=>''),   
								array('name'=>'account_sugar_id', 'value'=>''),   
								array('name'=>'serial_number', 'value'=>''),      
								array('name'=>'deleted', 'value'=>'0'),   	                               
								array('name'=>'phone_home_group', 'value'=>''),  
								array('name'=>'phone_work_group', 'value'=>''), 
								array('name'=>'phone_mobile_group', 'value'=>'918099617566'), 
								array('name'=>'phone_main_group', 'value'=>''), 
								array('name'=>'phone_iphone_group', 'value'=>''), 
								array('name'=>'blocked', 'value'=>'0'),                               
                         );
						 
		
		$set_entry_result = $client->call('set_contacts_acc',array('session'=>$session_id,
																	'user_id'=>$user_id,
                                                                    'created_by'=>$user_id,
																	'assigned_user_id'=>$user_id,
                                                                   	'name_value_lists'=>array('name_value_list'=>$params)
                                               ));

echo $log->lwrite('End Time');
 ```
 


#### Errors

- If the xml code is not correct, error will be shown.
- If the login credentials i.e username and password is not correct, output will not be shown.
- If the parameters are not given correctly, contact will not be created.



