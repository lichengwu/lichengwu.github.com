---
layout: page
title: 首卷语
tagline: stay hungry stay foolish

---


<div style="margin:0 auto;">
  <div style="float:right; width:280px;">
    <h3>最新文章</h3>
    <hr />
    <ul class="tag_box inline">
	{% for post in site.posts limit:10 %}
	<p><a href="{{post.url}}">{{ post.title }}</a></p>
	{% endfor %}
	</ul>
  </div>
  <!-- content -->
  <div style="float:left;">
  	<h3>送给在路上的人，共勉</h3>
  	
  	<img src="http://i1298.photobucket.com/albums/ag53/lichengwu/4b17b700-4e87-41e6-9d4b-f71a121d2b3b_zpsb974369f.jpg" >
  	
  	<p>这个博客是我从大学到毕业后，到现在的技术积累。现在看来，有些技术已经过时，有些想法显得幼稚；<br />但是正是这些见证了我的成长。</p>
  	<p>我一直在路上...</p>
  </div>
</div>


	





{% include JB/setup %}


