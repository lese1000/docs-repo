1、创建一个项目仓库
svnadmin create /home/huzza/test_svn

2、import初始文件到项目仓库中（本地的文件还不是工作副本）
svn import -m"comments" source_file_path remote_URL

3、checkout文件到本地目录（检出到本地的才是工作副本）
svn checkout(co) file:///home/huzza/test_svn/sesame/trunk sesame




4、检查文件状态或者日至
svn status [文件/目录]
svn log [文件/目录]
svn log -r6:8 [文件/目录]     查看版本6到版本8之间的日志

5、文件/目录比较（工作拷贝与项目创库中的比较）
svn diff [文件/目录]     或者      svn diff --rHEAD [文件/目录]     --rHEAD:表示最新版本

6、更新本地工作拷贝，提交工作修改
1）svn update(up) [文件/目录]       
update时的一些文件标志：
     U ------ 表示文件被更新
     G ------ 表示项目创库中的文件和本地工作拷贝的文件合并到了一起
     C ------ 表示合并时，有冲突产生
     A ------ 新加入了一个文件
     D ------ 删除了一个文件
     ? ------ 表示该文件或者目录没有被svn管理
     M ------ 该文件被修改
2）svn checkin(in) -m"comments" [文件/目录] 或者 svn commit(ci) -m"comments" [文件/目录]  《相当于通用概念：检入（checkin）》。

7、冲突。当svn update时，如果存在某个文件存在冲突，打开冲突的文件，<<<<<<<< 和 >>>>>>>> 表明了冲突发生的地方。
当冲突发生时，如果想使用项目仓库中的版本，而放弃本地拷贝的修改，可以使用以下命令：
a、svn revert [冲突的文件/目录]
b、svn update [冲突的文件/目录]
（svn resolved [文件/目录] && svn updata [文件/目录]，似乎也是ok的，需要confirm一下）
如果想保留本地工作拷贝的修改，而放弃项目仓库中版本的修改，可以如下：
a、cp 文件/目录.mine 文件/目录
b、svn resolved 文件/目录
c、svn ci -m "use my version please" 文件/目录
（在上面的三个步骤中，似乎不用做步骤a也可以达到目的）

8、使svn项目仓库联网
启动svn服务器：svnserve --daemon --root /home/huzza/test_svn
列服务器资源：svn list svn://192.168.0.4/sesame/trunk
出来后面的URL不同，其他操作的各部分均相同
svn+ssh 访问：svn list svn+ssh://192.168.0.4/sesame/trunk        (需要在服务器上支持ssh访问)

9、得到特定版本的工作拷贝
svn checkout -rVersionNum list svn://192.168.0.4/sesame/trunk butterfly
svn info butterfly     （查看当前版本拷贝的状态）

10、拷贝/移动文件
svn copy filename newfile
svn move oldfile newfile
svn ci -m "add or move some files" [修改文件所在的目录]      （这里确保服务器上也作跟本地拷贝相同的动作）

11、版本的符号
HEAD --------- 项目仓库中的最新版本
BASE --------- 工作拷贝的基准版本（也就是checkout出来时的版本）
COMMITTED ---- 最后一次checkin的版本
PREV --------- COMMITTED之前的一个版本

12、查找版本之间的差异
svn diff -r2:4 [文件/目录]
svn diff > diffname.patch (生成patch文件)
使用patch文件： patch -p0 -i diffname.patch

13、删除后一个版本对前一个版本的修改
svn merge -r27:26 [文件/目录] && svn ci -m "undo the work of version 27"
撤销版本27所做的修改

14、创建分支/标签
svn mkdir -m "Create branches" svn://192.168.0.4/sesame/branches
svn copy -m "Create release branches for version 1.0" svn://192.168.0.4/sesame/trunk \
svn://192.168.0.4/sesame/branches/release-1.0 



版本控制也称作Revision Control System(RCS)。

名词解释：

    修订版（revision）：可以认为是某个文件在其生命周期内各个保存的快照，每个快照和一个时间区间对应。
    版本库（Repository）：存放修订版的数据库
    本地工作拷贝（Local working copy）：修订版在本地的副本
    版本的检入（Check in）：本地副本提交到服务器的版本库
    检出（Check out）：从服务器的版本库中取出修订版成为本地副本
    版本号的来源：有两种策略，基于文件的计数和基于仓库的计数，subversion使用后者
    标签（Tags）：为版本加一个名字，便于检出
    分支（Branches）：修订版打分支，以后可以平行修改，互不干扰
    合并（Merging）：将分支的修订版合并为一个新的修订版
    锁（Locking）：为修订版枷锁
    冲突（Conflict）：并发版本控制时防止修订版混乱的错误机制

