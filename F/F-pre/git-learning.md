![[Pasted image 20260625175739.png]]
![[Pasted image 20260625175752.png]]
![[Pasted image 20260625175805.png]]
### 初始化
```Bash
# 初始化仓库
git init 

# 登录个人信息(仅对当前仓库生效)
git config --local user.name "你的名字"
git config --local user.email "your@email.com" # 注意这里的邮箱与远程仓库保持一致

# 可以先编写一个.gitignore文件防止之后不必要文件被commit
# .gitignore 示例 
# 忽略 Verilator 编译生成的 C++ 目标文件目录 
obj_dir/ 
# 忽略所有的波形文件（GTKWave 查看用的） 
*.vcd *.fst 
# 忽略编译出来的二进制可执行文件 
*.out *.exe 
# 忽略 VSCode 等编辑器的本地缓存 
.vscode/

# 如果是在 Windows PowerShell 里操作，为防止回车换行符与linux不一致，则需要加上下面的命令杜绝后患
git config --global core.autocrlf false

# 如果要上Github则还需以下步骤

# 登录你的 GitHub 账号，点击右上角的 “+” 号，选择 “New repository”。  
# Repository name（仓库名）：（与你本地的工程代号保持一致）
# 注意不要勾选 Add a README file 不要勾选 Add .gitignore 不要勾选 Choose a license 会与本地提交发生冲突

# 绑定远程地址，origin代表该项目的默认远程仓库
git remote add origin https://github.com/你的用户名/ysyx-workbench.git
# 统一分支命名，本地主分支为master，不符合GitHub规定
git branch -M main
# 首次推送，-u origin main代表以后默认git push到origin的main分支上
git push -u origin main
```
**这里引用一段Gemini的话帮助初学者更加善于利用GitHub**
>如何地道地管理你的 GitHub 仓库？
>GitHub 绝对不仅仅是一个代码云盘，它是开源社区的社交名片和项目管理中枢。针对你正在进行的硬核芯片学习，推荐以下三个管理习惯：
>1. 把 Issues 当作你的“个人看板”与“错题本”
>>**场景：** 做到 5 级流水线时，你发现数据前推（Forwarding）逻辑有问题，但今天实在太累了不想修。
>>**操作：** 去 GitHub 仓库页点击 **Issues** -> **New issue**，写下：“流水线 EX 阶段旁路数据错误”，甚至贴上你的报错波形截图。
>>**价值：** 这比写在纸上或记事本里好一万倍。当你以后修好了这个 Bug，在 Git 提交时写一句 `fix: resolve forwarding data hazard (closes #1)`。推送上去后，GitHub 会自动帮你把这个 Issue 标记为已解决（Closed），这种正反馈极度让人上瘾。 
>2. 精心打磨你的门面：README.md
>>当你进入开源社区，没人有空去看你成百上千行的 SystemVerilog 代码，大家第一眼看的一定是仓库首页的 `README.md`。
>>**写什么：** 不要只写一句“这是我的 CPU”。写明你的架构设计图、你在哪个指令上卡了三天、你是怎么用 Verilator 搭建仿真环境的、甚至贴上一张你的 CPU 成功跑通系统内核时的终端截图。
>>**价值：** 这是你未来找实习或向大佬请教时，最具说服力的技术名片。
>3. 善用 GitHub Insights 洞察自己的进度
>>点击仓库的 **Insights** 标签页，里面有你的代码提交频率（Commit 活动图）、代码行数统计。看着那张绿色的“提交格子图”一天天被填满，是你在这场孤独的硬件马拉松中最好的精神氮泵。
### 基础操作（未完）
```Bash
# 查询当前更改文件提交状态，养成经常查状态的习惯是好的
git status

# 将新增的文件加到暂存区，add . 会将所有更改写入（不建议，不符合原子化提交），add 具体文件名需要注意，地址为仓库根目录下的相对地址即可，尽量为英文字母且不包含空格
git add 你的文件名

# 查看差异
git diff --staged # 比较暂存区 vs 上一次 commit，看你 add 了什么
git diff # 比较工作区 vs 暂存区，看你改了但还没 add 的内容

# 提交为本地仓库的一个版本，描述需要注意格式（全部英文小写，README Makefile LICENSE必须保持原样，注意:后有一个空格）："类型: 祈使句式描述" 这些规定仅为规范化！
# 类型如下：
# 1. `feat` (Feature - 新功能) 当你为 CPU 添加了新的模块、新的指令或新的机制时使用。
# 2. `fix` (Fix - 修复缺陷) 当你修复了波形图里跑出来的时序错误或逻辑 Bug 时使用。
# 3. `refactor` (Refactor - 重构) 代码既没有增加新功能，也没有修复 Bug，仅仅是优化了写得太烂的旧代码（比如把一堆乱七八糟的 `if-else` 换成了优雅的 `case` 语句）。
# 4. `docs` (Documentation - 文档) 仅仅修改了 README.md 或是代码里的注释说明。
# 5. `test` (Test - 测试) 专门针对测试用例的提交，不涉及 CPU 本体逻辑的改动。
# 6. `style` (Style - 格式) 不影响代码运行的纯格式调整（比如删除了多余的空格、统一了缩进、替换了变量名）。
# 7. `chore` (Chore - 杂项/构建) 用于构建过程、辅助工具、配置文件的变动。
git commit -m "简要的描述"

# 如果发现上次的提交也缺失或者上次提交的描述需要修改，如果觉得描述不需要修改，则将-m以及后面的内容修改为--no-edit即可
git commit --amend -m "描述"

# 查看提交历史，HEAD -> main表示 HEAD 当前指向 main 分支的最新 commit。HEAD 记录的就是「你当前在哪个 commit 上」——切换分支时，HEAD 会跟着移动。
# git log -n 3 — 只看最近 3 条   git log --oneline — 简洁模式（一行一条）
git log

# 恢复文件，没有暂存过就恢复成最后一次提交的样子
git restore 工作区中想退回之前暂存区的样子的文件

# 将更改提交至远程仓库
git push
```
### FAQ
- **刚开始add了一个文件到暂存区后面又加了一个文件到暂存区，这次commit是一起提交的吗？**
  是的，它们是一起被提交的。
  在 Git 的逻辑里，git commit 就像是按下相机的快门。当快门按下的那一瞬间，Git 会把当前暂存区（Staging Area，也就是你的“购物车”）里的所有文件打包，拍成一张快照并永久存档。


