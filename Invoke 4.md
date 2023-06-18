Starting off with a port scan we see the following:

![[Pasted image 20230618001410.png]]

Going to port 80 we see the following:

![[Pasted image 20230618001512.png]]

Under socials is a web address, going to that we can get some emails from linked in:

![[Pasted image 20230618001858.png]]

Putting those names in a file we can try to see if anyone has pre-auth not required, but first we need the domain name:

![[Pasted image 20230618002113.png]]

![[Pasted image 20230618002155.png]]

We have a hash lets try and crack it:

![[Pasted image 20230618002406.png]]
That user does not have remote management so we will have to look at the SMB share:

![[Pasted image 20230618002617.png]]

We see a Reminder.pdf. lets open that up and see if we find anything:

![[Pasted image 20230618002710.png]]

Looks like it is password locked, lets you john to change it into a format that john can read and try to crack the password:

![[Pasted image 20230618002819.png]]

Weird, we did not see an AeroCMS and there is nothing else there. However we do know that carrot has been in the share, lets try and utilize SMBKiller to see if we can get a hash through the share:

![[Pasted image 20230618002923.png]]

https://github.com/overgrowncarrot1/Invoke-Everything/blob/main/SMB_Killer.py

![[Pasted image 20230618003436.png]]

```
python SMB_Killer.py -r 192.168.0.46 -l 192.168.0.26 -i tun0 -U alice.wonderland -P 'MyP@ssw0rd!' -a Share -A -d invoke.local
```

From here Responder automatically runs:

![[Pasted image 20230618003518.png]]

From here we get a hash back:

![[Pasted image 20230618004308.png]]


![[Pasted image 20230618004459.png]]

We get his password. Lets login with him:

![[Pasted image 20230618004656.png]]

Now we can look for that CMS that was talked about earlier in Reminder.pdf:

![[Pasted image 20230618004746.png]]

Port forwarding we can use ssh because port 22 was open on the machine:

![[Pasted image 20230618005003.png]]

And before we forget lets grab the user.txt hash

![[Pasted image 20230618005242.png]]

![[Pasted image 20230618005318.png]]

Since we port fowarded it, we should also be able to get into PHPMyAdmin

![[Pasted image 20230618005416.png]]

Here we can see the login for aerocms for carrot.wonderland

From here a directory brute force shows aerocms is called aero:

![[Pasted image 20230618005559.png]]

When trying to crack the hash we just obtained we cannot, lets try password resuse:

![[Pasted image 20230618005747.png]]

And we login 

![[Pasted image 20230618005934.png]]

Now lets make a new user and add a php reverse shell for a picture, remember you need a windows php reverse shell found here https://github.com/ivan-sincek/php-reverse-shell/blob/master/src/reverse/php_reverse_shell.php

Change line 174 to reflect your IP address and listening port, then start a listener

![[Pasted image 20230618010328.png]]

![[Pasted image 20230618010359.png]]


![[Pasted image 20230618010227.png]]

Click add user then go to users view users and you get a call back 

![[Pasted image 20230618010726.png]]

![[Pasted image 20230618010738.png]]

![[Pasted image 20230618010902.png]]


