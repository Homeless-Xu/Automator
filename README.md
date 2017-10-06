# Automator

    • 自动化 Mac 常用操作!    
    • 带✔︎的都是亲测可用的!
    • 使用方法: 下载对应ZIP文件, 解压就能用.
    • 自定义修改方法: 参考下面介绍,有点IT基础很容易改的!




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 01: mp4/m4v 视频无损合并 2017-10-5-00 ✔︎ 🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸 Version  1.0 🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸

🔸 why  

    视频分割很容易.  100M的一个视频,分成两个小视频.这两个小视频的总和还是100M.
    视频合并却很难! 10M 的视频 + 20M 的视频 合并出来往往是100M+ 的! 
    绝对不是你想要的30M! 你要的是无损合并视频! 你得用 ffmpeg 这个软件


🔸 ffmpeg 原理 

    FFMPEG 是个专业的视频处理软件.
    这个软件不是那么易用. 如果你单纯的只是想合并视频只要用三条命令就好!

    ffmpeg -i 1.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts input1.ts
    ffmpeg -i 2.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts input2.ts
    ffmpeg -i "concat:input1.ts|input2.ts" -c copy output.mp4

    1. 首先你要把你要合并的两个视频文件放到某个地方 比如桌面.
    2. 如果你不想改命令, 就重命名两个视频文件为 1.mp4 和2.mp4 
    3. 打开终端, cd 到视频文件所在地: 桌面 
    4. 输入第一条命令; 然后你会发现桌面的 1.mp4 变成 input1.ts 了
    5. 输入第二条命令; 然后你会发现桌面的 2.mp4 变成 input2.ts 了
    6. 输入第三条命令; 然后你会发现桌面多了一个 的 output.mp4

    要用第三条命令.首先需要把视频文件格式变成 ts 格式. 
    这就是为什么首先要执行前面两条命令.


🔸 Automator 自动合并视频

    自动化时代,当然也可以实现自动化合并视频了.
    你要做的只是写一个Automator, 
    然后你只要把任意两个视频拖到这个 Automator上. 就会自动合并成一个了!
    下面我简单说说 怎么写 Automator 


🔸 ffmpeg 命令路径

    不知为啥automator里的各种shell都不能直接使用 ffmpeg命令. 
    哪怕设置了环境路径也不行, 那就用ffmpeg的全路径来执行命令吧

    用 brew 安装的 软件绝大多数都是安装在 /usr/local/Cellar/ 文件夹下的.
    ffmpeg 的真实安装路径是 /usr/local/Cellar/ffmpeg/3.3.4 
    
    全路径就是: /usr/local/Cellar/ffmpeg/3.3.4/bin/ffmpeg


🔸 Automator 

    1. 运行 Automator ➜ 选择 Application ➜ choose 

    2. 搜索栏目搜索并添加: get selected finder items 

    3. 搜索栏目搜索并添加: Run Shell Script

    4. Pass input: 必须选择 as arguments
        上面选择Finder items 的作用就是给下面的脚本提供选中文件的文件名!
        文件名要传入脚本. 这里必须设置成  as arguments
        两个视频文件也就是两个变量! 脚本里就可以用 $1,$2 来表示两个视频文件名.

    5. shell: 一般都是 bash/zsh

    6. 复制下面5行到脚本内容里面

        cd ~/Desktop
        /usr/local/Cellar/ffmpeg/3.3.4/bin/ffmpeg -i "$1" -c copy -bsf:v h264_mp4toannexb -f mpegts input1.ts -y
        /usr/local/Cellar/ffmpeg/3.3.4/bin/ffmpeg -i "$2" -c copy -bsf:v h264_mp4toannexb -f mpegts input2.ts -y
        /usr/local/Cellar/ffmpeg/3.3.4/bin/ffmpeg -i "concat:input1.ts|input2.ts" -c copy output.mp4 -y
        rm input1.ts && rm input2.ts


        第一行是必须的. 不指定路径, 你生成的很有可能不在桌面的.而是在/home 目录的.
        第二第三两行是把 两个mp4文件 先转换为 ts 格式的, 这样让ffmpeg来合并!
        第四行是删除临时转换文件的. ts 格式的 你也用不到.
        注意: 这里只适合两个mp4 文件的合并! 要多个你自己改. 很简单的!
        注意: /usr/local/Cellar/ffmpeg/3.3.4/bin/ffmpeg 这个路径改成你自己的. 特别是 3.3.4 版本号极有可能不一样.

    7. 保存 Automator


