---
redirect_from: _posts/2023-08-26-Spacemacs/
title: Spacemacs安装
tags:
  - 软件安装
---

设备：Windows10

软件：Git、Powershell5、emacs

##参考链接：<br />
	 [参考1][https://blog.csdn.net/liweigao01/article/details/109498097]<br />
	 [参考2][https://blog.csdn.net/ytzys/article/details/80989421?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-80989421-blog-100057060.235%5Ev38%5Epc_relevant_anti_vip&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-80989421-blog-100057060.235%5Ev38%5Epc_relevant_anti_vip&utm_relevant_index=2]
	
git clone https://github.com/syl20bnr/spacemacs \~//.emacs.d


我不喜欢这个\~/，所以在C盘下把~文件夹删掉了，只留下\~/里面的.emacs.d

##前置条件：<br />
.spacmacs.d(如果默认的话，应该是在C:\Users\Administrator\.emacs.d)中找到(defun dotspacemacs/user-init ()并在其中添加以下代码：<br />

(当然Administrator可以换成你电脑用户名，example:C:\Users\XXX\.emacs.d)<br />

(setq configuration-layer-elpa-archives<br />
    '(("melpa-cn" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/")<br />
      ("org-cn"   . "http://mirrors.tuna.tsinghua.edu.cn/elpa/org/")<br />
      ("gnu-cn"   . "http://mirrors.tuna.tsinghua.edu.cn/elpa/gnu/")))<br />

      
###错误1：hl-todo、undo-tree、compat未安装<br />

链接：<br />
[hl-todo][https://melpa.org/#/hl-todo]<br />
[undo-tree][https://elpa.gnu.org/packages/undo-tree.html]<br />
[compat][https://elpa.gnu.org/packages/compat.html]<br />

下载解压这三个文件到C:\Users\Administrator\AppData\Roaming\.emacs.d\plugins（plugins是自建的）<br />
![image](https://github.com/GuiltyMaz/guiltymaz.github.io/assets/106474168/307d2dc0-e02d-4756-97a1-1de6099f2231)

在C:\Users\Administrator\.emacs.d尾部中添加这个<br />
(add-to-list 'load-path "C:/Users/Administrator/AppData/Roaming/.emacs.d/plugins")<br />
(require 'hl-todo)<br />
(require 'undo-tree)<br />
(require 'compat)<br />


###错误2：Source Code Pro不存在<br />

在.spacemacs中把Source Code Pro换成Source Code Pro<br />


###错误3：Non-hex digit used for Unicode escape：s(115)<br />

在spacemacs上M-x -debug-init<br />

输出一段错误日志：<br />
Debugger entered--Lisp error: (error "Non-hex character used for Unicode escape: s (115)")<br />
  read(#<buffer  *load*-595518>)<br />
  load-with-code-conversion("c:/Users/Administrator/AppData/Roaming/.spacemacs" "c:/Users/Administrator/AppData/Roaming/.spacemacs" nil nil)<br />
  load("c:/Users/Administrator/AppData/Roaming/.spacemacs")<br />

这里大概就知道自己应该是配置文件.spacemacs中出现错误，接着查了一段时间之后发现是十六进制转义的错误，接着
根据提供的错误日志和配置代码，我看到了一些潜在的问题：<br/>

错误信息: Symbol’s value as variable is void: path

在你的配置中，有一处代码是错误的：

emacs<br />
Copy code<br />
(add-to-list 'load path "C:\Users\Administrator\.emacs.d\lisp\winum")<br />
正确的代码应该是使用 load-path 变量来添加路径，而不是使用未定义的 path 变量。此外，Windows 路径需要使用反斜杠 \\ 来表示。你可以修改为：<br />

emacs
Copy code
(add-to-list 'load-path "C:/Users/Administrator/.emacs.d/lisp/winum")
错误信息: Symbol's value as variable is void: configuration-layer--elpa-archives

##总结<br />
然后，就大功告成了。（遇到问题，先看错误，再查文档。我去问别人，别人根本不搭理我，耍大牌的太多了。很多问题不是谷歌就是文档，你问别人可能要做到过年。）<br />
![image](https://github.com/GuiltyMaz/guiltymaz.github.io/assets/106474168/192324a2-6f0d-4021-89a5-aa2eee8c0ef5)


