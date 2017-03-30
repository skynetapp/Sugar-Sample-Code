#### Date: 03-30-2017
#### Description: This document explains the code flow of meeting list using sample code.

#### Step 1:

Soap xml code which gets from wsdl url - https://dev2.lytepole.com/sales/soap.php?wsdl. copy the entire xml code and save it as .xml file.

**_Code:_**
	
```
$url="devsoapserver.xml";

```

#### Step 2:

Require nusoap library folder which we can get in online also.

**_Code:_**
	
```
	require_once("nusoap/lib/nusoap.php");
	require_once("nusoap/lib/class.wsdlcache.php");
  
  ```
  
  #### Step 3:
  
  Logging class initialization.
  
  **_Code:_**
	
```
require_once("Logging.php");
$log = new Logging();
```

#### Step 4:

Set the path and name of log file.

  **_Code:_**
	
```
$log->lfile('/var/www/html/soapws/mylog.txt');
$cache = new wsdlcache('/var/www/html/soapws/temp/', 12000);

	$wsdl = $cache->get($url );
	if (is_null($wsdl)) {
	$wsdl = new wsdl($url );
	$cache->put($wsdl);
	}
  
 
```

#### Step 5:

Dev login phone number and password which is converted to md5.

**_Code:_**
	
```
 $username = "918099617565";
  $password = "****************************";
```

#### Step 6:

Initialise nusoap_client which will accept a WSDL file.

**_Code:_**
	
```
$client = new nusoap_client($wsdl,true);

```

#### Step 7:

Write message to log file which reports the start time and add the login parameters to WSDL call.

- Here login parameters are created and passed to WSDL call **login**.

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

Get the session id and pass the session id to get the login user id.

- Create the parameters session id in array and pass the parameters to WSDL call **get_user_id**.

**_Code:_**
	
```
$session_id =  $login_result['id'];

$param_array =array('session'=>$session_id,);
$user_id = $client->call("get_user_id", $param_array);

```

#### Step 9:

Creating the parameters and passing the parameters to WSDL call **get_entry_list_meetings_web**.

- Here log time reports the end time in the log sheet.

**_Code:_**
	
```
$get_entry_list_parameters = array('session'=>$session_id,
                            'userid'=>$user_id,
                            'user_phone'=>'918099617565',
                            'query'=>"meetings.name !='' AND meetings.event_type = 'Discussion' and meetings.access_type='Private' ",
                            'order_by'=>'date_entered desc',
                            'offset'=>'',
                            'select_fields'=>'',
                            'max_results'=>'10',
                            'deleted'=>'0',
							
                            );

$set_entry_result = $client->call('get_entry_list_meetings_web', $get_entry_list_parameters);

echo $log->lwrite('End Time');
 ```
 
#### Step 10:

Results shown in a table based on the output array generated.

**_Code:_**

```
<table cellpadding="5" cellspacing="5" align="center" width="70%" bgcolor="#fff" style="border:1px solid #CCCCCC; margin:5px;">
  <tr>
    <td>Date</td>
    <td>Name</td>
  </tr>
  <?php  foreach($set_entry_result['entry_list'] as $key=>$val)
	{?>
  <tr>
    <td><?php echo $val['name_value_list'][2]['value'];?></td>
    <td><?php echo $val['name_value_list'][1]['value'];?></td>
  </tr>
  <?php }?>
</table>

```


#### Errors

- If the xml code is not correct, output will not appear.
- If the login credentials i.e username and password is not correct, output will not be shown.
- If the query is wrong, results will not appear..



