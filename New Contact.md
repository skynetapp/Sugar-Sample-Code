#### Date: 04-04-2017
#### Description: This document explains the code flow of creating a contact using sample code.

#### The Folder Structure is as follows:
   
   
   Root Directory | Description
------------ | -------------
nusoap | Folder |
temp | stores the WSDL Cache | 
serverlog | saves the log file start time and end time |
cacheWsdlCreateContact | Creates a new contact |

#### Step 1:

Soap xml code which gets from wsdl url - https://dev2.lytepole.com/sales/soap.php?wsdl. copy the entire xml code and save it as .xml file.

**_Code:_**
	
```
$url="somename.xml";

```

#### Step 2:

Require nusoap library folder which we already have, or we can get in online also.

**_Code:_**
	
```
	require_once("nusoap/lib/nusoap.php");
	require_once("nusoap/lib/class.wsdlcache.php");
  
  ```
  
  #### Step 3:
  
  Logging class initialization.
  
  **_Code:_**
	
```
Ex :	require_once("Logging.php");
	$log = new Logging();
```

#### Step 4:

Set the path for log file and store the wsdl cache file.

First nusoap try to get the cache file, if cache file not exist it uses the wsdl url and save the cache file in given path


  **_Code:_**
	
```
Ex : 	$log->lfile('/var/www/html/soapws/mylog.txt');
	$cache = new wsdlcache('/var/www/html/soapws/temp/', 12000);

	$wsdl = $cache->get($url );
	if (is_null($wsdl)) {
		$wsdl = new wsdl($url );
		$cache->put($wsdl);
	}
  
 
```

#### Step 5:

We have to login with crediantials, user name and password

**_Code:_**
	
```
	$username = "19951XXXXXX"; (user phone number with country code)
  	$password = "6ae119e6d4*******************"; (md5 converted password)
```

#### Step 6:

Initialise nusoap_client which will accept a WSDL file.

**_Code:_**
	
```
$client = new nusoap_client($wsdl,true);

```

#### Step 7:

Write message to log file which reports the start time and add the login parameters to WSDL call.

- Here pass username and password to WSDL call **login** for user login.

**_Code:_**
	
```
$log->lwrite('Start Time');
    
$login_parameters = array(
     'user_auth' => array(
          'user_name' => $username,
          'password' => $password,
          'version' => ''
     ),
     'application_name' => ''
        
);
    
$login_result = $client->call('login', $login_parameters);
```

#### Step 8:

Get the session id from login result and pass the session id to WSDL call **get_user_id** to get the login user id


**_Code:_**
	
```
$session_id =  $login_result['id'];

$param_array =array('session'=>$session_id,);
$user_id = $client->call("get_user_id", $param_array);

```

#### Step 9:

Creating the parameters for create new contact and pass to WSDL call **set_contacts_acc**.

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
		
	$set_entry_result = $client->call('set_contacts_acc',array('session'=>$session_id,														   'user_id'=>$user_id,
                                                                   'created_by'=>$user_id,														   'assigned_user_id'=>$user_id,
                                                                   'name_value_lists'=>array('name_value_list'=>$params)
                                                                 )
					 );

echo $log->lwrite('End Time');
 ```
 


#### Errors

- If the xml code is not correct, error will be shown.
- If the login credentials i.e username and password is not correct, output will not be shown.
- If the parameters are not given correctly, contact will not be created.



