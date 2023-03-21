# Flag 0
When making a new page you will notice that the page number does not go up in incremental steps from the other pages suggesting there is hidden pages.

in this example my pages started at 9 ```/page/9```

after i noticed this i went and entered /page/4 where i was greeted by a forbidden page

when you attempt to edit this page you are greeted with **flag 0**

# Flag 1
When viewing the source of the edit page you can see a ```<form method="POST">``` which suggests some form of query taking place when you post the page

I also noticed that the page ID was in the URL and this could be exploitable

to test if it was vulnerable i put a single quote mark at the end of the URL

And it displayed **flag 1** which indicates a successful SQL vulnerability

# Flag 2
I noticed that it said scripts were not allowed in the body of the pages when you create them.

But it did not mention if they were allowed in the name of the page so i put a <script> </script> in the title name.

and surely enough when you return to the homepage the script will execute and you are given **flag 2**

# Flag 3
For this flag i required some help from the hints which pointed me towards a XSS attack unlike flag 2 it would need to be in the body of the page.

Meaning i would need to find a way to bypass the script check that occurs.

I used the button on page2 to enable me to add a onclick event which successfully executed.

I was abit confused when viewing the page as no flag appeared but upon inspection of the source the flag was revealed

Therefore completing Micro-CMS V1 and obtaining the final flag **flag 3**
