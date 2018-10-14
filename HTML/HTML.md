<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

# HTML

- [HTML5 主要功能](#html5-%E4%B8%BB%E8%A6%81%E5%8A%9F%E8%83%BD)
- [HTML5 部分新标签](#html5-%E9%83%A8%E5%88%86%E6%96%B0%E6%A0%87%E7%AD%BE)
- [常用 Meta 标签](#%E5%B8%B8%E7%94%A8-meta-%E6%A0%87%E7%AD%BE)
- [Cookie, LocalStorage 与 SessionStorage](#cookie-localstorage-%E4%B8%8E-sessionstorage)
- [Canvas](./Canvas.md)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## HTML5 主要功能
- 语义化标签：能够让你更恰当地描述你的内容是什么。
- 离线 & 存储：能够让网页在客户端本地存储数据以及更高效地离线运行。
- 多媒体：使 video 和 audio 成为了在所有 Web 中的一等公民。
- 2D/3D 绘图 & 效果：提供了一个更加分化范围的呈现选择。
- 性能 & 集成：提供了非常显著的性能优化和更有效的计算机硬件使用。

## HTML5 部分新标签

[HTML5 标签列表](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML5/HTML5_element_list)

| 标签 | 描述 |
| :- | :- |
| `<template>` | 通过 JavaScript 在运行时实例化内容的容器。 |
| `<header>` | 定义页面或章节的头部。它经常包含 logo、页面标题和导航性的目录。 |
| `<footer>` | 定义页面或章节的尾部。它经常包含版权信息、法律信息链接和反馈建议用的地址。 |
| `<main>` | 定义文档中主要或重要的内容。 |
| `<section>` | 定义文档中的一个章节。 |
| `<nav>` | 定义只包含导航链接的章节。 |
| `<article>` | 定义可以独立于内容其余部分的完整独立内容块。 |
| `<aside>` | 定义和页面内容关联度较低的内容——如果被删除，剩下的内容仍然很合理。 |
| `<video>` | 代表一段视频 及其视频文件和字幕，并提供了播放视频的用户界面。 |
| `<audio>` | 代表一段声音 ，或音频流 。 |
| `<source>` | 为 `<video>` 或 `<audio>` 这类媒体元素指定媒体源 。 |
| `<track>` | 为 `<video>` 或 `<audio>` 这类媒体元素指定文本轨道（字幕） 。 |
| `<canvas>` | 代表位图区域 ，可以通过脚本在它上面实时呈现图形，如图表、游戏绘图等。 |
| `<embed>` | 代表一个嵌入 的外部资源，如应用程序或交互内容。 |
| `<svg>` | 定义一个嵌入式矢量图 。 |
| `<math>` | 定义一段数学公式 。 |
| `<summary>` | 代表 `<details>` 元素的综述 或标题 。 |
| `<details>` | 代表一个用户可以(点击)获取额外信息或控件的小部件 。 |
| `<menu>` | 代表菜单。 |
| `<menuitem>` | 代表一个用户可以点击的菜单项。 |

## 常用 Meta 标签

[HTML meta标签总结与属性使用介绍](https://segmentfault.com/a/1190000004279791)

meta标签共有两个属性，分别是http-equiv属性和name属性。
- name 属性：主要用于描述网页，比如网页的关键词，叙述等。
- http-equiv 属性：equivalent，把 content 属性关联到 HTTP 头部。
- scheme 属性：定义用于翻译 content 属性值的格式。

```html 
<meta charset="utf-8">    声明文档使用的字符编码
<meta name="author" content="name, email@gmail.com"/>    网页作者
<meta name="keywords" content="keywords1,keywords2"/>      页面关键词
<meta name="description" content="不超过150个字符"/>       页面描述
<meta name="viewport" content="initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no"> 为移动设备添加 viewport
<meta name="renderer" content="webkit">  默认渲染内核
<meta name="robots" content="index,follow">
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>   指定IE和Chrome使用最新版本渲染当前页面
<meta http-equiv="Cache-Control" content="no-siteapp" />   禁止百度自动转码
设置页面不缓存
<meta http-equiv="pragma" content="no-cache">
<meta http-equiv="cache-control" content="no-cache">
<meta http-equiv="expires" content="0″>
```

## Cookie, LocalStorage 与 SessionStorage

<table style="border: 1px solid #ccc;">
	<thead>
		<tr>
			<th>特性</th>
			<th>Cookie</th>
			<th>localStorage</th>
			<th>sessionStorage</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>数据的生命期</td>
			<td>一般由服务器生成，可设置失效时间。如果在浏览器端生成Cookie，默认是关闭浏览器后失效</td>
			<td>除非被清除，否则永久保存</td>
			<td>仅在当前会话下有效，关闭页面或浏览器后被清除</td>
		</tr>
		<tr>
			<td>存放数据大小</td>
			<td>4K左右</td>
			<td colspan="2">一般为5MB</td>
		</tr>
		<tr>
			<td>与服务器端通信</td>
			<td>每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题</td>
			<td colspan="2">仅在客户端（即浏览器）中保存，不参与和服务器的通信</td>
		</tr>
		<tr>
			<td>易用性</td>
			<td>需要程序员自己封装，源生的Cookie接口不友好</td>
			<td colspan="2">源生接口可以接受，亦可再次封装来对Object和Array有更好的支持</td>
		</tr>
	</tbody>
</table>
