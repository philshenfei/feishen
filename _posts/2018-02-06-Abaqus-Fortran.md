---
title: Abaqus子程序使用方法
cover-image:
---

在Abaqus中使用子程序，首先需要Fortran编译器来编译子程序，然后在主程序（Standard或Explicit求解器）调用编译之后的子程序，完成计算分析。在安装Abaqus和Fortran编译器之后，一般会将两者关联起来，方法比较简单，即在Abaqus命令之前调用Fortran环境文件。这样之后，在CAE中可以直接选择子程序文件，或者使用如下命令行提交计算。

{% highlight css %}
abaqus job=input.inp user=subroutine.for cpus=n
{% endhighlight %}

如果没有进行关联，可以单独编译子程序文件，然后在主程序调用编译的文件，步骤如下：

* 使用Abaqus中的make命令来编译子程序文件。这样会生成一个目标文件（Windows下是.obj文件，Linux下是.o文件）和一个共享库文件（Windows下是.obj文件，Linux下是.o文件）。Abaqus/Standard下的目标文件后缀是-std，库文件是libstandardU，Abaqus/Explicit下的后缀是-xpl或-xplD，库文件是libexplicitU或 libexplicitU-D。
{% highlight css %}
abaqus make library=subroutine.for
{% endhighlight %}

* 把生成的两个文件拷贝到工作目录，然后在abaqus_v6.env中设置usub_lib_dir为当前工作目录。

{% highlight css %}
import os
usub_lib_dir=os.getcwd()
{% endhighlight %}

* 不使用–user选项，提交计算。

{% highlight css %}
abaqus job=input.inp cpus=n
{% endhighlight %}

PS：如果想把一个子程序文件合并到其他编译后的目标文件，可以使用directory选项。

{% highlight css %}
abaqus make library=subroutine2.for directory=\location\of\subroutine1-std.obj
{% endhighlight %}