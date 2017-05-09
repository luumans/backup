title: Sublime Mac 快捷键
date: 2017-03-21 18:29:20
description: 来到GitHub这么长时间，才开始真真的了解GitHub，这个国外的代码托管平台，充满着大牛的身影。
categories:
- Tool
tags:
- Sublime
toc: true
author:
comments:
original:
permalink: 
---

　　**自用笔记：**本文属于自用笔记，不做详解，仅供参考。在此记录自己已理解并开始遵循的前端代码规范。What How Why
<!-- more -->

### 符号说明
⌘：command 
⌃：control 
⌥：option 
⇧：shift 
↩：enter 
⌫：delete

### 全局
    ⌃ + ⇧ + T：打开文件夹控制台
    ⌘ + ⌥ + ⌃ + ->：网易云下一曲
    ⌘ + ⌥ + ⌃ + <-：网易云上一曲
    ⌘ + ⌥ + P：网易云暂停k

### 通用（General）
    ↑↓←→：上下左右移动光标，注意不是不是KJHL！
    Alt：调出菜单

### 整理（clear）
    Tab：缩进：自动完成
    Shift+Tab：去除缩进
    Ctrl+KT：折叠属性
    Ctrl+K0：展开所有

### 窗口（Window）
    ⌘ + 1、2、3：切换文件

### 移动（Move）
    ⌘ + <-：行首
    ⌘ + ->：行尾
    ⌘ + ↑：头部
    ⌘ + ↓：尾部
    ⌘ + ⇧ + ↑：向上全选
    ⌘ + ⇧ + ↓：向下全选
    ⌘ + ⌃ + ↑/↓：移动当前行
    Ctrl+←/→：进行逐词移动

<!--     Ctrl+Shift+←/→进行逐词选择
    Ctrl+↑/↓移动当前显示区域
    Ctrl+Shift+↑/↓移动当前行
    Ctrl+D：选择当前光标所在的词并高亮该词所有出现的位置，再次Ctrl+D选择该词出现的下一个位置，在多重选词的过程中，使用Ctrl+K进行跳过，使用Ctrl+U进行回退，使用Esc退出多重
### 编辑
    Ctrl+Shift+L：将当前选中区域打散
### 文件（File）
    Ctrl+N：在当前窗口创建一个新标签
    Ctrl+O：打开文件
    Ctrl+Shift+T：打开最近关闭的文件
    Ctrl+S：保存
    Ctrl+Shift+S：另存为
    Ctrl+Shift+N：创建新窗口
    Ctrl+Shift+W：关闭窗口
    Ctrl+W：关闭当前标签，当窗口内没有标签时会关闭该窗口
### 编辑（Edit）
    Ctrl+Z：撤销
    Ctrl+Y：恢复
### 取消选择（Undo Selection）
    Ctrl+U：智能撤销
    Ctrl+ Shift+U：智能重做
    Ctrl+ Shift+V：粘贴并缩进
    Ctrl+K，Ctrl+V：
