---
layout: post
title:  "webhacking.kr old-01"
date:   2019-09-18
excerpt: "webhacking.kr"
tag:
- wargame
- webhacking
comments: true
---
~~~ php
<?php
  include "../../config.php";
  if($_GET['view-source'] == 1){ view_source(); }
  if(!$_COOKIE['user_lv']){
    SetCookie("user_lv","1",time()+86400*30,"./");
    echo("<meta http-equiv=refresh content=0>");
  }
?>
<html>
<head>
<title>Challenge 1</title>
</head>
<body bgcolor=black>
<center>
<br><br><br><br><br>
<font color=white>
---------------------<br>
<?php
  if(!is_numeric($_COOKIE['user_lv'])) $_COOKIE['user_lv']=1;
  if($_COOKIE['user_lv']>=6) $_COOKIE['user_lv']=1;
  if($_COOKIE['user_lv']>5) solve(1);
  echo "<br>level : {$_COOKIE['user_lv']}";
?>
<br>
<a href=./?view-source=1>view-source</a>
</body>
</html>
~~~

# Solve
1. 코드분석(해석?) -> 처음에 user_lv라는 값이 1인 쿠키 생성
2. user_lv 값이 6 이상이면 다시 1로 변환
3. user_lv 값이 5 초과이면 클리어
4. 5 < user_lv <= 6

## Tips 
* 크롬의 확장 프로그램에서 EditThisCookie를 사용하면 쿠키 값을 바꾸기 쉽다.