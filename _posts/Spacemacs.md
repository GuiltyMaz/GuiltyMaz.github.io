第一篇博客：解决Spacemacs安装的问题
设备：Windows10
软件：Git、Powershell5
参考链接：https://blog.csdn.net/liweigao01/article/details/109498097
	
git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
我不喜欢这个~，所以在C盘下把~文件夹删掉了，只留下~里面的.emacs.d

前置条件：
.spacmacs.d(如果默认的话，应该是在C:\Users\Administrator\.emacs.d)中找到(defun dotspacemacs/user-init ()并在其中添加以下代码：
(当然Administrator可以换成你电脑用户名，example:C:\Users\XXX\.emacs.d)
(setq configuration-layer-elpa-archives
    '(("melpa-cn" . "http://mirrors.tuna.tsinghua.edu.cn/elpa/melpa/")
      ("org-cn"   . "http://mirrors.tuna.tsinghua.edu.cn/elpa/org/")
      ("gnu-cn"   . "http://mirrors.tuna.tsinghua.edu.cn/elpa/gnu/")))
      
错误1：hl-todo、undo-tree、compat未安装
链接：
https://melpa.org/#/hl-todo
https://elpa.gnu.org/packages/undo-tree.html
https://elpa.gnu.org/packages/compat.html
下载解压这三个文件到C:\Users\Administrator\AppData\Roaming\.emacs.d\plugins（plugins是自建的）PS:里面的文件全部取出到plugins
![image](https://github.com/GuiltyMaz/guiltymaz.github.io/assets/106474168/307d2dc0-e02d-4756-97a1-1de6099f2231)
在C:\Users\Administrator\.emacs.d尾部中添加这个
(add-to-list 'load-path "C:/Users/Administrator/AppData/Roaming/.emacs.d/plugins")
(require 'hl-todo)
(require 'undo-tree)
(require 'compat)

错误2：Source Code Pro不存在
在.spacemacs中把Source Code Pro换成Source Code Pro

错误3：Non-hex digit used for Unicode escape：s(115)
在spacemacs上M-x -debug-init
输出一段错误日志：
Debugger entered--Lisp error: (error "Non-hex character used for Unicode escape: s (115)")
  read(#<buffer  *load*-595518>)
  load-with-code-conversion("c:/Users/Administrator/AppData/Roaming/.spacemacs" "c:/Users/Administrator/AppData/Roaming/.spacemacs" nil nil)
  load("c:/Users/Administrator/AppData/Roaming/.spacemacs")
  (condition-case err (load dotspacemacs) ((debug error) (message "Error loading .spacemacs: %S" err) nil))
  (if (condition-case err (load dotspacemacs) ((debug error) (message "Error loading .spacemacs: %S" err) nil)) nil (dotspacemacs/safe-load))
  (if (file-exists-p dotspacemacs) (if (condition-case err (load dotspacemacs) ((debug error) (message "Error loading .spacemacs: %S" err) nil)) nil (dotspacemacs/safe-load)))
  (let ((dotspacemacs (dotspacemacs/location))) (if (file-exists-p dotspacemacs) (if (condition-case err (load dotspacemacs) ((debug error) (message "Error loading .spacemacs: %S" err) nil)) nil (dotspacemacs/safe-load))))
  dotspacemacs/load-file()
  spacemacs/init()
  (let ((please-do-not-disable-file-name-handler-alist nil)) (require 'core-spacemacs) (spacemacs/dump-restore-load-path) (configuration-layer/load-lock-file) (spacemacs/init) (configuration-layer/stable-elpa-init) (configuration-layer/load) (spacemacs-buffer/display-startup-note) (spacemacs/setup-startup-hook) (spacemacs/dump-eval-delayed-functions) (if (and dotspacemacs-enable-server (not (spacemacs-is-dumping-p))) (progn (require 'server) (if dotspacemacs-server-socket-dir (progn (setq server-socket-dir dotspacemacs-server-socket-dir))) (if (server-running-p) nil (message "Starting a server...") (server-start)))))
  (if (not (version<= spacemacs-emacs-min-version emacs-version)) (error (concat "Your version of Emacs (%s) is too old. " "Spacemacs requires Emacs version %s or above.") emacs-version spacemacs-emacs-min-version) (let ((please-do-not-disable-file-name-handler-alist nil)) (require 'core-spacemacs) (spacemacs/dump-restore-load-path) (configuration-layer/load-lock-file) (spacemacs/init) (configuration-layer/stable-elpa-init) (configuration-layer/load) (spacemacs-buffer/display-startup-note) (spacemacs/setup-startup-hook) (spacemacs/dump-eval-delayed-functions) (if (and dotspacemacs-enable-server (not (spacemacs-is-dumping-p))) (progn (require 'server) (if dotspacemacs-server-socket-dir (progn (setq server-socket-dir dotspacemacs-server-socket-dir))) (if (server-running-p) nil (message "Starting a server...") (server-start))))))
  load-with-code-conversion("c:/Users/Administrator/.emacs.d/init.el" "c:/Users/Administrator/.emacs.d/init.el" nil nil)
  load-file("C:\\Users\\Administrator\\.emacs.d\\init.el")
  load-with-code-conversion("c:/Users/Administrator/AppData/Roaming/.emacs" "c:/Users/Administrator/AppData/Roaming/.emacs" t t)
  load("~/.emacs" noerror nomessage)
  startup--load-user-init-file(#f(compiled-function () #<bytecode 0x22ac2fd9acbcf5c>) #f(compiled-function () #<bytecode -0x1f3c686ddc0da035>) t)
  command-line()
  normal-top-level()
  这里大概就知道自己应该是配置文件.spacemacs中出现错误，接着查了一段时间之后发现是十六进制转义的错误，接着
  根据提供的错误日志和配置代码，我看到了一些潜在的问题：

错误信息: Symbol’s value as variable is void: path

在你的配置中，有一处代码是错误的：

emacs
Copy code
(add-to-list 'load path "C:\Users\Administrator\.emacs.d\lisp\winum")
正确的代码应该是使用 load-path 变量来添加路径，而不是使用未定义的 path 变量。此外，Windows 路径需要使用反斜杠 \\ 来表示。你可以修改为：

emacs
Copy code
(add-to-list 'load-path "C:/Users/Administrator/.emacs.d/lisp/winum")
错误信息: Symbol's value as variable is void: configuration-layer--elpa-archives
然后，就大功告成了。（遇到问题，先看错误，再查文档。我去问别人，别人根本不搭理我，耍大牌的太多了。很多问题不是谷歌就是文档，你问别人可能要做到过年。）

