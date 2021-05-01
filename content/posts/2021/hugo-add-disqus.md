---
title: "用 Disqus 幫 Hugo 加上留言板"
date: 2021-05-01T23:28:36+08:00
description: "記錄我如何幫 Hugo GitHub Page 加上留言板"
tags: [Blog]
---

賀！今天終於幫 Blog 加上留言板辣！

基本上需要做兩件事：

1. 註冊 Disqus
2. 將 Hugo 專案跟 Disqus 做連結

## 註冊 Disqus

先在 Disqus 完成註冊、設定

### 網址

[Disqus](https://disqus.com/)

### 步驟

![](https://i.imgur.com/Mt3Yt6R.png)

![](https://i.imgur.com/zH9C1gf.png)

`Website Name` 比較重要，記下來待會要用！

`Website Name` 比較重要，記下來待會要用！

`Website Name` 比較重要，記下來待會要用！

![](https://i.imgur.com/Mf5q5CN.png)

身為免費仔，直接選 `Basic`

![](https://i.imgur.com/ALkVdcF.png)

Hugo 還沒有在上面，選 `Universal Code`

![](https://i.imgur.com/a61P6uq.png)

![](https://i.imgur.com/p72Lyuc.png)

![](https://i.imgur.com/i9NfM0k.png)

這邊的 `Balanced`、`Strict` 可選、可不選

![](https://i.imgur.com/DahZhEW.png)

到這邊 Disqus 設定就完成惹！

![](https://i.imgur.com/VHYuYUB.png)

## Hugo 專案加東西

接下來，需要幫專案跟 Disqus 做連結

### config.toml

- `config.toml` 加上剛剛 Disqus 的 `Website Name`

```toml
# Disqus
disqusShortname = "test" # test 為剛剛註冊的 Website Name
```

### disqus.html

- `layouts/partials` 裡面新增 `disqus.html`

```html
<div id="disqus_thread"></div>
<script type="text/javascript">
  (function () {
    // Don't ever inject Disqus on localhost--it creates unwanted
    // discussions from 'localhost:1313' on your Disqus account...
    // if (window.location.hostname == "localhost") return;

    var dsq = document.createElement("script");
    dsq.type = "text/javascript";
    dsq.async = true;
    var disqus_shortname = "{{ .Site.DisqusShortname }}";
    dsq.src = "//" + disqus_shortname + ".disqus.com/embed.js";
    (
      document.getElementsByTagName("head")[0] ||
      document.getElementsByTagName("body")[0]
    ).appendChild(dsq);
  })();
</script>
<noscript
  >Please enable JavaScript to view the
  <a href="https://disqus.com/?ref_noscript"
    >comments powered by Disqus.</a
  ></noscript
>
<a href="https://disqus.com/" class="dsq-brlink"
  >comments powered by <span class="logo-disqus">Disqus</span></a
>
```

> 這邊需要注意的是，因為希望等等在本機測試可以顯示結果，所以我已經先把這段註解了

```
    // Don't ever inject Disqus on localhost--it creates unwanted
    // discussions from 'localhost:1313' on your Disqus account...
    // if (window.location.hostname == "localhost") return;
```

### single.html

- `layouts/_default/single.html` 裡面，在要顯示的位置補上

```html
<div class="disqus markdown">{{ partial "disqus.html" . }}</div>
```

以我的專案為例，我是加在底下這邊

```html
{{ define "main" }}
<main>
	<article>
		<div class="title">
			<h1 class="title">{{ .Title }}</h1>
			<div class="meta">Posted on {{ dateFormat "Jan 2, 2006" .Date }}{{ if .Draft }} <span class="draft-label">DRAFT</span> {{ end }}</div>
		</div>
		{{ if isset .Params "tldr" }}
		<div class="tldr">
			<strong>tl;dr:</strong>
			{{ .Params.tldr }}
		</div>{{ end }}

		<section class="body">
			{{ .Content }}
		</section>

		<div class="post-tags">
			{{ if ne .Type "page" }}
			{{ if gt .Params.tags 0 }}
			<nav class="nav tags">
				<ul class="tags">
					{{ range .Params.tags }}
					<li><a href="{{ "/tags/" | relLangURL }}{{ . | urlize }}">{{ . }}</a></li>
					{{ end }}
				</ul>
			</nav>
			{{ end }}
			{{ end }}
		</div>

        <!-- add disqus -->
		<div class="disqus markdown">
			{{ partial "disqus.html" . }}
		</div>
	</article>
</main>
{{ end }}
```

這些都設定好，在本機測試，理論上就會出現留言板，完成！

![](https://i.imgur.com/L60pqzJ.png)

## Disqus 其他功能

- 表情可以從 `Reactions` 這邊開啟

![](https://i.imgur.com/g1l0CSi.png)

## 後記

如果這篇對你有幫助，歡迎在下面留言讓我知道！！