🔸 使用:  鼠标选择两个桌面上的 mp4 文件 ➜ 拖到新建的Automator 上就可以自动合并了.



🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸 Version  2.0 🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸

🔸 简介 
    https://www.cnbeining.com/2014/05/dealing-with-cat-all-on-video-non-destructive-merge-mainly-h-264-problem/

    参考了这篇文章后, 发现 MP4box 在视频合并方面比 FFmpeg 更好用.
    iphone 拍出来的格式是 m4v的...  这么合并呢.
    FFmpeg 要合并 m4v 得用别的命令. 
    mp4box 可以直接合并 mp4、m4v ... 
    据说还能混合格式直接合并 这个没试过...

    MP4box 手动下载地址:
    https://www.videohelp.com/software/MP4Box/old-versions#downloadold
    下载: gpac-0.7.0-rev27-g0639a09-master.dmg

    MP4box brew安装方式: brew install mp4box 就好了!!
    ==> Pouring gpac-0.7.1.sierra.bottle.tar.gz
    🍺  /usr/local/Cellar/gpac/0.7.1: 126 files, 5MB

    设置环境路径:  /usr/local/Cellar/gpac/0.7.1/bin

    使用: /usr/local/Cellar/gpac/0.7.1/bin/MP4Box -h  ➜  MP4Box 必须区分大小写

       语法: /usr/local/Cellar/gpac/0.7.1/bin/MP4Box -cat "$1" -cat "$2" output.m4v
    m4v实例: /usr/local/Cellar/gpac/0.7.1/bin/MP4Box -cat 1.m4v -cat 2.m4v output.m4v
    mp4实例: /usr/local/Cellar/gpac/0.7.1/bin/MP4Box -cat 1.mp4 -cat 2.mp4 output.mp4

    由于不同的文件格式, 输出的格式也是不一样的. 
    所以要先判断文件格式,执行对应的命令,输出对应的格式.
    而且我们暂时只支持 mp4 m4v 两种格式!

    脚本判断文件格式: 
    首先给 $1 $2 变量分别设置一个变量名.
    filename="$1"
    filename2="$2"

    判断 filename / filename2 的格式要么是 mp4 要么是 m4v, 不是就退出脚本!
    if [ "${filename##*.}" = "mp4" -o "${filename##*.}" = "m4v" ]
        then echo "filename is ok"
        else exit
    fi


    if [ "${filename2##*.}" = "mp4" -o "${filename2##*.}" = "m4v" ]
        then echo "filename2 is ok"
        else exit
    fi


    如果两个文件都是mp4 则输出mp4 格式; 如果两个文件都是 m4v 则输出m4v格式.
    注意脚本里是用 -a 来表示 and 与 关系; 用-o 来表示 or 或者关系; 用! 表示否定.

    if [ "${filename##*.}" = "mp4" -a "${filename2##*.}" = "mp4" ];
        then /usr/local/Cellar/gpac/0.7.1/bin/MP4Box -cat filename -cat filename2 output.mp4;
        echo "mp4 made";
    fi

    if [ "${filename##*.}" = "m4v" -a "${filename2##*.}" = "m4v" ];
        then /usr/local/Cellar/gpac/0.7.1/bin/MP4Box -cat "$1" -cat "$2" output.m4v;
        echo "m4v made";
    fi






🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 02-显隐系统文件 2017-10-5-15 ✔︎ 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸 显隐文件 
        
    Mac 10.11 +  可以直接用 ⌘+⇧+. 来快速显隐了

    Mac 10.8 + 的操作系统
        defaults write com.apple.finder AppleShowAllFiles -bool true; killall Finder   ➜ 显示文件并重启Finder
        defaults write com.apple.finder AppleShowAllFiles -bool false; killall Finder  ➜ 隐藏文件并重启Finder

    Mac 10.8 - 的系统
        defaults write com.apple.finder AppleShowAllFiles TRUE ; killall Finder        ➜ 显示文件并重启Finder
        defaults write com.apple.finder AppleShowAllFiles FALSE ; killall Finder       ➜ 隐藏文件并重启Finder