### 行（Line）
    Ctrl +]：缩进
    Ctrl +[：反缩进
    Ctrl + Shift + Up：上移一行 
    Ctrl + Shift + Down：下移一行
    Ctrl + Shift + D：复制行(加倍)
    Ctrl + Shift + K：删除行
    Ctrl + J：连接行
### 文本（Text）
    Ctrl+Shift+Enter：在当前行上面增加一行并跳至该行
    Ctrl+Alt+Enter：替换所有关键字匹配
    Ctrl+Enter：在当前行下面新增一行然后跳至该行
    Ctrl+Delete：删除单词前部
    Ctrl+Backspace：删除单词后部
    Ctrl+K，Ctrl+K：从光标处删除至行尾
    Ctrl+K+Backspace：从光标处删除至行首
    Ctrl+T：前后调转
### 注释（Comment）
    Ctrl+/：注释（如已选择内容，同“Ctrl+Shift+/”效果）
    Ctrl+Shift：/：块注释(注释已选择内容)
    Ctrl+Alt+/：块注释，并Focus到首行，写注释说明用的
### 标签（Tag）
    Alt+.：闭合当前标签
    Ctrl+Shift+A：选择标签(可重复)
    Ctrl+Shift+W：选择区域被标签包含
### （Mark）
    Ctrl+K， Alt+Space：设置记号
    Ctrl+K，Alt+A：选择到记号
    Ctrl+K，Alt+W：删除到记号
    Ctrl+K，Alt+S：交换(移动)记号
    Ctrl+K，Alt+G：移除记号
    Ctrl+K，Alt+Y：Yank
    Ctrl+K，Alt+J：取消所有折叠
### 代码折叠（Code Folding）
    Ctrl+Shift+[：折叠代码
    Ctrl+Shift+]：展开代码
    （Convert Case）
    Ctrl+K，Ctrl+U：改为大写
    Ctrl+K，Ctrl+L：改为小写
### （Wrap）
    Alt+Q：
    Ctrl+Space：显示提示
    F9：按行排序
    Ctrl+F9：按行排序(区分大小写)
### 选择（Selection）
    Ctrl+ Shift+L：分割为多光标(选择多行时)
    Ctrl+ Alt +Up：向上一行添加光标
    Ctrl+ Alt +Down：向下一行添加光标
    Escape单光标
### 扩展（Expand）
    Ctrl+A：全选
    Ctrl+L：选择整行（按住-继续选择下行）
    Ctrl+D：选词：（按住-继续选择下个相同的字符串）
    Ctrl+Shift+Space：快速选择当前作用域（Scope）的内容
    Ctrl+Shift+M：快速选择括号间的内容{}
    Ctrl+Shift+J：快速选择同缩进的内容
    Ctrl+Shift+A：选择光标位置父标签对儿
### 查找（Find）
    Ctrl+F：进行标准查找
    F3：跳至当前关键字下一个位置
    Shift+F3：跳到当前关键字上一个位置
    Ctrl +I：
    Ctrl +H：进行标准替换
    Ctrl+Shift+H：替换当前关键字
    Ctrl +F3：快速查询
    Alt +F3：选中当前关键字出现的所有位置
    Ctrl+D：快速查询下一个(多光标)
    Ctrl+K，Ctrl+D：快速查询跳过下一个(多光标)
    Ctrl+E：字
    Ctrl+Shift+E：字
    Ctrl+Shift+F：多文件搜索&替换
### 视图（View）
    Ctrl+K，Ctrl+B：侧边栏开关Side Bar
    Ctrl+`：调出控制台
    F11：切换普通全屏
    Shift+F11：切换无干扰全屏
    Alt+Shift+2：进行左右分屏
    Alt+Shift+5：进行上下左右分屏
    Alt+Shift+8：进行上下分屏。
    分屏，使用Ctrl+数字键跳转到指定屏，使用Ctrl+Shift+数字键将当前屏移动到指定屏
### 组（Group）：
    Ctrl+K，Ctrl+Up：
    Ctrl+K，Ctrl+ Shift+ Up：
    Ctrl+K，Ctrl+Down：
### 焦点小组（Focus Group）：
    Ctrl+K，Ctrl+Right：
    Ctrl+K，Ctrl+ Left：
    Ctrl+1：组间切换焦点
    Ctrl+ Shift +1：移动文件到组
    Syntax语法和文件类型、indentation缩排、Line Endings行尾结束符号
    F6：拼写检查
    Ctrl + F6：下一个错误
    Ctrl+Shift+ F6：上一个错误
### 跳转（Goto）
    Ctrl+P：跳转到指定文件
    Ctrl+R：跳转到指定符号
    Ctrl+Shift+R：
    F12：
    Ctrl+G：跳转到指定行号
    Alt+-：跳转到底部
    Alt+Shift +-：
### 文件开关（Switch File）
    Ctrl+Pagedown：下一个文件
    Ctrl+Pageup：上一个文件
    Ctrl+Tab：下一个文件(stack)
    Ctrl+Shift + Tab：上一个文件(stack)
    Alt+O：
    Alt+1：最近打开文件
### 滚动（Scroll）
    Ctrl+K，Ctrl+C：滚动到光标处
    Ctrl+Up：向上滚动一行(定光标)
    Ctrl+Down：向下滚动一行(定光标)
### 书签（Boolmarks）
    Ctrl+F2：设置书签
    F2：下一个书签
    Shift+F2：上一个书签
    Ctrl+Shift+F2：清除书签
    Alt+F2：全选书签
    Ctrl+M：在起始括号和结尾括号间切换
### 工具（Tools）
    Ctrl+Shift+P：调出命令板（Command Palette）
    Ctrl +B：
    Ctrl+Shift+B：
    Ctrl +Break：
    F4：
    Shift+ F4：
    Ctrl +Q：
    Ctrl+Shift+Q：
### 项目（Project）
    Ctrl+Alt+P：切换项目
    
#### 首选项（Preferences）
    Ctrl+ Keypad Plus：
    Ctrl+Shift+Keypad Plus：
    Help（帮助）


### Chrome
    ⌘ + ⌥ + J：调试工具
    ⌘ + 1、2、3：切换文件
    ⌘ + ⌥ + L：下载
    ⌘ + R：刷新
    
#### 标签页和窗口快捷键
    ⌘-N 打开新窗口。
    ⌘-T 打开新标签页。
    ⌘-Shift-N   在隐身模式下打开新窗口。
    按 ⌘-O，然后选择文件。   在 Chrome 浏览器中打开计算机中的文件。
    按住 ⌘ 的同时点击链接。或用鼠标中键（或鼠标滚轮）点击链接。 从后台在新标签页中打开链接。
    按住 ⌘-Shift 的同时点击链接。或按住 Shift 键的同时用鼠标中键（或鼠标滚轮）点击链接。  在新标签页中打开链接并切换到刚打开的标签页。
    按住 Shift 键的同时点击链接。  在新窗口中打开链接。
    ⌘-Shift-T   重新打开上次关闭的标签页。Chrome 浏览器可记住最近关闭的 10 个标签页。
    将标签页拖出标签栏。  在新窗口中打开标签页。
    将标签页从标签栏拖到现有窗口中。    在现有窗口中打开标签页。
    同时按 ⌘-Option 和向右箭头键。    切换到下一个标签页。
    同时按 ⌘-Option 和向左箭头键。    切换到上一个标签页。
    ⌘-W 关闭当前标签页或弹出窗口。
    ⌘-Shift-W   关闭当前窗口。
    点击并按住浏览器工具栏中的后退或前进箭头。   在新标签页中显示浏览历史记录。
    按 Delete 或 ⌘-[  转到当前标签页的上一页浏览历史记录。
    按 Shift-Delete 或 ⌘-]    转到当前标签页的下一页浏览历史记录。
    按住 Shift 键的同时点击窗口左上角的 + 按钮。 最大化窗口。
    ⌘-M 最小化窗口。
    ⌘-H 隐藏 Chrome 浏览器。
    ⌘-Option-H  隐藏其他所有窗口。
    ⌘-Q 关闭 Chrome 浏览器。
    
#### Chrome 浏览器功能快捷键

    ⌘-Shift-B   打开和关闭书签栏。
    ⌘-Option-B  打开书签管理器。
    ⌘-, 打开“偏好设置”对话框。
    ⌘-Y 打开“历史记录”页。
    ⌘-Shift-J   打开“下载内容”页。
    ⌘-Shift-Delete  打开“清除浏览数据”对话框。
    
#### 地址栏快捷键

    键入搜索字词，然后按 Enter 键。 使用默认搜索引擎进行搜索。
    键入搜索引擎关键字，按空格键，然后键入搜索字词，再按 Enter 键。 使用与关键字相关联的搜索引擎进行搜索。
    首先键入搜索引擎网址，然后在系统提示时按 Tab 键，键入搜索字词，再按 Enter 键。   使用与网址相关联的搜索引擎进行搜索。
    键入网址，然后按 ⌘-Enter。   在新后台标签页中打开网址。
    ⌘-L 突出显示网址。
    ⌘-Option-F  在地址栏中输入“?”。在问号后键入搜索字词可用默认搜索引擎执行搜索。
    同时按 Option 和向左箭头键。  将光标移到地址栏中的前一个关键字词
    同时按 Option 和向右箭头键。  在地址栏中将光标移到下一个关键字词
    同时按 Shift-Option 和向左箭头键。    在地址栏中突出显示上一关键字词
    同时按 Shift-Option 和向右箭头键。    在地址栏中突出显示下一关键字词
    ⌘-Delete    在地址栏中删除光标前的字词
    在地址栏菜单中按 Page Up 或 Page Down。   在菜单中选择上一条目或下一条目。
    
#### 网页快捷键

    ⌘-P 打印当前网页。
    ⌘-Shift-P   打开“网页设置”对话框。
    ⌘-S 保存当前网页。
    ⌘-Shift-I   通过电子邮件发送当前网页。
    ⌘-R 重新载入当前网页。
    ⌘-, 停止载入当前网页。
    ⌘-F 打开查找栏。
    ⌘-G 在查找栏中查找下一条与输入内容相匹配的内容。
    ⌘-Shift-G 或 Shift-Enter 在查找栏中查找上一条与输入内容相匹配的内容。
    ⌘-E 使用所选内容查找。
    ⌘-J 跳到所选内容。
    ⌘-Option-I  打开“开发人员工具”。
    ⌘-Option-J  打开“JavaScript 控制台”。
    ⌘-Option-U  打开当前网页的源代码。
    按住 Option 键，然后点击链接。 下载链接目标。
    将链接拖到书签栏中。  将链接保存为书签。
    ⌘-D 将当前网页保存为书签。
    ⌘-Shift-D   将所有打开的标签页以书签的形式保存在新文件夹中。
    ⌘-Shift-F   在全屏模式下打开网页。再按一次 ⌘-Shift-F 可退出全屏模式。
    ⌘-+ 放大网页上的所有内容。
    ⌘ 和 -   缩小网页上的所有内容。
    ⌘-0 将网页上的所有内容恢复到正常大小。
    ⌘-Shift-H   在当前标签页中打开主页。
    空格键 向下滚动网页。
    ⌘-Option-F  搜索网页。
    
#### 文本快捷键

    ⌘-C 将突出显示的内容复制到剪贴板中。
    ⌘-Option-C  将您正在查看的网页的网址复制到剪贴板中。
    ⌘-V 从剪贴板中粘贴内容。
    ⌘-Shift-Option-V    粘贴内容并应用周围文本的格式。
    ⌘-X 或 Shift-Delete  删除突出显示的内容并将其复制到剪贴板中。
    ⌘-Z 撤消最后一步操作。
    ⌘-Shift-Z   重复最后一步操作。
    ⌘-X 删除突出显示的内容并将其保存到剪贴板中（剪切）。
    ⌘-A 选择当前网页上的所有文本。
    ⌘-: 打开“拼写和语法”对话框。
    ⌘-; 检查当前网页上的拼写和语法。 -->