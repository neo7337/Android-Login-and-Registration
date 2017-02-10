# Android-Login-and-Registration
i am going to show you step by step on how to create a login and registration android app and connect it to the local xampp server

Hello Everyone 
This tutorial is for the people who have the working knowledge of android studio 

First of All to connect your app with the localhost server you need to install the server on your system, you can go with the xampp server as I am using that server.The xampp server contains the mysql and tomcat and many ohter features in built, for this project we require only the tomcat and mysql
If you don't know the usage of xampp go to he official site : https://www.apachefriends.org/index.html
I walso wanna let you know that we will be using the PHP as well so the latest xampp version uses the mysqli version of code and the xampp version 3.2.1 uses the mysql code in php for database connectivity.
Hence, change your code respectively otherwise the code won't work

Step 1:
Create your database on phpmyadmin
or use the below script to create the databse
CREATE TABLE IF NOT EXISTS 'users' (
'id' int(20) NOT NULL AUTO_INCREMENT,
'username' varchar(70) NOT NULL,
'password' varchar(40) NOT NULL,
'email' varchar(50) NOT NULL,
PRIMARY KEY ('id'),
UNIQUE KEY 'email' ('email')
);

Once the database is created now write the php script
Step 2:

The following php script is used to do the login and registration actions

user.php

<?php
$link=mysql_connect("localhost","root","");
mysql_select_db("androidlogin",$link);

class User{
	
	private $db;
	private $db_table = "users";
	
	public function isLoginExist($username, $password){		
				
		$query = "select * from " . $this->db_table . " where username = '$username' AND password = '$password' Limit 1";
		$row=mysql_query($query);
        $data=mysql_fetch_array($row);
		
		if($data>0)
            {
              	return true;
           	}
           	else
            {
				return false;
            }		
	}
	
	public function createNewRegisterUser($username, $password, $email){
			
		$query = "insert into users (username, password, email, created_at, updated_at) values ('$username', '$password', '$email', NOW(), NOW())";
		
		$row=mysql_query($query);
		if($row>0)
            {
              	$json['success'] = 1;	
           	}
           	else
            {
				$json['success'] = 0;
            }
		return $json;
	}
	
	public function loginUsers($username, $password){
			
		$json = array();
		$canUserLogin = $this->isLoginExist($username, $password);
		if($canUserLogin){
			$json['success'] = 1;
		}else{
			$json['success'] = 0;
		}
		return $json;
	}

}
?>

index.php

<?php

require_once 'include/user.php';

$username = "";
$password = "";
$email = "";

if(isset($_POST['username'])){
	$username = $_POST['username'];
}
if(isset($_POST['password'])){
    $password = $_POST['password'];
}
if(isset($_POST['email'])){
	$email = $_POST['email'];
}

// Instance of a User class
$userObject = new User();

// Registration of new user
if(!empty($username) && !empty($password) && !empty($email)){
	$hashed_password = md5($password);
	$json_registration = $userObject->createNewRegisterUser($username, $hashed_password, $email);
	
	echo json_encode($json_registration);
}

// User Login
if(!empty($username) && !empty($password) && empty($email)){
	$hashed_password = md5($password);	
    $json_array = $userObject->loginUsers($username, $hashed_password);

    echo json_encode($json_array);
}

?>

once you have written the scripts and saved in the folder inside htdocs they are ready to go
but keep in mind that I am using the mysql format for database queries if you are working in the mysqli format do change it otherwise the script won't work

Step 3:
create your RegisterActivity.java file inside the src folder of your app

Step 4: 
similarly create your MainActivity.java and LoginActivity.java files

all these files can be downloaded from above

Step 5:
create the layout file activty_main.xml, activity_login.xml and activity_register.xml from the above given files

Similarly give create the strings.xml 

now at the end it is most important to include the line inside your manifest file
<uses-permission android:name="android.permission.INTERNET" />

the above is responsible for your app to connect through internet to your localhost

and keep in mind that the device and the machine on which the local host is installed must be using the same network.
