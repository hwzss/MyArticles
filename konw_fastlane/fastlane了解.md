
![fastlane.图](https://raw.githubusercontent.com/hwzss/MyArticles/master/konw_fastlane/fastlane.png)

>在日常 App 的开发中，经常需要为自己的 App 的打包，如果你不是很在行脚本语言的话，你可能一般都是直接使用 Xcode 进行打包， 而这样一个过程通常需要耗费相当长的一段时间，这段时间并不是有意义的，你完全可以将它们话在更有意义的地方， 但这点时间目前却是必须浪费的，因为你可能并没找到替代它的工具，而 Fastlane 就是这样一个工具，使用它，你可以一个命令将整个过程放在后台进行，它将自动帮你打包然后上传，甚至提交 AppStore。

##前言

做为一位 iOS 开发 Cocopods 再熟悉不过了，他为我们的工作提供了很多的方便之处，Fastlane 和 Cocopods 一样皆指在为开发者提供便利的项目管理功能，节省开发者的时间，让开发者腾出更多的于自己的业务代码, 它们两都是基于 Ruby 写的工具，所以在使用方面能看到它们的语法很是相近，或者说根本就是是 Ruby 的语法。下面我们将讲解下 Fastlane 的作用。

##安装

和 Cocoapods 一样，安装 Fastlane 也很简单，你可以使用 RubyGem 的 安装方式

``` shell
sudo gem install fastlane -NV
```

也可以使用 Homebrew 进行安装:

``` shell
brew cask install fastlane
```

如何确认自己安装成功了 ？ 可以在 shell 中使用 which 命令，如下：

``` shell
which fastlane
```
终端将为你输出 Fastlane 所在路径，如果没有输出则表示并未成功安装上。

##使用

fastlane 使用也很简单，基本上和 Cocoapods 一样，所以 cd 到当前项目路径，然后执行下面命令：

``` shell
fastlane init
```
是不是和 Cocoapods 差不多，Fastlane 也会为你生成和 Podfile 类似的配置文件 Fastfile 让你进行配置。 在新版的 Fastlane 中，改进了很多东西，比如你第一次在项目路径进行 init 的时候，它会询问你需要什么样的功能，如下：

![初次使用截图.png](https://raw.githubusercontent.com/hwzss/MyArticles/master/konw_fastlane/shell%E6%88%AA%E5%9B%BE.png)

可以看到很方便了，如果你选择 2，Fastlane 将帮你配置好 Fastfile 文件，然后你就可以通过简单的命名进行 ipa 包上传了，文件内容如下：

![文件内容截图](https://raw.githubusercontent.com/hwzss/MyArticles/master/konw_fastlane/%E6%96%B9%E6%B3%95%E6%88%AA%E5%9B%BE.jpg)

代码里出现了 lane, 你可以想象成方法，而 beta 则是方法名，方法里面的 build_app 和 upload_to_testflight 是其他的一些方法，如它们的命名一样，它们会帮你构建 ipa 文件，然后上传到 testfilght 上，对， 就只需要这几行代码就可以实现。

init 完成后，我们可以在 shell 里输出如下命令：

``` shell
fastlane beta
```

shell 将自动开始对项目进行构建，然后打包，上传。其中的 beta 就是 Fastfile 里的 beta 方法。

上面这些现成的方法在 Fastlane 里称为 Action, 关于 Action 的文档，你可以在这里看到（[文档链接](https://docs.fastlane.tools/actions/)） 。

##进阶

Fastalne 不只单单帮你打包上传 ipa 的功能，它是一堆功能的合集，平台上已经拥有很多现成的功能方法合集，比如跑项目测试，截屏等等等等..

Fastalne 的配置文件 Fastfile 相对于 Podfile 更像一个代码文件，你可以在启动添加任意 action 来组织你想要的任务，比如添加 Git 操作， 添加上传到 蒲公英等 三方平台，这些 Action 有些已经被开发者实现了，有些可能需要你自己实现，因为 Fastlane 的 Action 支持开发者自己定义，具体可见[自定义 action 文档](). 当然 Fastlane 中也支持 Ruby 代码，所以你完全可以跳过自定义 Action 方式，直接手敲代码来实现你想要的功能。

基于公司项目打包需要，我实现了下面编写了下面一个 Fastfile 文件，自定义自己公司项目或者的配置文件，相比于 Fastlane 自带的行为，我的文件新添加如下功能：

1. 检查项目 version 的合理性，比如必须是 A.B.C的格式的 version, 以及新的构建版本号（ build ）应该为整数；
2. 再打包时可以在命令中指定 version 版本号或者 build 版本号，如果没有指定，每次构建默认在 build 上加一；
3. 指定 ipa 文件输出路径，方便 ipa 文件查找管理，目录排版效果如下:
   
   ![目录效果图](https://raw.githubusercontent.com/hwzss/MyArticles/master/konw_fastlane/%E7%9B%AE%E5%BD%95%E6%88%AA%E5%9B%BE.jpg)
   
4. 已集成 Testfilght 和 蒲公英 的支持，新项目使用只需要初始化 Fastlane 环境，然后复制配置内容既可以实现一键打包 Testfilght 或 蒲公英。

项目地址（ http://121.41.108.203/MC-iOS/MC_FalstLaneFile.git ）  目前只支持这些功能持续增加中。。。 欢迎各路大神提需求，提bug, 提代码。


##效果

相比于使用 Xcode 打包，Fastlane 输入命令后不再需要人为操作了，所以基本上不费多少时间， 假设使用 Xcode 打包每次需要 5 分钟（事实上多数人需要跟多的时间），那 Fastlane 基本每次能给你省去这4分钟，一次4 分钟，多来几次省出来的时间是很可观的。

##总结

其实说简单点，Fastlane 和 Cocoapods 本质上就是一个通过 Ruby 来执行 shell 进而操作一堆现有的三方工具的工具。 如果你会一种脚本语言，完全可以很简单的 整合 XcodeBuild, Vcgtool等工具实现 Fastlane 的功能。

Fastlane 相比 Cocopods 更自由些, 所以拥有等多功能，但也随值带来更多额外的 Gem 配置， 导致有时项目配置起来可能不是那么顺利，有时又很简单。

最后 Fastlane 不仅仅支持 iOS 同时也支持安卓。

##相关资料

1. [Fastlane官网](https://fastlane.tools/)
2. [Fastlane官方文档](https://docs.fastlane.tools/)
3. [Fastlane-移动开发自动化之道
](https://mp.weixin.qq.com/s?__biz=MzUxMzcxMzE5Ng==&mid=2247488389&amp;idx=1&amp;sn=a218682281a3b3f205eeb09fb93aeadd&source=41#wechat_redirect)


