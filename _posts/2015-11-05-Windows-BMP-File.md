---
title: 位图 Bitmap 数据结构研究
---

位图 Bitmap 是 Windows 标准格式图形文件，扩展名多为 BMP。它将图像定义为由点（像素）组成，每个点可以由多种色彩表示，包括 2、4、8、16、24 和 32 位色彩。例如，一幅 1024×768 分辨率的 32 位真彩图片，其所占存储字节数为：1024×768×32/8=3072KB。

BMP 文件由四部分组成：

* 位图文件头（bitmap-file header）
* 位图信息头（bitmap-information header）
* 颜色表（color table）（24 位真彩色位图没有此部分）
* 点阵数据（bits data）

本文以 24 位位图 lotus.bmp 文件来具体说明。

<div class="figure">
  <img src="{{ site.baseurl }}/img/lotus.bmp"> 
  <small><capfigure>图 1 lotus.bmp</capfigure></small>
</div>

首先是位图文件头，其数据结构定义为：

{% highlight css %}
typedef struct tagBITMAPFILEHEADER{
WORD bfType;//位图文件的类型，必须为BM
DWORD bfSize;//位图文件的大小，以字节为单位
WORD bfReserved1;//位图文件保留字，必须为0
WORD bfReserved2;//位图文件保留字，必须为0
DWORD bfOffBits;//位图数据的起始位置，以相对于位图文件头的偏移量表示，以字节为单位
}BITMAPFILEHEADER;
{% endhighlight %}

根据位图文件头的结构可知位图文件头占 14 字节。bfType 占 2 字节，值为 BM，对应到其 ASCII 码的十六进制形式为 0x42 4D。bfSize 占 4 字节，由于 Lotus.bmp 文件大小为 1234326 字节，对应于 0x00 12 D5 96，根据字存储顺序为低位在前、高位在后，于是在位图文件中的表示形式为 0x96 D5 12 00。bfReserved1 和 bfReserved2 一共占 4 字节，全为 0，因此为 0x00 00 00 00。bfOffBits 占 4 字节，根据图 2 可知此值为 0x36 00 00 00，调转之后为 0x00 00 00 36，也即是 54 字节。位图数据起始位置在偏移文件开头 54 字节的地方。

<div class="figure">
  <img src="{{ site.baseurl }}/img/bmp-part.jpg"> 
  <small><capfigure>图 2 lotus.bmp 文件的十六进制形式（部分）</capfigure></small>
</div>

位图信息头的数据结构为：

{% highlight css %}
typedef struct tagBITMAPINFOHEADER{
DWORD biSize;//本结构所占用字节数
LONG biWidth;//位图的宽度，以像素为单位
LONG biHeight;//位图的高度，以像素为单位
WORD biPlanes;//目标设备的级别，必须为1
WORD biBitCount;//每个像素所需的位数，必须是1(双色),4(16色)，8(256色)或24(真彩色)之一
DWORD biCompression;//位图压缩类型，必须是 0(不压缩), 1(BI_RLE8压缩类型)或2(BI_RLE4压缩类型)之一
DWORD biSizeImage;//位图全部像素占用的字节数，以字节为单位
LONG biXPelsPerMeter;//位图水平分辨率，每米像素数
LONG biYPelsPerMeter;//位图垂直分辨率，每米像素数
DWORD biClrUsed;//位图实际使用的颜色表中的颜色数
DWORD biClrImportant;//位图显示过程中重要的颜色数
}BITMAPINFOHEADER;
{% endhighlight %}

位图信息头共 40 字节。biSize 占 4 字节，其值为 40，即 0x00 28，对应到文件中为 0x28 00 00 00。biWidth 其类型为 long，至少是 4 字节，Lotus.bmp 文件的像素为 687×598，687 对于 0x02 AF，对应与文件的 0xAF 02 00 00。biHeight 同样占 4 字节，598 对于 0x02 56，对应与文件的 0x56 02 00 00。biPlanes 占 2 字节，其值为 1，对应与文件为 0x01 00。biBitCount 占 2 字节，由于真彩色的每个像素所需的位数为 24，对应于文件为 0x18 00。biCompression 占 4 字节，由于不压缩，故其值为 0，对应文件为 0x00 00 00 00。biSizeImage 占 4 字节，其大小指的是全部像素所占的字节数，可根据位图文件的大小减掉 54 字节的偏移量，即得 1234272 字节，对应于文件为 0x60 D5 12 00。biXPelsPerMeter 占 4 字节，其值为 3780，对应于文件为 0xC4 0E 00 00。biYPelsPerMeter 占 4 字节，其值为 3780，对应于文件为 0xC4 0E 00 00。biClrUsed 占 4 字节，如果为 0，则颜色数为 2 的 biBitCount 次方，对应文件为 0x00 00 00 00。biClrImportant 占 4 字节，如果为 0，则表示所有颜色都重要，对应文件为 0x00 00 00 00。

位图全部的像素，是按照自下向上，自左向右的顺序排列的。也就是说，从文件中最先读到的是图象最下面一行的左边第一个象素，然后是左边第二个像素，接下来是倒数第二行左边第一个像素，左边第二个像素，依次类推，最后得到的是最上面一行的最右一个像素。因为 32 位的 Windows 操作系统处理 4 个字节（32位）的速度比较快，所以位图文件的每一行颜色占用的字节数规定为 4 的整数倍。于是存在行补位公式：widthBytes = (width*biBitCount+31)/32*4，对于 lotus.bmp 文件，width=687，biBitCount=24，于是每行的字节数为 2064（其中除以 32 之后需要取整）。所以整个位图像素数据大小为 2064×598=1234272。

行补位是在一行像素数据之后补上 0，使得每行的字符数是 4 的整数倍。对于 lotus.bmp 文件，需要补位 2064-687×3=3 个字节。在存储 RGB 三原色时采用的是 BGR 的方式，例如第一个像素的数据为 0x49 17 05，对应于 RGB 应为 R=5，G=17，B=49。

需要注意的是：

* biXPelsPerMeter 的值为 3780，可以根据位图的 DPI 来计算。例如 lotus.bmp 的 DPI 为 96，即 2.54 厘米的长度上有 96 个像素，那么每米的像素数为 3780。
