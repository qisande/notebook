# 将notebook上传到github

1. 生成ssh公钥

   > C:\Users\npj\\.ssh\id_rsa ：存放私钥的文件
   > C:\Users\npj\\.ssh\id_rsa.pub ：存放公钥的文件
   >
   > 
   >
   > 代码参数含义：
   > -t 指定密钥类型，默认是 rsa ，可以省略。
   > -C 设置注释文字，比如邮箱。
   >
   > 
   >
   > 执行命令后需要进行3次或4次确认：
   >
   > 1. 确认秘钥的保存路径（如果不需要改路径则直接回车）；
   >
   > 2. 如果上一步置顶的保存路径下已经有秘钥文件，则需要确认是否覆盖（如果之前的秘钥不再需要则直接回车覆盖，如需要则手动拷贝到其他目录后再覆盖）；
   > 3. 创建密码（如果不需要密码则直接回车）；（该密码是你push文件的时候要输入的密码，而不是github管理者的密码）
   >    当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到github上了
   >    这里我们选择按回车不输出密码
   >
   > 4. 确认密码；
   >
   >    
   >
   > 最后：复制id_rsa.pub的内容添加到github的ssh公钥处，这样向github提交时就不用输入密码了

   ```bash
   ssh-keygen -t rsa -C "sshkey"
   ```

   

2. 上传

   > GitHub官方

   ```bash
   # …or create a new repository on the command line
   echo "# test" >> README.md
   git init
   git add README.md
   git commit -m "first commit"
   git branch -M main
   git remote add origin git@github.com:pengjieniu/test.git
   git push -u origin main
   
   
   # …or push an existing repository from the command line
   git remote add origin git@github.com:pengjieniu/test.git
   git branch -M main
   git push -u origin main
   
   ```

   > 最终执行
   >
   > - `git add -A`（--all）添加所有内容，以便磁盘文件夹中的所有内容都显示在暂存区中，空文件夹也会推送
   > - `git add .`暂存所有内容，但不会移除已从磁盘删除的文件，空文件夹不会推送
   > - `git add -u`（--update）仅暂存已修改的文件，删除已从磁盘删除的文件，不添加新文件
   > - `git add <file name 1> <file name 2>`仅添加某些文件
   >
   > - `git add -A`适用于整个工作目录，`git add .`始终适用于当前目录
   >
   > 具体命令的解释，参见[这里](https://blog.csdn.net/wq6ylg08/article/details/89028412)

   ```bash
   # 在笔记文件夹目录下 打开git bash输入下列命令初始化本地仓库
   git init 
   
   # 将笔记本文件夹下所有文件进行跟踪
   git add -A # 提交所有变化
   
   # 生成一个本地库版本
   git commit -m "latest version"
   
   # 将本地分支设置成main
   git branch -m main
   
   # 对远程仓库url设置别名为origin
   git remote add origin git@github.com:niupengjie/notebook.git
   
   # 将本地仓库的本地分支推送到远程仓库的远程分支中
   git push -u origin main
   ```

   

# 学习经验：

- **人类的大脑擅长联想，对比，类比，创造，解释就是不擅长计算和记忆。**
  - 所以要在实践中去学习，以项目和问题为导向去学习。
  - 学习新科目和新知识的时候，最好一开始直接上手。先了解能解决什么问题、能用在什么地方。

- **学习要多份材料对照着看，死磕就是浪费时间**

  不如睡大觉

- **理解不了是因为缺少关键的认知拼图，不是因为笨**

  补全关键认知拼图的一种有效途径就是多阅读，多找不同材料从不同角度思考，这样会增加补全的可能性。

- **扩展知识面，有一个主线突出，剩下的部分慢慢根据问题去延申就好了**

  如果不是当前非常关心的问题，没有必要去读一本完整的专著。因为没有足够的动机的情况下，读了很快也忘了，和没有读过差别不大。

# AI在学习中的应用

- **阅读**

  翻译+自带OCR图片文字识别以及合适的GPTS的角色的情况下，外文书籍(不仅限于英文版，还有其他外语)的书籍阅读难度小很多。

- **讨论**

  虽然他的专业性有一定的问题，但是也不失为一个好的讨论对象（把他当成一个知识储备很大，但是学的一般般的同龄人对待就好了）。一边讨论一边写作的时候，往往最后都是自己搞懂了很多问题，但是这里面也离不开AI给我的启发和反馈(包括纠正它的错误)

- **代码**

  除了特别小众的，比如sagemath其余的大多数编程在自己有一定的基础上，编程效率提高至少五倍。