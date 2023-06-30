# Flag 0
### starting point
To start with you are greeted by a page with a text box used for commenting on the blog
On the blog you can see that "cody" mentions the site using PHP. So the first thing i did was write a statement that would use PHP code to echo back a message.
Knowing some basics of php i wrote
```
<?php echo "hello" ?>
```
When entered and click submit this would return flag 0

# Flag 1
Next i ran a dirsearch on the website to find any hidden directories or pages.
there i found the following interesting pages:
```
/admin.inc.php
```
Upon going to this site no useful information was gained so i moved on to inspecting the main site i already had access to.
In the source of this site was a hidden comment about a admin login at admin.auth.inc. when you go to ?page=admin.auth.inc you will see a admin login page.
I was stumpted on how to get the admin credentials so i took the hint.
#### Simple guessing might help you out
From this i started removing parts of the post name starting with the obvious one of "auth" and to my surprise that worked i now had admin access and the flag

# Flag 2

From flag 1 in my trials i got the error 
```

Warning: include(admin.inc.php.php): failed to open stream: No such file or directory in /app/index.php on line 21

Warning: include(): Failed opening 'admin.inc.php.php' for inclusion (include_path='.:/usr/share/php:/usr/share/pear') in /app/index.php on line 21
```
this shows us that its using the include command and pointing defaultly to index.php
when you enter a ?page= in the URL its automatically adding the .php onto the end
So first I try accessing index.php

```
 Fatal error: Allowed memory size of 134217728 bytes exhausted (tried to allocate 16384 bytes) in /app/index.php on line 20
```
is then shown to me meaning i cannot access it this way.
But if i use the localhost of the server i can likely access it this way using `?page=http://localhost/index`

When you do this it shows the comments box twice
This shows that the comments are integrated into the index.php which we can force to load.

With my basic knowledge of php i can get it to print the content of the php index file

```
<?php readfile("index.php");>
```
is the code i wrote for this purpose.
you then need to approve the comment through admin.inc and then reload the index.php through the method shown above
Upon this you will find the flag in the sourcecode of the website

