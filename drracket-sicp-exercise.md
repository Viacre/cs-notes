# 使用 DrRacket 完成《计算机程序的构造和解释》的题目

# 1 前置阅读

[计算机语言学习导论](https://github.com/FrankHB/pl-docs/blob/master/zh-CN/introduction-to-learning-computer-languages.md)

## 1.1 所谓“语言工具论”

引用 Edsger W. Dijkstra 的一段话
> The tools we use have a profound (and devious!) influence on our thinking habits, and, therefore, on our thinking abilities.
> It is practically impossible to teach good programming to students that have had a prior exposure to BASIC: as potential programmers they are mentally mutilated beyond hope of regeneration.
> About the use of language: it is impossible to sharpen a pencil with a blunt axe. It is equally vain to try to do it with ten blunt axes instead.

* 我们使用的工具会对我们的思维习惯产生深远（而狡猾！）的影响，进而影响我们的思维能力。
* 实际上，要想教那些以前学过 BASIC 的学生学好编程是不可能的：作为潜在的程序员，他们在精神上已经残缺不全，没有再生的希望。
* 关于语言的使用：用一把钝斧是不可能削好一支铅笔的。用十把钝斧头来削铅笔，同样是徒劳的。

## 1.2 Scheme 和 Racket

**注释** 虽然本文会尽量少放次要资料，但为了避免读者在信息检索的时候被一些流行观点带偏，这里还是补充一些。

* [MIT/GNU Scheme](https://zh.wikipedia.org/wiki/MIT/GNU_Scheme) 在书中的脚注和代码注释中多次提到，而本文不推荐使用 MIT/GNU Scheme 的原因如下。
	* 不再支持 Windows 平台。
	* 需要学习使用 Emacs 之类古老的编辑器，虽然 Emacs 对于 Lisp 开发来说基本没有更好的替代，但这里只是做题，使用 DrRacket 足够。笔者认为折腾编辑器、操作系统的风气对缺乏常识、能力不足以分析具体需求的新手来说是有害的。如果确实有使用编辑器更方便的需求，如编辑 Markdown、LaTeX 等，可以使用 [Visual Studio Code](https://code.visualstudio.com/)，有较全面的官方文档。
	* [《计算机程序的结构与解释》的配套网站](https://mitp-content-server.mit.edu/books/content/sectbyfn/books_pres_0/6515/sicp.zip/sicp.html)中的大部分做题相关的过程可以由下文提到的 Racket 库 `sicp` 和 `sicp-pict` 提供，除了 [Code from the book](https://mitp-content-server.mit.edu/books/content/sectbyfn/books_pres_0/6515/sicp.zip/code/index.html) 之外的资源可以忽略。
	* MIT/GNU Scheme 主要符合 [R<sup>7</sup>RS(small)](https://standards.scheme.org/official/r7rs.pdf) 标准，笔者更推荐从 [R<sup>5</sup>RS](https://standards.scheme.org/official/r5rs.pdf) 开始。r5rs 文档长达 50 页，在编程语言爱好者中备受推崇。它是 Scheme 社区的试金石，在其他社区也被视为简洁而富有表现力的语言规范的典范。（翻译自 [Scheme Standards](https://standards.scheme.org/)）
* [Racket](https://zh.wikipedia.org/wiki/Racket) 的优势
	* 有相对友好的 IDE，图形化调试功能。
	* 借用 Racket 的模块系统。只是简单使用，学习成本很低。

# 2 配置 DrRacket

1. 进入[清华大学 TUNA 镜像](https://mirrors.tuna.tsinghua.edu.cn/racket-installers/)找到最新的版本(version)文件夹中的 `racket-<version>-x86_64-win32-cs.exe` DrRacket 安装程序。
2. 安装完成后打开 DrRacket，可以点击 `Help` -> `使用简体中文作为 DrRacket 界面语言`。
3. 重启 DrRacket 后点击 `文件` -> `Package管理...` -> `执行`，在 `Package来源` 后输入以下软件包名并回车以下载。软件包的主要来源是 Github，下载插件时有网络问题可以考虑使用 [Watt Toolkit](https://steampp.net/)。
	* `sicp`: [Github 页面](https://github.com/sicp-lang/sicp)
	* `files-viewer`: 文件管理器
		* 右键左边文件夹区域，点击 `change the directory` 更改到你要存放代码的目录。
		* 执行添加、删除、重命名文件等操作前需要先左键选中。
	* `drcomplete`: 自动补全
		* `drcomplete-method-names` 需要单独安装，详见 [Github 页面](https://github.com/yjqww6/drcomplete)。补全默认在输入三个字母后开始。
4. `Ctrl+Shift+L` 在左右分割和上下分隔中切换，左右分割更方便阅读代码和查看运行结果。

# 3 做题

## 3.1 目录结构

* `ch1` `ch2` `ch3` `ch4` `ch5` 存放各个章节的题目代码和示例代码，各个章节的文件夹互相独立，需要使用其他章节的代码可以直接复制需要的代码文件。
* `allcode` 存放从 [Code from the book](https://mitp-content-server.mit.edu/books/content/sectbyfn/books_pres_0/6515/sicp.zip/code/index.html) 下载的 Scheme 代码文件，不需要更改为 `.rkt` 扩展名，只是方便使用 `Ctrl+F` 查找定义，实际使用应该拆分到各章节的文件夹中。

## 3.2 模块系统

1. `ch1~ch5` 的所有 Racket 代码文件的头两行应分别包含 `#lang racket` 和 `(require sicp)`。`sicp` 提供的过程详见 [SICP Language](https://docs.racket-lang.org/sicp-manual/SICP_Language.html)
2. `(require sicp-pict)` 仅在书中 2.2.4 节相关的代码文件中使用。`sicp-pict` 提供的过程详见 [SICP Picture Language](https://docs.racket-lang.org/sicp-manual/SICP_Picture_Language.html)
3. 如果需要让其他代码文件能使用 `(require "file-name.rkt")` 导入 `file-name.rkt` 的定义，在 `file-name.rkt` 末尾添加 `(provide (all-defined-out))` 一行。
5. 统一使用 `#lang racket` 而不是 `#lang sicp` 的一个差异是解释器在对 list 等数据结构求值时的响应，如有需要可以使用 `display` 或自定义过程打印。

## 3.3 勘误表和解题集

* [《计算机程序的构造和解释》勘误表](https://www.math.pku.edu.cn/teachers/qiuzy/books/sicp/errata.htm)
* [中文解题集](https://sicp.readthedocs.io/en/latest/)
	* [Github 仓库](https://github.com/huangzworks/SICP-answers)最后一次提交(commit)是在 2014 年 6 月 24 日，基本上也不会再更新缺的题目了。而且大多数由作者 `huangz` 一个人完成，一些答案质量不如英文解题集。
	* 作者使用的也是 MIT/GNU Scheme，创建项目时还没有 `sicp` 和 `sicp-pict` 库。
	* 虽然其中大多数链接已经失效，但也没有太多参考的必要，很多是只限于 MIT/GNU Scheme 的资源。
* [英文解题集](http://community.schemewiki.org/?SICP-Solutions)
	* 作为中文解题集的补充，交流人数多，不过不需要花太多时间看，写出答案只是次要的。

# 4 引用

* [How do we tell truths that might hurt?](https://www.cs.virginia.edu/~evans/cs655-S00/readings/ewd498.html) -- Edsger W. Dijkstra

