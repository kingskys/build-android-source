# build-android-source
Mac OS 编译 android 8.1.0 源码

系统 macOS Mojave 版本 10.14.6

<h1>下载Android源码</h1>
<ol>
  <li>设置.bash_profile</li>
  <pre>
    <code>
    # 其他内容全注释掉
    ulimit -S -n 1024
    export PATH=~/bin:$PATH
    </code>
  </pre>
  <li>安装repo</li>
  <pre>
    <code>
    $ mkdir ~/bin       //创建bin文件夹
    $ cd ~/bin
    $ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo 
    $ chmod a+x ~/bin/repo    //修改repo的权限
    </code>
  </pre>
  <li>创建映像</li>
    <ol>
      <li>打开磁盘工具</li>
      <li>文件->新建映像->空白空白映像</li>
      <li>存储为ASOP.dmg 名称 ASOP 大小 200GB 格式 Mac OS 扩展（区分大小写，日志式）</li>
    </ol>  
    <code></code>
  <li>初始化repo</li>
  <pre>
    <code>双击打开 AOSP.dmg</code>
    <code>
    $ cd /Volumes/ASOP
    $ mkdir WORKING_DIRECTORY
    $ cd WORKING_DIRECTORY
    </code>
  </pre>
  <li>获取最新的源码库 （最好是拉取主分支，方便切换分支）</li>
  <pre>
    <code>
    $ repo init -u https://android.googlesource.com/platform/manifest  //拉取主分支，也就是最新的代码
    $ repo sync
    </code>
  </pre>
  <li>切换到指定分支</li>
  <pre>
    <code>
    $ repo init -b  android-8.1.0_r1
    $ repo sync
    $ repo start android-8.1.0_r1 --all
    </code>
  </pre>
</ol>
<h2>repo命令</h2>
<ol>
  <li>查看版本信息</li>
  <code>repo info</code>
  <li>查看当前分支</li>
  <code>repo branches</code>
  <li>查看可切换分支</li>
  <pre>
    <code>$ cd .repo/manifests</code>
    <code>$ git branch -a | cut -d / -f 3</code>
  </pre>
  <li>切换分支</li>
  <pre>
    <code>
    $ repo init -b  android-8.1.0_r1
    $ repo sync
    $ repo start android-8.1.0_r1 --all
    </code>
  </pre>
  <li>查看android当前版本号</li>
  <code>grep PLATFORM_VERSION build/core/version_defaults.mk</code>
</ol>
<h1>编译工具</h1>
<table>
  <tr>
    <th>工具</th>
    <th width="100">链接</th>
    <th>安装</th>
    <th>安装目录</th>
    <th>备注</th>
  </tr>
  <tr>
    <td>Command Line Tools (macOS 10.12) for Xcode 8.3</td>
    <td>https://developer.apple.com/download/more/</td>
    <td>双击</td>
    <td>/Library/Developer/CommandLineTools/SDKs</td>
    <td>使用 Xcode 8.3 的</td>
  </tr>
  <tr>
    <td>xz utils 压缩工具</td>
    <td>https://tukaani.org/xz/xz-5.2.4.tar.gz</td>
    <td><pre>
$ cd xz-5.2.4
$ ./configure
$ make
$ make install
    </pre>
    </td>
    <td></td>
    <td>查看版本 $ xz -V</td>
  </tr>
  <tr>
    <td>cmake</td>
    <td></td>
    <td></td>
    <td></td>
    <td>版本GNU Make 3.81</td>
  </tr>
  <tr>
    <td>git</td>
    <td></td>
    <td></td>
    <td></td>
    <td>版本git version 2.11.0 (Apple Git-81)</td>
  </tr>
</table>
<h1>编译前</h1>
<ol>
  <li>进入编译目录</li>
  <li>清空编译输出文件</li>
  <code>$ make clobber</code>
  <li>真机驱动（官方）</li>
  <pre>
  版本号对应设备 https://source.android.com/setup/build-numbers#source-code-tags-and-builds
  下载专有二进制文件 https://developers.google.com/android/drivers
  </pre>
  <li>驱动使用方法</li>
  <pre>
    将下载的sh文件（2个）问放入编译目录，然后运行
    一直按回车 最后（8条）同意 输入 I ACCEPT
    文件生成在vendor目录
  </pre>
</ol>  
<h1>编译</h1>
<ol>
  <li>进入编译目录</li>
  <code>$ cd WORKING_DIRECTORY</code>
  <li>开始编译</li>
  <pre>
  $ source build/envsetup.sh
  $ lunch 
  Which would you like? [aosp_arm-eng] 6 // 6. aosp_x86_64-eng
  $ make -j4
  </pre>
	<li>编译成功</li>
	<pre>
	#### build completed successfully (04:08 (mm:ss)) ####
	</pre>
</ol>
<h1>编译时可能会遇到的问题</h1>
<ol>
  <li>bison</li>
  <code>编译bison</code>
  <pre>
    $ cd /Volumes/AOSP/external/bison
    $ git cherry-pick c0c852bd6fe462b148475476d9124fd740eba160	
    $ mm
  </pre>
  <code>（如果mm不能执行）</code>
  <pre>
    $ source build/envsetup.sh
	  $ lunch 
    Which would you like? [aosp_arm-eng] 6 // 6. aosp_x86_64-eng
    $ mm
  </pre>
  <code>复制编译好的bison文件</code>
  <code>cp /Volumes/AOSP/out/host/darwin-x86/bin/bison /Volumes/AOSP/prebuilts/misc/darwin-x86/bison/</code>
</ol>
<h1>链接</h1>
<ul>
  <li>Mac OS 工具下载地址 https://developer.apple.com/download/more/</li>
  <li>安卓源码所有分支 https://android.googlesource.com/platform/manifest</li>
  <li>官方编译方法</li>
  <ul>
    <li>下载源代码 https://source.android.com/source/downloading.html</li>
    <li>搭建编译环境 https://source.android.com/source/initializing.html</li>
    <li>编译准备工作 https://source.android.com/source/building.html</li>
  </ul>
</ul>
<h1>刷入手机（因为我没手机，所以这步弄不了了）</h1>
<pre>
编译完后，执行：
$ export ANDROID_PRODUCT_OUT=export ANDROID_PRODUCT_OUT=/Volumes/ASOP/WORKING_DIRECTORY/out/target/product/generic_x86_64
$ fastboot flashall -w
$ fastboot reboot
</pre>
<h1>用Android Studio 查看源码</h1>
<h2>编译idegen模块，生成 ./out/host/darwin-x86/framework/idegen.jar </h2>
<pre>
配置文件添加
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_111/Contents/Home
export ANDROID_JAVA_HOME=$JAVA_HOME
</pre>
<pre>
重启打开一个终端
$ cd /Volumes/ASOP/WORKING_DIRECTORY/
$ mmm development/tools/idegen/
</pre>
<code>$ mmm development/tools/idegen/</code>
<code>生成工程文件，生成在了 /Volumes/ASOP/WORKING_DIRECTORY 目录下</code>
<code>$ development/tools/idegen/idegen.sh</code>
<code>用 Android Studio 打开 idegen.ipr</code>
