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


