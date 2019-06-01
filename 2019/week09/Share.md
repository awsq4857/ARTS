# fastlane 入门使用

> fastlane 是 iOS (还有 Android ) 布署和发布的一套工具。它处理了所有重复的工作，例如生成截图，处理签名和发布应用。

## 安装

* fastlane 安装。`sudo gem install fastlane -NV`
* 项目初始化。`fastlane  init`

## 基础介绍

> 初始化后在./fastlane文件件即可找到。

* Appfile

存放着 AppleID 或者 BundleID 等一些fastlane需要用到的信息。基本上我们不需要改动这个文件的内容。

* FastFile
	
    * before_all 用于执行任务之前的操作，比如使用cocopods更新pod库
	* after_all 用于执行任务之后的操作，比如发送邮件，通知之前的
	* error 用于发生错误的操作
	* lane 定义用户的主要任务流程。例如打包ipa，执行测试等等

```
platform :ios do
  before_all do

  end

  desc "Runs all the tests"
  lane :test do
    scan
  end

  # You can define as many lanes as you want

  after_all do |lane|

  end

  error do |lane, exception|
    # slack(
    #   message: "Error message"
    # )
  end
end
```

* lane

	* build_app 生成 ipa 文件
	* upload_to_testflight 把 ipa 文件上传到 TestFilght

```
desc "Push a new beta build to TestFlight"   //该任务的描述
lane :beta do  //定义名字为 beta 的任务
  build_app(workspace: "expample.xcworkspace", scheme: "example") //构建App，又叫gym
  upload_to_testflight //上传到testfilght，
end
```

## 实践

```
desc "发布到Fir"
lane :pulish_to_fir do
  # 运行 pod install 
  cocoapods 
  # 构建和打包ipa
  gym(
    clean: true,
    output_directory: './firim',
    scheme: 'xxxx',
    configuration: 'Test',
    export_options: {
      method: 'development',
      provisioningProfiles: {
          "xxx.xxx.xxx": "match Development xxx.xxx.xxx"
      },
    }
  )
  # 上传ipa到fir.im服务器，在fir.im获取firim_api_token
  firim(firim_api_token: "fir_token")
end
```

* cocoapods。在项目里执行 pod install。
* gym。又名build_app，是fastlane的里面一部分，它可以方便生成和签名ipa，能为开发者省下不少功夫。
* firim。firim 是一个插件，执行 fastlane add_plugin firim 即可把插件装好。

## 资料

1. [fastlane](https://github.com/fastlane/fastlane)
2. [fastlane docs](https://docs.fastlane.tools/)
3. [fastlane Example](https://github.com/fastlane/examples)
4. [fastlane ci](https://github.com/fastlane/ci)


