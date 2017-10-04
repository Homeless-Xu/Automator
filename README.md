# Automator

    自动化 Mac 常用操作!    带✔︎的都是亲测可用的!








🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸 MP4 视频无损合并 2017-10-5-00 ✔︎🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸

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









🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸🔸

