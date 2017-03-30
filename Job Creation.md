#### Date: 03-30-2017
#### Description: This document explains the code flow of meeting list using sample code.

#### The Folder Structure is as follows:
   
   
   Root Directory | Description
------------ | -------------
nusoap | Folder |
temp | stores the WSDL Cache | 
serverlog | saves the log file start time and end time |
cachewsdl | Gives the results of meeting list |

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
$params=array(
        array('name'=>'name','value'=>'MaheshJobTest'),	
        array('name'=>'description','value'=>'Test Job16022017'),
        array('name'=>'date_start', 'value'=>'2017-02-16 06:46:19'),							
        array('name'=>'duration_hours', 'value'=>'0'),
        array('name'=>'duration_minutes', 'value'=>'0'),	
				array('name'=>'assigned_user_id', 'value'=>$user_id),										
        array('name'=>'location', 'value'=>'Achille,OK'),	
				array('name'=>'company', 'value'=>'skynet new'),		
        array('name'=>'max_invitee', 'value'=>'5000'),	
        array('name'=>'invitee_c_invite', 'value'=>'1'),
				array('name'=>'event_type', 'value'=>'Discussion'),
        array('name'=>'access_type', 'value'=>'Private'),	
        array('name'=>'email', 'value'=>'mahesh.pattepu@skynetappdev.com'),	
				array('name'=>'experience', 'value'=>'3'),
				array('name'=>'referral_fee', 'value'=>''),
				array('name'=>'meeting_empoptions', 'value'=>'Full Time,Telecommute,Student,EAD,GC'),
				array('name'=>'meeting_skills', 'value'=>'Methodology,Product Reliability'),
				array('name'=>'meeting_emptype', 'value'=>'Hourly Rate:300,Yearly Rate:2000,Additional Compensation Details:testing'),
				array('name'=>'meeting_invitees', 'value'=>'name:Docomo  #mname:#lname:#phone:919030187616#email:kushalp77@gmail.com#phone_actual:919030187616#to_name:Docomo  #from_name:'),
						
                            );
		$set_entry_result = $client->call('set_entry',array(
                                                         'session'=>$session_id,
                                                         'module_name'=>'Meetings',
                                                         'name_value_list'=>$params)
                                               );

echo $log->lwrite('End Time');
 ```
 


#### Errors

- If the xml code is not correct, error will be shown.
- If the login credentials i.e username and password is not correct, output will not be shown.
- If the parameters are not given correctly, contact will not be created.





