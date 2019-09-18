---
layout: post
title:  "webhacking.kr old-15"
date:   2019-09-18
excerpt: "webhacking.kr"
tag:
- wargame
- webhacking
comments: true
---
# Solve
1. 들어가면 Access_Denied가 뜨면서 메인페이지로 돌아감
2. 프록시툴(버프스위트)로 response 했을때 HTML 코드를 볼 수 있음
3. ?getFlag를 url에 추가하여 이동

~~~ html
<html>
<head>
<title>Challenge 15</title>
</head>
<body>
<script>
  alert("Access_Denied");
  location.href='/';
  document.write("<a href=?getFlag>[Get Flag]</a>");
</script>
</body>
~~~

