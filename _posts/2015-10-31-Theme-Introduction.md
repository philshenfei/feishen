---
title: 主题介绍 
cover-image: marilyn-monroe.jpg
---

这是一篇关于博客主题的介绍。这个主题的原始模板来自于互联网，具体来源已经忘记了，在此基础上，结合 [Markdown](https://daringfireball.net/projects/markdown/) 语言和 css，进行了适当的修改。很难说现在的模样已经一劳永逸地达到了我的要求，毕竟审美观念是会不断更新的。

对其中的一些重要元素进行说明：

* 封面图片。通过设置 cover-image 来添加，图片的宽度尽量超过 800px，通过自动缩小来适应页面。页面中的图片左对齐，没有居中处理。如果需要居中的话，可以在 css 文件 img 中设置 `margin: 0 auto;`。 
* 一级标题。没有采用 `# H1 #` 来添加，而是通过 `title` 来设置博客标题。
* 二级标题。直接采用 Markdown 语言 `## H2 ##` 来添加，在二级标题上有一道灰线，作为分隔线。
* 三级及以下标题。方法同二级标题 `### H3 ###` ，但是没有分隔线。
* 段落。 行高设置为 1.8em，段前段后空 1em。
* 引用。在段前采用 `>`，引用文字为灰色，呈斜体。
* 图片。不建议采用 `![figure alt](figure url)`，因为这种类型的图片已经设置了一种特殊的 css 样式。而建议采用 html 的语法 `<div class="figure">...</div>`。
* 高亮代码。在代码前后分别添加 `{\% highlight css \%}` 和 `{\% endhighlight \%}` 。
* 公式。采用 `<div class="formula">...</div>`。   
* 视频。采用 `<div class="video">...</div>`。

## 二级标题 ##

### 三级标题 ###

注意设置段落文字字体、字号、段间距和段前段后。

#### 列表 ####

* item 1：橄榄树
* item 2：月琴
* item 3：美丽岛

#### 引用 ####

> 关关雎鸠，在河之洲。——《诗经》

#### 图片 ####

<div class="figure">
  <img src="{{ site.baseurl }}/img/forest.jpg">
  <small>一张风景图片</small>
</div>

#### 代码 ####

{% highlight css %}
nav a:hover {
  color: rgba(0,0,0,.72);
}
nav a.current {
  color: rgba(0, 0, 0, .72)
}
.subtitle {
  margin: 30px 0;
}

@media all and (min-width: 370px) {
  .website-title {
    font-size: 32px;
  }
  body {
    font-size: 24px;
  }
  a {
    background-position: 0 18px;
  }
}

//一段C++代码：
#include <iostream>
#include std::cout;

int main(){
  int a=10;
  int b=20;
  cout<<a+b<<endl;
}
{% endhighlight %}

#### 公式 ####

行内公式 $E=mc^2$

<div class="formula">
\begin{eqnarray}
\dot{x}& =& \sigma(y-x) \\ 
\dot{y}& =& \rho x - y - xz \\ 
\dot{z}& = &-\beta z + xy
\end{eqnarray}

\begin{eqnarray}
\frac{\mathrm{d}D_e}{\mathrm{d}N} & = &[1-(1-D)^{\beta+1}]^{\alpha(\sigma_{max}\bar{\sigma})} \nonumber\\
& & \Big[\frac{\sigma_{max}-\bar{\sigma}}{M_0 (1-b_2\bar{\sigma})(1-D)}\Big]^{\beta}
\end{eqnarray}

\begin{equation*}
   E = mc^2
\end{equation*}
</div>

#### 视频 ####

<div class="video">
<iframe width="560" height="315" src="https://www.youtube.com/embed/GYrOqDxefbM" frameborder="0" allowfullscreen></iframe>
</div>

#### 表格 ####

| Left-Aligned  | Center Aligned  | Right Aligned |
| :------------ |:---------------:| -----:|
| col 3 is      | some wordy text | $1600 |
| col 2 is      | centered        |   $12 |
| zebra stripes | are neat        |    $1 |

#### 超链接 ####

[GitHub](https://github.com/philshenfei?tab=repositories)