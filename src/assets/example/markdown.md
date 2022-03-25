# 博客如何将流量导向微信公众号？

> 博客将流量导向微信公众号，可以使用 TechGrow 的免费导流工具实现，用户扫码关注微信公众号后可以解锁全站文章，让微信公众号的粉丝数躺着增长。整个过程只需六步就可以搞定，适用于各类主流的博客，本文以 Hexo 的 NexT 主题博客举例。

## 第一步：注册博客

PC 端浏览器访问 TechGrow（open.techgrow.cn）的官网，注册并登录账号后，进入博客的后台管理页面。首先点击左侧的菜单 `博客注册`，然后点击 `新增` 按钮，添加自己博客的信息。博客注册成功后，记录下博客 ID，后面的步骤会使用到。

![](https://www.techgrow.cn/uploads/2022/02/28/717e14eb59dd44dea62d6a0b7549abfd.png)

## 第二步：设置微信公众号自动回复

在微信公众号的后台管理页面，菜单栏里选择 `自动回复` - `关键词回复`，启用 `自动回复`，然后点击 `添加回复` 按钮：

![](https://www.techgrow.cn/uploads/2022/02/28/em64p7w8wlqtt0rsjop0jjeywx29m25w.png)

填写 `规则名称`、`关键词（当初你在 TechGrow 中设置的）`、`回复内容` 选择 `文字`，然后 `回复文字` 的内容填写获取博客解锁验证码的链接，如下所示（请自行更改 `xxxxx-xxxxxxxxx-xxx` 为你申请到的博客 ID）

``` html
<a href="https://open.techgrow.cn/#/readmore/captcha/generate?blogId=xxxxx-xxxxxxxxx-xxx">点击链接，获取博客解锁验证码</a>
```

![](https://www.techgrow.cn/uploads/2022/02/28/yd89wbdji196ixtwzgzamw37fbein1ia.png)

此时，当读者关注你的微信公众号，并输入关键词后（比如我设置的关键词就是 `tech`），那么读者就会自动接收到获取博客解锁验证码的链接

## 第三步：定位文章主体的标签元素

在博客的 `themes` 目录下，找到你正在使用的主题目录，比如：`next` 等，具体根据你选择的主题来判断。进入主题的 `layout` 目录，找到 `_macro/post.njk` 模板文件，若这里有一大段与文章主体内容相关的 HTML 代码，那就说明文章的模板定义就在这里，示例模板代码如下：

``` js
<div class="post-block">
  {# Gallery support #}
  {{ post_gallery(post.photos) }}

  <article itemscope itemtype="http://schema.org/Article" class="post-content" lang="{{ post.lang }}">
    <link itemprop="mainEntityOfPage" href="{{ post.permalink }}">

    <span hidden itemprop="author" itemscope itemtype="http://schema.org/Person">
      <meta itemprop="image" content="{{ url_for(theme.avatar.url or theme.images + '/avatar.gif') }}">
      <meta itemprop="name" content="{{ author }}">
      <meta itemprop="description" content="{{ description }}">
    </span>

    ...（省略）

  </article>
</div>
```

另一种查看方式是打开你博客的任意一篇文章，利用 Chrome 等浏览器的元素审查功能，找到文章页面中文章主体的标签元素，比如下图中的 `article` 就是整篇文章的主体标签元素：

![](https://www.techgrow.cn/uploads/2022/02/28/5562a8e4868843e0868a4bdfd67c530e.png)

## 第四步：新增文章内容的 DIV 标签

在文章模板文件中找到文章主体的标签元素之后，在其上一层包一层 `div` 标签，并将 `div` 标签的 `id` 属性设置为 `readmore-container`，示例模板代码如下：

``` html
<div class="post-block">
  {# Gallery support #}
  {{ post_gallery(post.photos) }}

  <div id="readmore-container">
    <article itemscope itemtype="http://schema.org/Article" class="post-content" lang="{{ post.lang }}">
        <link itemprop="mainEntityOfPage" href="{{ post.permalink }}">

        <span hidden itemprop="author" itemscope itemtype="http://schema.org/Person">
        <meta itemprop="image" content="{{ url_for(theme.avatar.url or theme.images + '/avatar.gif') }}">
        <meta itemprop="name" content="{{ author }}">
        <meta itemprop="description" content="{{ description }}">
        </span>

        ...（省略）

    </article>
  </div>
</div>
```

## 第五步：新增导流工具的 HTML 代码

打开 TechGrow 的[博客后台管理页面](https://open.techgrow.cn/#/readmore/website/register)，点击博客列表中右侧的 `使用` 链接，将窗口里的 HTML 代码复制到第三步中找到的文章模板文件的末尾，也可以添加到主题的 `footer` 模板文件中，示例 HTML 代码如下图所示：

![](https://www.techgrow.cn/uploads/2022/02/28/ad963a38752743169e8f351983cc6cc1.png)

- 参数说明

```
id：div 标签的 ID
blogId：已申请的博客 ID
name: 已申请的微信公众号名称
qrcode: 已申请的微信公众号二维码链接
keyword：已申请的微信公众号回复关键字
random：每篇文章随机添加微信公众号导流工具的概率，有效范围在 0.1 ~ 1 之间，1 则表示所有文章默认都自动添加导流工具（可选）
```

## 第六步：验证导流工具是否正常运行

重新构建并运行博客服务后，打开文章页面，若文章自动隐藏了部分内容，并且出现了 `阅读全文` 按钮，则说明导流工具整合成功，如下图所示：

![](https://www.techgrow.cn/uploads/2022/02/28/3f53ab36dfa84fb99a6508ae46e5373a.png)


欢迎关注我的公众号“**全栈技术驿站**”，全栈开发的最新技术和最新动态第一时间推送。

<center>
    <img src="https://www.techgrow.cn/img/wx_mp_qr.png" style="width: 100px;">
</center>