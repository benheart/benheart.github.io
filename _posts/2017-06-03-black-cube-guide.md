---
layout: post
title: Black Cube 主题使用手册
description: 基于简单而又有质感的理念，<a href="http://www.pizn.me" target="_blank">PIZn</a> 设计并编写了这个主题。下面介绍文章页的一些标签使用方法。
key: pizn theme, Black Cube
---

* 目录
{:toc}

### "BLACKQUERY"标签：

> Your time is limited, so don't waste it living someone else's life.…Don't let the noise of others' opinions drown out your own inner voice.

### "H"标签：

# 如何创建漂亮的 Jekyll 模板(h1)
{:.no_toc}

## 如何创建漂亮的 Jekyll 模板(h2)
{:.no_toc}

### 如何创建漂亮的 Jekyll 模板(h3)
{:.no_toc}

#### 如何创建漂亮的 Jekyll 模板(h4)
{:.no_toc}

##### 如何创建漂亮的 Jekyll 模板(h5)
{:.no_toc}

##### 如何创建漂亮的 Jekyll 模板(h6)
{:.no_toc}

### "P"标签：

基于简单而又有质感的理念，<a href="http://www.pizn.me" target="_blank">PIZn</a> 设计并编写了这个主题。下面介绍文章页的一些标签使用方法

### "LI"标签：

* 基于简单而又有质感的理念，<a href="http://www.pizn.me" target="_blank">PIZn</a> 设计并编写了这个主题。下面介绍文章页的一些标签使用方法
* 基于简单而又有质感的理念，<a href="http://www.pizn.me" target="_blank">PIZn</a> 设计并编写了这个主题。下面介绍文章页的一些标签使用方法
* 基于简单而又有质感的理念，<a href="http://www.pizn.me" target="_blank">PIZn</a> 设计并编写了这个主题。下面介绍文章页的一些标签使用方法
* 基于简单而又有质感的理念，<a href="http://www.pizn.me" target="_blank">PIZn</a> 设计并编写了这个主题。下面介绍文章页的一些标签使用方法

### "CODE" 和 "PRE"：
<code class="code">Hello world</code>

{% highlight java %}
/**
 * ElasticSearch configuration.
 */
@Bean(destroyMethod = "close")
public Client client() throws Exception {
    Client client = null;
    try {
        // set ping schedule execute every 10 min.
        Settings settings = Settings.settingsBuilder()
                .put("transport.ping_schedule", "600s").build();

    } catch (Exception e) {
        LOG.warn("Create es client failed, no ip or port param, " +
                "or connect es timeout. Error msg: {}", e.getMessage());
    }
    return client;
}
{% endhighlight %}

### "IMG"标签：
图片提供 3 种样式，分别是<code class="code">img-center</code>, <code class="code">img-right</code>, <code class="code">img-left</code>  
<img src="/images/hacker.jpg" class="img-center" style="width: 400px;" alt="thumb" >

### "TABLE"标签：

<table width="100%">
	<tbody>
		<tr>
			<th width="30%">命令</th>
			<th width="70%">作用（解释)</th>
		</tr>
		<tr>
			<td>
				<code class="v-code">:w</code>
			</td>
			<td>保存</td>
		</tr>
		<tr>
			<td>
				<code class="v-code">:wq</code>,
				<code class="v-code">:x</code>
			</td>
			<td>保存并关闭</td>
		</tr>
		<tr>
			<td>
				<code class="v-code">:q</code>
			</td>
			<td>关闭（已保存）</td>
		</tr>
		<tr>
			<td>
				<code class="v-code">:q!</code>
			</td>
			<td>强制关闭</td>
		</tr>
	</tbody>
</table>