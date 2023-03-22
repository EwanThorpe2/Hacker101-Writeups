# Flag 0
I got started off with a hint which was
"the username "user" has a easy password
I knew it had to be either password or pass which it was revealing the first flag

# Flag 1
Next i went to inspect the posts and noticed the ID was in the url
i changed this from 3 to 2 and it revealed the next flag. So far pretty easy

# Flag 4
I did the same on the edit page till i found the flag

# Flag 2
I took out another hint which pointed towards the form on the edit page
in the form there is a line of code which is the following
```<input type="hidden" name="user_id" value="2">```
Changing the user_id could allow for posting as another member of the page
in this case i set it to 1 and posted as the admin revealing the key

# Flag 3
I once again took a hint and got 189*5
I wasnt sure what this meant for awhile so was very confused
It then occured to me it could be a id for a post so entered 189*5 and nothing came up
So i did the maths and entered 945 and sure enough was given the flag

# Flag 4
For flag 4 i was abit confused till i saw a video on cookies and i wondered if the site stored the user id in the cookies
Sure enough when you login a ID is stored as a cookie.
I took the value and tried to decode as hex but got no worthwhile results
I then tried to do it as a hash and tried md5 
It came back as a value of 2
I then entered the MD5 value for 1 to try login as admin
And it worked and gave me the flag

# Flag 5
Getting this flag took a while of playing around with the different functions of the site and burpsuite intercepts
when deleting a page i noticed the ID was similar to that of flag 4 so i entered the same value i did for flag4
When i put the request through it deleted a post not belong to the user i was signed in as and gave me the flag.

# Conclusion
I learned alot about using burpsuite this CTF which is invaluable knowledge to have
I also learned some new skills at identifying exploitable cookies