🔸 Automator 详解

    只要执行脚本就可以了. 就4行. 非常简单
    用read 命令判断当前显隐状态, 如果当前是显示的, 那么就进行隐藏...

    STATUS=`defaults read com.apple.finder AppleShowAllFiles`

    if [ $STATUS == 1 ]
        then  defaults write com.apple.finder AppleShowAllFiles -bool false; killall Finder 
        else  defaults write com.apple.finder AppleShowAllFiles -bool true; killall Finder
    fi



🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 03-隐藏/不隐藏某文件 2017-10-5-15 ✔︎ 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸 why 

    显隐系统文件那个机器人只能控制是否显示隐藏文件. 
    如果你想把某个正常文件 设置成为隐藏文件就需要用另外一个命令.
        chflags hidden ~/Desktop/1.mp4    ➜ 把 1.mp4 设置为隐藏文件
        chflags nohidden ~/Desktop/1.mp4  ➜ 把 1.mp4 设置为正常文件

    当然隐藏了某个文件后,要查看的话 用上面的 显隐系统文件就可以了


🔸 Automator 简解 


🔸 Todo

    现在是两个功能分开的. 隐藏文件一个机器人. 不隐藏文件另外一个机器人.
    想要 用一个机器人来实现 隐藏/不隐藏的切换. 
    这就需要判断 一个文件是否是隐藏文件.. 这个我不会! 求助大家. 

　


🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 04-锁定/解锁文件 2017-10-5-16 ✔  ︎🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸 why 

    有些重要文件,你需要特别注意.不要被自己/他人随便删除了!这时候你就可以用 这个文件加锁功能.
    其实这个是文件的一个额外属性,很少用,但是非常有用! 类似Linux文件的普通权限: 读/写/执行
    该属性的作用就是, 就算你有电脑的最高控制器都不能直接删除文件. 除非你先解锁这个文件

    属性	           chflags中使用	           详述
    系统级只能添加	  sappnd, sappend	         文件不能够截断或者复写(overwrite)，只能通过append模式添加内容
    用户级只能添加	  uappnd, uappend	         文件不能够截断或者复写(overwrite)，只能通过append模式添加内容
    系统级只读		schg, schange, simmutable  不能够重命名、移动、删除、更改内容
    用户级只读		uchg, uchange, uimmutable  不能够更改内容

    命令详细参考 http://zhengyi.me/2016/06/02/learning-shell-chflags/


    ⦿ 查看文件的额外属性: lsattr
        这个命令只适合Linux. mac 是不能用的. mac 就用ll将就看下就好了.
        ✘✘∙𝒗 Desktop ll 1.mp4
        -rw-------@ 1 v  staff    10M Sep 24 11:30 1.mp4  ➜ 有@ 就说明该文件是有额外属性的.

    ⦿ 修改文件的额外属性: chflags 命令;  类似 chown 来修改权限.
      chflags ➜ change file flags 


🔸 uappend 只读属性 

    chflags -R uappend filename  ➜ 加锁文件
    chflags -R nouappend filename ➜ 解锁文件


🔸 Automator 详解

    1. Get selected Finder items 
    2. Run shell script 
    3. Run shell script 的 shell 选择 bash 
    4. Run shell script 的 pass input 选择 as arguments 
    5. Run shell script 的加锁脚本内容:
        for f in "$@"
        do
            chflags -R uappend "$f"
        done


    5. Run shell script 的解锁脚本内容:
        for f in "$@"
        do
            chflags -R nouappend "$f"
        done



