---
title: Tzrlk
showtitle: true
page-liquid: true
page-description: My personal site
---

# {{ page.title }}

<div itemscope itemtype="http://schema.org/Blog">
	{% for post in site.posts %}
		<hr/>
		<article class="post" itemprop="blogPost" itemscope itemtype="http://schema.org/BlogPosting">

			<h2>
				<a class="post-link" href="{{ post.url | prepend: site.baseurl }}" itemprop="url">
					<span itemprop="name">{{ post.title }}</span>
				</a>
			</h2>

			<p class="post-meta">
				<a href="{{ site.baseurl }}/about" itemprop="author">{{ post.author }}</a>
				<time itemprop="datePublished" datetime="{{ post.date | date_to_xmlschema }}">
					{{ post.date | date: "%-d %b, %Y" }}
				</time>
				<span itemprop="keywords">{{ post.tags | join: ',' }}</span>
			</p>

			<div class="entry" itemprop="description">
				<p>{{ post.excerpt }}</p>
				{{ post.content }}
			</div>

		</article>
	{% endfor %}
</div>