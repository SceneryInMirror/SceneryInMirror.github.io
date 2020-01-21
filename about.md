---
title: About
layout: page
---
<!--
![Profile Image]({{ site.url }}/{{ site.picture }})
-->


**Welcome to my blog!**

Hi! My name is 杨凯欣 (Kaixin Yang), a Ph.D. student from Viterbi School of Electrical and Computer Engineering, University of Southern California. For research, now I focus on the topic about security of hardware and sequential logic encryption methods.

I love all kinds of sports, especially soccer and basketball. I am a big fan of Cristiano Ronaldo and Kobe Byrant. Also, if you love photograph, maybe we can have some discussion!

Now I live in Los Angeles, a famous and beautiful city. In my blog, you will find something about my life here, as well as my learning of computer technique. If you share the same interest, please contact me!

<div class="
col-lg-8 col-lg-offset-2
col-md-10 col-md-offset-1
sidebar-container">
<!-- Friends Blog -->
{% if site.friends %}
<hr>
<h5>FRIENDS</h5>
<ul class="list-inline">
{% for friend in site.friends %}
<li><a href="{{friend.href}}">{{friend.title}}</a></li>
{% endfor %}
</ul>
{% endif %}
</div>