🔸 Automator 用法

    两个机器人, 一个加锁, 一个解锁.
    把想要加锁的文件拖到 加锁的机器人上面. 然后你就删不了那个文件了! 但是还能正常改.
    把加锁后的文件拖到 解锁的机器人上面. 就恢复正常了.能改`能删.


















🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 图片 2017-10-6-13 ✔︎ 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸 图片旋转

    机器人自带的 Rotate images ➜ 可以选择左转90° / 右转90° / 转 180°


🔸 图片缩放
    机器人自带的 Scale images ➜ 选择 To Size (pixels) ➜ 然后输入值 就可以自动转换大小了.
    图片缩放前,最好备份下原始图片(Copy Finder items)




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 自动SSH登录 2017-10-6-13 ✔︎ 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸 why

    服务器有点多, 比如3个云vps; 还有几个本地虚拟机, IP 各种记不住. 那么试试机器人吧,
    双击机器人就可以自动打开 iTerm 然后自动连接预设好的 ssh 服务器.
    这里由于要操作 Mac 的程序(iTerm) 所以建议使用 AppleScript 而不是 Bash.


🔸 Automator 详解

    ⦿ Launch Application ➜ 选择要拉开的软件,这里选 iTerm, 你没装的话建议你先装 非常好用.

    ⦿ Run AppleScript

        on run {input, parameters}
            # iterm 用法: 脚本编辑器 ➜ 文件 ➜ 打开字典 ➜ iterm 	       
            tell application "iTerm"
                activate
                # 打开 iterm这个软件.并激活
                tell current window
                    create tab with default profile
                end tell
                # 新建一个tab窗口
                tell current session of current window
                    # split horizontally with default profile
                    # 把当前窗口 上下均分成两个窗口.
                    write text "ssh -p 2222 root@23.105.192.96"
                end tell
            end tell
            return input
        end run


🔸 Automator 自定义 

    你要用的话需要自己先设置 ssh 免密钥访问.
    write text "ssh -p 2222 root@23.105.192.96"

    -p 2222  这个是指定ssh 端口的, 你没修改过端口就用下面的
    你只要把 23.105.192.96 改成你自己服务器的IP就可以了.
    write text "ssh root@23.105.192.96"




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵  新建文件 2017-10-6-13 ✔︎ 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸 WHY

    大家都知道 Mac 是没有 右键新建文本文件的功能的! 
    双击机器人就可以在当然文件夹(桌面) 新建一个空白的文件
    这是机器人自带的功能.不用写脚本的.


🔸 Automator
    
    搜索并添加: New Text File 
    • File Format ➜ same as input text ➜ 选择新文件的格式 (txt) 
    • Save as: ➜ Newfile.txt   ➜ 这个是设置新文件的文件名的. 可自定义
    • Where: ➜ Desktop ➜ 设置新文件位置.一般都桌面... ➜ 记得打勾 replace existing files
    • Encoding: ➜ UTF-8 ➜ 文本编码.





🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵 文件添加后缀 2017-10-6-13 ✔︎ 🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔸 WHY 

    有些视频文件下下来. 居然没有后缀.要自己加.几个视频手动改无所谓..
    但是一下就是几十个视频 就懒得手动改了,用机器人吧.
    这个也是机器人自带的功能. 非常简单
    你只要把所有文件拖到下面机器人上面.就会给所有文件加上 .mp4 后缀.


🔸 Automator 
    
    搜索并添加 Rename Finder Items: Add Text
        • Add Test
        • Add: .mp4 
        • as extension




🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵   Misc   🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵

🔵 内存释放 ?

    purge命令可以清除内存和硬盘的缓存，与重启Mac的效果差不多。
    purge命令可以让不活跃的系统内存转变为可以使用的内存。你只需在终端中输入下面的命令即可。
    sudo purge

好像没什么效果啊..




🔵 使用open命令开启多个相同应用

    open命令可以在终端中开启应用，使用-n可以开启多个相同应用。比如你可以使用下面的命令开启新Safari窗口

    open -n /Applications/Safari.app/



🔵 创建有密码保护的压缩文件 ✔︎

    你可以通过下面的命令将桌面上的macx.txt文件创建成有密码保护压缩文件protected.zip。

    zip -e protected.zip ~/Desktop/1.mp4



🔵 调整 Launchpad 中应用图标的行数和列数 ✔︎
    默认是5行 7列, 可以随意调整,改下面的 13 和 9 就可以了
    注意下面命令会初始化你的排列. 恢复成新电脑适合的排列.

    defaults write com.apple.dock springboard-columns -int 13;
    defaults write com.apple.dock springboard-rows -int 9;
    defaults write com.apple.dock ResetLaunchPad -bool TRUE;
    killall Dock




