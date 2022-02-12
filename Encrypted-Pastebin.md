# Flag 0
I started on the website by creating a post on the page with special characters to see if could get any info back about SQL this proved to show no results
Finding this flag took me getting the hint **What are these encrypted links?** once I knew to look at the link i started looking at the url more closely

```?post=qnDlHhTCJ1Mt6id!FYCl8HsBQttHUickn5GMtjOKoHU404ahDVSxmkh-ZXH3cPJZdIqOWM4wnYbjmuTEE3KUKXcww!KIr0VNjK1oLSHP6CoJtQ0o!Kmt17x0uBwMREI47QjYY5-k!GrdhitIcexM7tLT-ditXMF7rc!IeTrbrGYyegw6BfWts!NbfTupMYHDJbqDV3Ue9yLWh8kKCOSnfg~~**```

Finding how the pages were encrypted was my next step I started by entering my own data into the url
I entered **?post=1** into the url and was met with
```py
Traceback (most recent call last):
  File "./main.py", line 69, in index
    post = json.loads(decryptLink(postCt).decode('utf8'))
  File "./common.py", line 46, in decryptLink
    data = b64d(data)
  File "./common.py", line 11, in <lambda>
    b64d = lambda x: base64.decodestring(x.replace('~', '=').replace('!', '/').replace('-', '+'))
  File "/usr/local/lib/python2.7/base64.py", line 328, in decodestring
    return binascii.a2b_base64(s)
Error: Incorrect padding
``` 
And flag0 

# Flag 1

Because the above error we know we can use a padding oracle attack to decrypt the ciphertext to plain text.