- **第一次提交为一个版本，发现有问题可以回退吗？**
  直接回退会报错，报错信息 `fatal: ambiguous argument 'HEAD~1': unknown revision` 翻译过来是“未知的版本”。 原因在于，`HEAD~1` 的意思是“退回到当前节点的上一个节点”。但是，这是你在当前仓库的**第一次提交（Root Commit）**！就像宇宙大爆炸的奇点一样，它没有“上一步”可以退。Git 找不到更早的历史，所以报错了。对于这“开天辟地”的第一次提交，我们不能用常规的撤销（reset），而是要用 Git 的另一招神技：**修改最后一次提交（Amend）**。`git rm --cached 还需更改的文件 git commit --amend -m "对第一个版本的描述"`


- **还未提交但是放入暂存区的文件想重新提交怎么办？**
   `git rm --cached 还需更改的文件` 更改完成再add即可；如果不是第一个版本，还可以通过`git restore --staged 想重新修改的文件` 


- **在 Git 中，LF 将被替换为 CRLF - 这是什么？它重要吗？**
   **格式和空白**格式和空格问题是许多开发者在协作时（尤其是在跨平台协作时）会遇到的一些令人沮丧且不易察觉的问题。补丁或其他协作工作很容易引入细微的空格更改，因为编辑器会在后台默默地进行修改；而且，如果你的文件最终进入 Windows 系统，它们的行尾符可能会被替换。Git 提供了一些配置选项来帮助解决这些问题。`core.autocrlf`如果你在 Windows 系统上编程，而你的同事却不在同一个系统上（反之亦然），那么你很可能会遇到换行符的问题。这是因为 Windows 系统在文件中同时使用回车符和换行符来表示换行，而 Mac 和 Linux 系统只使用换行符。这是跨平台工作中一个不易察觉却极其恼人的问题；许多 Windows 编辑器会在用户按下回车键时，悄悄地将原有的 LF 换行符替换为 CRLF，或者同时插入回车符和换行符。Git 可以通过在向索引添加文件时自动将 CRLF 行尾符转换为 LF，以及在将代码检出到文件系统时自动将 CRLF 行尾符转换为 LF 来处理这个问题。您可以使用 core.autocrlf 设置启用此功能。如果您使用的是 Windows 系统，请将其设置为 true——这样在检出代码时就会将 LF 行尾符转换为 CRLF：

```
$ git config --global core.autocrlf true
```
   如果您使用的是 Linux 或 Mac 系统，并且系统使用 LF 行尾符，那么您不希望 Git 在检出文件时自动转换行尾符；但是，如果意外引入了使用 CRLF 行尾符的文件，您可能希望 Git 进行修复。您可以通过将 core.autocrlf 设置为 input 来告诉 Git 在提交时将 CRLF 转换为 LF，但反之则不然：
```
$ git config --global core.autocrlf input
```
   这样设置后，Windows 检出的文件将以 CRLF 结尾，而 Mac 和 Linux 系统以及存储库中的文件将以 LF 结尾。
   如果您是一名 Windows 程序员，并且正在开发一个仅限 Windows 平台的项目，那么您可以通过将配置值设置为 false 来关闭此功能，从而在存储库中记录回车符：
```
$ git config --global core.autocrlf false
```

- 