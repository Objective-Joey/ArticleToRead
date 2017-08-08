### 一、介绍
1. 什么是Appium?

    Appium是一个开源、跨平台的测试框架，可以用来测试原生及混合的移动端应用,支持iOS、Android及FirefoxOS平台。Appium使用WebDriver的json wire协议，来驱动Apple系统的UIAutomation/XCUITest库、Android系统的UIAutomator/Selendroid框架。
2. 使用Appium进行自动化测试的优点
    
    - Appium在不同平台中使用了标准的自动化APIs，所以在跨平台时，不需要重新编译或者修改自己的应用

    - Appium支持多种主流语言编写测试用例，如Ruby,Python,Java,JavaScript,PHP,C#等
    
3. MacOS系统搭建Appium环境有三种方法
    - 使用npm命令安装，目前最高版本为1.6.5
    
    - 直接下载appium.dmg安装运行即可，目前最高版本为1.5.3, 这种方式比上面方式简单，安装后大部分配置已经配置好
        
    - 下载桌面desktop版本,目前最新版本为1.1.0-beta.4版本，这是上方方式的加强版本，包含了比较完善的Client测试配置项
4. 工作方式
    
    Appium分为服务端，客户端和测试终端（手机），客户端（Client）通过不同语言编写的测试脚本发送Http REST请求到服务端，服务端处理请求后把命令发送到测试终端上面，终端解析命令后进行相应的操作
    

### 二、服务端配置
   
    
##### 0. *爬墙，这会节省很多时间；另外使用npm过程中如果下载不了请切换下载源*

```
npm config set registry https://registry.npm.taobao.org
#或者
npm config set registry http://r.cnpmjs.org/
```


##### 1. *安装Homebrew管理器*
检查是否安装：

```
brew -v
```

安装命令：

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

注：brew每次使用都会去检查更新，很耗费时间，可以在.bash_profile中配置，配置后请执行脚本使其生效

```
export HOMEBREW_NO_AUTO_UPDATE=true
```
使配置生效:

```
source ~/.bash_profile
```


    
    
##### 2. *安装Java环境*
MacOS默认已经安装了Java环境，安装位置/Library/Java/，如果没有安装请到[下载安装](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html),  检查是否安装：

```
java -version
```

在.bash_profile中配置环境变量（根据自己的版本和安装位置配置）： 
 
```
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_77.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
export CLASS_PATH=$JAVA_HOME/lib
```
使配置生效:

```
source ~/.bash_profile
```
    
#####  3.*配置Android环境*
[下载最新版本](https://developer.android.google.cn/studio/archive.html#android-studio-2-3-2)AndroidStudio，下载后按照默认配置安装（全部下一步）,安装完成后配置环境变量：

```
export ANDROID_HOME=~/Library/Android/sdk
export PATH=${PATH}:~/Library/Android/sdk/platform-tools/.
export PATH=${PATH}:~/Library/Android/sdk/tools/.
```

使配置生效:

```
source ~/.bash_profile
```

#####  4.*配置iOS环境*
到AppStore下载XCode(如果需要使用app-inspector2工具XCode需要8.3.2及以上版本)
    
##### 5.*安装git工具*
建议安装最新版本，因为下面如果使用app-inspector工具获取元素位置时对git有版本要求

检查是否安装：

```
git --version
```

安装:

```
brew install git
```

    
##### 6. *安装node环境*
建议安装最新版本，因为下面如果使用app-inspector工具获取元素位置时对node有版本要求

检查是否安装：

```
node -v
```

安装:

```
brew install node
```


##### 7. *安装wd工具*

```
 npm install -g wd
```


##### 8. *安装appium服务端*
可配合app版本使用，app版本已集成环境检查工具appium-doctor和元素获取工具app-inspector
   
```
 npm install -g appium
```

    
#####  9.*安装appium环境检查工具*
这一步不是必须的，只是为了检查环境是否配置好，建议安装，appium应用版本已集成该工具
    
```
npm install -g appium-doctor
```
使用方法:

```
appium-doctor
```




##### 10.*安装app-inspector元素获取工具*
这一步不是必须的，只是为了方便写Client测试脚本时定位元素，appium应用版本已集成该工具

```
npm install -g macaca-cli
npm install -g app-inspector
```

下面命令可用来检测macaca配置环境是否成功（绿色代表该项成功，红色失败）

```
macaca doctor 
```

下面是定位元素工具的启动命令，服务端必须开启，客户端必须配置好

```
app-inspector -u YOUR-DEVICE-ID  
```


### 三、客户端配置
##### 1.开发工具安装
这里使用Java语言编写客户端,下载[IntelliJ IDEA](https://download.jetbrains.8686c.com/idea/ideaIC-2017.1.5.dmg)工具进行开发, 下载后按照默认配置安装（全部下一步）

##### 2.下载开发所需要依赖包
[appium Java Client所需要依赖包](https://search.maven.org/remotecontent?filepath=io/appium/java-client/5.0.0-BETA9/java-client-5.0.0-BETA9.jar)

[seleium依赖包](http://selenium-release.storage.googleapis.com/3.4/selenium-java-3.4.0.zip)

##### 3.新建测试项目

新建一个空的Java项目，新建libs目录，把上面下载的所有.jar文件放到libs目录下，使用jar包（File -> ProjectStructure -> 选中新建的项目 -> 选中Dependencies -> 点击"+"号 -> 点击 1.JARs or directories, 选择刚才新建的libs目录中所有的jar文件）

##### 4.编写Client测试脚本

    见 六、七参考脚本

##### 5.流程描述：
- shell中启动服务端，shell中输入: appium -a 127.0.0.1 -p 4723 --session-override  或者在app版本点击launch开启服务
- 连接上手机
- 启动客户端测试脚本，在对应test方法中右键点击 “Run test()”启动单元测试
- app-inspector -u 40C7E288-7432-4CA7-84CE-D70F1476C4F0 --verbose启动元素获取工具


### 四、 iOS真机测试注意事项
	注：测试时确保手机上已安装app或者测试脚本中指定app安装路径

	iOS项目真机需要对项目签名，因此需要配置项目。如果有appleId，请在Xcode->Preferences->
	Accounts添加自己的开发账号
	
	打开WebDriverAgent.xcodeproj项目，修改bundleId
	修改后真机跑一下，失败请参照错误提示进行修改。

-  WebDriverAgent配置[参照](https://testerhome.com/topics/7220)

```
cd usr/local/lib/node_modules/appium/node_modules/appium-xcuitest-driver/WebDriverAgent 
```

- XCTestWD配置[参照](https://macacajs.github.io/zh/environment-setup)

```
cd /usr/local/lib/node_modules/app-inspector/node_modules/xctestwd/XCTestWD
```
打开XCTestWD.xcodeproj项目，修改bundleId
修改后真机跑一下，失败请参照错误提示进行修改。
此步骤在真机跑app-inspector时会出现失败，主要在此，如果失败请检查是否正确更改bundleId



- 启动Appium服务端

	获取真机udid（手机已连接上电脑）
	
	```
	#获取设备udid
	instruments -s devices
	
	#此处请替换为目标真机udid
	appium -u udid --app com.pingan.zqzzxx
	```

- 安装WebDriverAgentRunner

	```
	#进入WebDriverAgent目录
	cd usr/local/lib/node_modules/appium/node_modules/appium-xcuitest-driver/WebDriverAgent 
	#请将UDID替换为目标真机的udid
	xcodebuild -project WebDriverAgent.xcodeproj -scheme WebDriverAgentRunner -destination "UDID" test
	```

- 启动客户端脚本。
    
    打开IntelliJ IDEA开发工具，右键执行测试脚本

- 启动app-inspector

    请替换为目标真机的udid。
    
	```
	app-inspector -u udid --verbose
	```



### 五、H5测试配置
这一步不是必须的，如果不需要测试H5,可以不必安装，以Chrome为例:
1. 安装chromedriver

```
 npm install -g chromedriver
```

2. 脚本配置

```
package com.pingan;

import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.Dimension;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.remote.DesiredCapabilities;

import java.util.HashMap;
import java.util.Map;

/**
 * Created by cxh on 17/7/28.
 */
public class TestHtml {

    private WebDriver driver;

    @Before
    public void setUp(){
        System.setProperty("webdriver.chrome.driver", "/usr/local/lib/node_modules/chromedriver/lib/chromedriver/chromedriver");
        DesiredCapabilities capabilities = new DesiredCapabilities();

        Map mobileOptions = new HashMap();
        mobileOptions.put("deviceName", "iPhone 6");


        Map chromeOptions = new HashMap();
        chromeOptions.put("mobileEmulation", mobileOptions);
        capabilities.setCapability(ChromeOptions.CAPABILITY, chromeOptions);

        driver = new ChromeDriver(capabilities);
        driver.get("https://www.baidu.com");
    }

    @Test
    public void test(){
        driver.manage().window().setSize(new Dimension(500, 900));
        driver.findElement(By.xpath("//div")).click();
    }
}

```





### 六、平安证券app测试脚本


测试脚本:

```
package com.pingan;

import io.appium.java_client.android.AndroidDriver;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.remote.CapabilityType;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.net.URL;
import java.util.Date;

/**
 * Created by cxh on 17/7/26.
 */
public class ZQTest {
    private AndroidDriver driver;
    private String newsPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.RelativeLayout[2]/android.widget.LinearLayout[1]/android.widget.ScrollView[1]/android.widget.LinearLayout[1]/android.widget.LinearLayout[1]/android.widget.LinearLayout[1]/android.widget.GridView[1]/android.widget.LinearLayout[4]";
    private String newsBackPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.RelativeLayout[1]/android.widget.LinearLayout[1]";
    private String newsTopPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[2]/android.widget.LinearLayout[1]/android.support.v4.view.ViewPager[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.Button[1]";
    private String searchPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.RelativeLayout[2]/android.widget.LinearLayout[1]/android.widget.LinearLayout[1]/android.widget.LinearLayout[1]/android.widget.RelativeLayout[2]";
    private String inputPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.LinearLayout[1]/android.widget.LinearLayout[1]/android.widget.EditText[1]";

    private String inputCancelPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.LinearLayout[1]/android.widget.TextView[1]";

    @Before
    public void setUp() throws Exception {
        //设置自动化相关参数
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability(CapabilityType.BROWSER_NAME, "");
        capabilities.setCapability("platformName", "Android");
        capabilities.setCapability("deviceName", "Android Emulator");//安卓该字段不起作用
        capabilities.setCapability("noReset", true);   //不需要再次安装

        //设置安卓系统版本
        capabilities.setCapability("platformVersion", "6.0.1");
        //设置可支持编码，可直接输入中文
        capabilities.setCapability("unicodeKeyboard", true);
//        capabilities.setCapability("resetKeyboard", true);
        //设置app的主包名和主类名
        capabilities.setCapability("appPackage", "com.hundsun.winner.pazq");
        capabilities.setCapability("appActivity", "com.hundsun.winner.pazq.ui.init.activity.SplashActivity");
        capabilities.setCapability("sessionOverride", true);

        capabilities.setCapability("newCommandTimeout", 600);
        //初始化
        driver = new AndroidDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);
    }

    @Test
    public void test() {
        System.out.println(new Date().toString() + "----------" + "进入平安证券：成功");
        WebElement news = getElement(By.xpath(newsPath), 20);
        news.click();
        System.out.println(new Date().toString() + "----------" + "进入首页资讯：成功");
//        sleep(5000);

        System.out.println(new Date().toString() + "----------" + "进入 资讯/要闻：成功");
        for (int i = 0; i < 2; i++) {
            String[] newsTitles = {"要闻", "直播", "推荐"};
            System.out.println(new Date().toString() + "----------" + "滑动选中 资讯/" + newsTitles[i + 1] + "：成功");
            sleep(2000);
            swipRightToLeft();
        }

        for (int i = 0; i < 10; i++) {
            sleep(3000);
            swipBottomToTop();
            System.out.println(new Date().toString() + "----------" + "第" + (i + 1)  + "次" + "滑动加载更多 资讯/推荐：成功");
        }

        getElement(By.xpath(newsTopPath), 2).click();//返回列表顶部
        System.out.println(new Date().toString() + "----------" + "返回首页 资讯/推荐 顶部：成功");

        getElement(By.xpath(newsBackPath), 2).click();//从资讯返回主页
        System.out.println(new Date().toString() + "----------" + "退出资讯返回首页：成功");


        getElement(By.xpath(searchPath), 2).click();//首页搜索
        System.out.println(new Date().toString() + "----------" + "点击首页顶部搜索：成功");

        getElement(By.xpath(inputPath), 2).click();//点击输入
        System.out.println(new Date().toString() + "----------" + "点击搜索获取焦点：成功");

        getElement(By.xpath(inputPath), 2).sendKeys("pingan");//输入内容
        System.out.println(new Date().toString() + "----------" + "输入搜索内容：成功");

        getElement(By.xpath(inputCancelPath), 2).click();//取消搜索
        System.out.println(new Date().toString() + "----------" + "取消搜索返回首页：成功");

        System.out.println(new Date().toString() + "----------" + "测试成功！");
    }


    public void sleep(int time) {
        try {
            Thread.sleep(time);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }


    /**
     * 获取元素
     * @param by
     * @param maxWaitSeconds
     * @return
     */
    public WebElement getElement(By by, int maxWaitSeconds) {
        WebDriverWait wait = new WebDriverWait(driver, maxWaitSeconds);
        wait.until(ExpectedConditions.visibilityOfElementLocated(by));

        WebElement element = driver.findElement(by);
        return element;
    }


    /**
     * 从右向左滑动
     */
    public void swipRightToLeft() {
        int width = driver.manage().window().getSize().getWidth();
        int height = driver.manage().window().getSize().getHeight();
        driver.swipe(width - 20, height / 2, 20, height / 2, 1000);
    }


    /**
     * 从底向上滑动（加载更多手势）
     */
    public void swipBottomToTop() {
        int width = driver.manage().window().getSize().getWidth();
        int height = driver.manage().window().getSize().getHeight();
        driver.swipe(width / 2, height - 20, width / 2, 60, 1000);
    }

    @After
    public void tearDown() throws Exception {
        driver.quit();
    }
}
```

测试结果：

```
Thu Jul 27 20:13:56 CST 2017----------进入平安证券：成功
Thu Jul 27 20:14:38 CST 2017----------进入首页资讯：成功
Thu Jul 27 20:14:38 CST 2017----------进入 资讯/要闻：成功
Thu Jul 27 20:14:38 CST 2017----------滑动选中 资讯/直播：成功
Thu Jul 27 20:14:40 CST 2017----------滑动选中 资讯/推荐：成功
Thu Jul 27 20:14:47 CST 2017----------第1次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:14:50 CST 2017----------第2次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:14:54 CST 2017----------第3次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:14:57 CST 2017----------第4次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:15:01 CST 2017----------第5次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:15:05 CST 2017----------第6次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:15:08 CST 2017----------第7次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:15:12 CST 2017----------第8次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:15:16 CST 2017----------第9次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:15:19 CST 2017----------第10次滑动加载更多 资讯/推荐：成功
Thu Jul 27 20:15:21 CST 2017----------返回首页 资讯/推荐 顶部：成功
Thu Jul 27 20:15:23 CST 2017----------退出资讯返回首页：成功
Thu Jul 27 20:16:04 CST 2017----------点击首页顶部搜索：成功
Thu Jul 27 20:16:10 CST 2017----------点击搜索获取焦点：成功
Thu Jul 27 20:16:16 CST 2017----------输入搜索内容：成功
Thu Jul 27 20:16:19 CST 2017----------取消搜索返回首页：成功
Thu Jul 27 20:16:19 CST 2017----------测试成功！
```





### 七、指掌行销测试脚本

测试脚本:

```
package com.pingan;

import io.appium.java_client.android.AndroidDriver;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.ie.InternetExplorerDriver;
import org.openqa.selenium.remote.CapabilityType;
import org.openqa.selenium.remote.DesiredCapabilities;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.net.URL;
import java.util.Date;

/**
 * Created by cxh on 17/7/26.
 */

public class FPMTest {
    //引导页
    String viewpagerDotPath = "//android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.RelativeLayout[1]/android.widget.RelativeLayout[1]/android.widget.RelativeLayout[1]/android.widget.LinearLayout[1]";

    //登录页用户名
    String namePath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.widget.EditText[1]";

    //登录页密码
    String pwdPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[3]/android.widget.EditText[1]";

    //登录页登录按钮
    String loginPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[3]/android.widget.TextView[1]";

    //首页-我的按钮
    String navMyPath = "//android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.view.ViewGroup[5]";

    //我的-设置按钮
    String setPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.widget.ScrollView[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[11]";

    //设置-退出登录按钮
    String logoutPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.widget.ScrollView[1]/android.view.ViewGroup[1]/android.view.ViewGroup[7]";

    //设置-确认退出登录按钮
    String logoutSurePath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[3]";


    //首页-资讯
    String infoPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.widget.ScrollView[1]/android.view.ViewGroup[1]/android.view.ViewGroup[5]/android.widget.ImageView[1]";
    //首页-资讯-返回
    String infoBack = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.widget.TextView[1]";
    //首页-资讯-活期
    String currentPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.webkit.WebView[1]/android.webkit.WebView[1]/android.view.View[2]/android.view.View[1]/android.view.View[2]/android.view.View[1]/android.widget.ListView[1]/android.view.View[1]";


    //工作台
    String workPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.view.ViewGroup[1]/android.widget.TextView[2]";

    //客户
    String customerPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.view.ViewGroup[2]/android.widget.TextView[2]";
    String customerFilterPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.view.ViewGroup[1]/android.widget.TextView[1]";
    String dealFilterPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[4]/android.widget.ScrollView[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.widget.TextView[1]";
    String noDealPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[4]/android.view.ViewGroup[1]/android.widget.ScrollView[1]/android.view.ViewGroup[1]/android.view.ViewGroup[3]/android.widget.TextView[1]";
    String confirmFilter = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[4]/android.view.ViewGroup[2]/android.view.ViewGroup[2]/android.widget.TextView[1]";

    //日志
    String logPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.widget.ScrollView[1]/android.view.ViewGroup[1]/android.view.ViewGroup[4]/android.widget.ImageView[1]";
    String logTypePath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.widget.TextView[2]";
    String branchLogPath = "//android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.widget.FrameLayout[1]/android.widget.LinearLayout[1]/android.widget.FrameLayout[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.widget.ScrollView[1]/android.view.ViewGroup[1]/android.view.ViewGroup[2]/android.widget.TextView[1]";



    private AndroidDriver driver;
    private WebElement name;
    private WebElement pwd;
    private WebElement login;

    @Before
    public void setUp() throws Exception {
        //设置自动化相关参数
        DesiredCapabilities capabilities = new DesiredCapabilities();
        capabilities.setCapability(CapabilityType.BROWSER_NAME, "");
        capabilities.setCapability("platformName", "Android");
        capabilities.setCapability("deviceName", "Android Emulator");//安卓该字段不起作用
        capabilities.setCapability("noReset", true);   //不需要再次安装

        //设置安卓系统版本
        capabilities.setCapability("platformVersion", "6.0.1");

        //设置可支持编码，可直接输入中文
        capabilities.setCapability("unicodeKeyboard", true);
        capabilities.setCapability("resetKeyboard", true);
        //设置网络选项，可选项
        capabilities.setCapability(InternetExplorerDriver.IE_ENSURE_CLEAN_SESSION, true);
        //设置app的主包名和主类名
        capabilities.setCapability("appPackage", "com.pingan.fpm");
        capabilities.setCapability("appActivity", ".MainActivity");
        capabilities.setCapability("sessionOverride", true);

        capabilities.setCapability("newCommandTimeout", 600);

        //初始化
        driver = new AndroidDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);
    }


    /**
     * 检测元素是否可见
     * @param by
     * @return
     */
    public boolean isElementVisible(By by) {
        try {
            WebDriverWait wait = new WebDriverWait(driver, 1);
            wait.until(ExpectedConditions.visibilityOfElementLocated(by));
            return true;
        } catch (Exception e) {

        }
        return false;
    }


    /**
     * 左滑
     */
    public void swipRightToLeft() {
        int width = driver.manage().window().getSize().getWidth();
        int height = driver.manage().window().getSize().getHeight();
        driver.swipe(width - 20, height / 2, 20, height / 2, 1000);
    }


    /**
     * 左滑
     */
    public void swipLeftToRight() {
        int width = driver.manage().window().getSize().getWidth();
        int height = driver.manage().window().getSize().getHeight();
        driver.swipe(10, height / 2 , width - 20, height / 2 , 1000);
    }

    /**
     * 上滑
     */
    public void swipBottomToTop() {
        int width = driver.manage().window().getSize().getWidth();
        int height = driver.manage().window().getSize().getHeight();
        driver.swipe(width / 2, height - 20, width / 2, 60, 1000);
    }

    /**
     * 下滑
     */
    public void swipTopToBottom() {
        int width = driver.manage().window().getSize().getWidth();
        int height = driver.manage().window().getSize().getHeight();
        driver.swipe(width / 2, 600, width / 2, height - 50 , 1000);
    }



    /**
     * 线程休眠
     * @param time
     */
    public void sleep(int time) {
        try {
            Thread.sleep(time);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取元素
     * @param by
     * @param maxWaitSeconds
     * @return
     */
    public WebElement getElement(By by, int maxWaitSeconds) {
        WebElement element = null;
        try {
            WebDriverWait wait = new WebDriverWait(driver, maxWaitSeconds);
            wait.until(ExpectedConditions.visibilityOfElementLocated(by));

            element = driver.findElement(by);
        } catch (Exception e) {

        }
        return element;
    }

    /**
     * 启动测试
     */
    @Test
    public void test() {
        //处理引导页
        handleGuidePage();

        boolean waitingSplash = true;
        while (waitingSplash) {
            if (isElementVisible(By.xpath(navMyPath))) {//长登录，直接进入首页
                System.out.println(new Date() + "已登录，进入主页------------------");
                waitingSplash = false;
                handleLogined();
            } else if (isElementVisible(By.xpath(namePath))) {
                System.out.println(new Date() + "未登录，进入登录页------------------");
                waitingSplash = false;

                boolean isLogined = false;
                for(int i = 5; !isLogined && i < 7; i++) {
                    System.out.println(new Date() + "第" + (i-4) + "次登录------------------");
                    loginAction("zhaozheng215", "pa12345" + i);
                    WebElement element = getElement(By.xpath(navMyPath), 10);
                    if (element == null) {
                        System.out.println(new Date() + "登录失败------------------");
                    } else {
                        System.out.println(new Date() + "登录成功------------------");
                        handleLogined();
                    }
                }
            } else {
                sleep(1000);
            }
        }

    }

    private void handleLogined() {
        toLog();
        toInfo();
        toCustomer();
        logout();
    }

    /**
     * 跳转到日志
     */
    private void toLog() {
        sleep(5000);
        System.out.println(new Date().toString()  + "进入日志页面--------------");
        getElement(By.xpath(logPath), 20).click();

        System.out.println(new Date().toString()  + "点击日志类型--------------");
        getElement(By.xpath(logTypePath), 20).click();

        System.out.println(new Date().toString()  + "点击下属日志--------------");
        getElement(By.xpath(branchLogPath), 20).click();

        System.out.println(new Date().toString()  + "右滑手势退出日志页面--------------");
        sleep(6000);
        swipLeftToRight();
        sleep(5000);
    }

    /**
     * 跳转到客户
     */
    private void toCustomer() {
        System.out.println(new Date().toString()  + "进入客户页面--------------");
        getElement(By.xpath(customerPath), 10).click();

        sleep(10000);
        System.out.println(new Date().toString()  + "点击筛选--------------");
        getElement(By.xpath(customerFilterPath), 20).click();

        sleep(5000);
        System.out.println(new Date().toString()  + "点击交易筛选--------------");
        getElement(By.xpath(dealFilterPath), 20).click();

        System.out.println(new Date().toString()  + "点击最近一月无交易--------------");
        getElement(By.xpath(noDealPath), 20).click();

        System.out.println(new Date().toString()  + "确定筛选条件--------------");
        getElement(By.xpath(confirmFilter), 20).click();

        sleep(20000);

        System.out.println(new Date().toString()  + "点击工作台--------------");
        getElement(By.xpath(workPath), 20).click();
    }


    /**
     * 退出登录
     */
    private void logout(){
        sleep(20000);
        System.out.println(new Date().toString()  + "进入我的页面--------------");
        getElement(By.xpath(navMyPath), 10).click();

        System.out.println(new Date().toString()  + "进入我的设置页面--------------");
        getElement(By.xpath(setPath), 10).click();

        System.out.println(new Date().toString()  + "点击退出登录页面--------------");
        getElement(By.xpath(logoutPath), 10).click();

        System.out.println(new Date().toString()  + "点击确定退出按钮--------------");
        getElement(By.xpath(logoutSurePath), 10).click();
    }

    private void toInfo() {
        sleep(5000);
        WebElement info = getElement(By.xpath(infoPath), 30);
        System.out.println(new Date().toString()  + "进入资讯页面--------------");
        info.click();

        sleep(10000);

        System.out.println(new Date().toString()  + "进入资讯-活期页面--------------");
        try {
            getElement(By.xpath(currentPath), 30).click();

            for (int i = 0; i < 4; i++) {
                sleep(3000);
                System.out.println(new Date().toString()  + "第" + (i + 1)  + "次" + "滑动加载更多--------------");
                swipBottomToTop();
            }

        } catch (Exception e) {
            System.out.println(new Date().toString()  + "进入资讯-活期页面:失败--------------");
        }


        System.out.println(new Date().toString()  + "返回首页--------------");
        getElement(By.xpath(infoBack), 2).click();

    }

    /**
     * 处理引导页
     */
    private void handleGuidePage() {
        //初次安装有引导页，先滑动到最后并隐藏引导页
        try {
            WebDriverWait pagerWait = new WebDriverWait(driver, 5);
            pagerWait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath(viewpagerDotPath)));
            System.out.println(new Date() + "检测到引导页------------------");


            for (int i = 0; i < 10; i++) {//TODO可用while处理，不管引导页数量
                swipRightToLeft();
                System.out.println(new Date() + "从右向左滑动------------------");
                if (isElementVisible(By.id("com.pingan.fpm:id/btn_start"))) {
                    driver.findElementById("com.pingan.fpm:id/btn_start").click();
                    System.out.println(new Date() + "隐藏引导页，进入登录页------------------");
                    break;
                }
            }
        } catch (Exception e) {
            //没有引导页，直接等待登录页
            System.out.println(new Date() + "没有检测到引导页------------------");
        }
    }

    /**
     * 发起登录
     * @param userName
     * @param password
     */
    private void loginAction(String userName, String password) {
        //等待splash（初次还有引导页）页面过去后开始找到输入框
        name = getElement(By.xpath(namePath), 20);

        name.clear();
        name.clear();
        name.sendKeys(userName);

        pwd = getElement(By.xpath(pwdPath), 2);
        pwd.clear();
        pwd.clear();
        pwd.sendKeys(password);

        login = getElement(By.xpath(loginPath), 2);
        login.click();
        System.out.println(new Date() + "登录： 用户名：" + userName + "; 密码：" + password +";------------------");
    }

//
//    @After
//    public void tearDown() throws Exception {
//        driver.quit();
//    }
}

```
测试结果：

```
Thu Jul 27 20:52:30 CST 2017检测到引导页------------------
Thu Jul 27 20:52:31 CST 2017从右向左滑动------------------
Thu Jul 27 20:52:33 CST 2017从右向左滑动------------------
Thu Jul 27 20:52:34 CST 2017从右向左滑动------------------
Thu Jul 27 20:52:40 CST 2017隐藏引导页，进入登录页------------------
Thu Jul 27 20:52:42 CST 2017未登录，进入登录页------------------
Thu Jul 27 20:52:42 CST 2017第1次登录------------------
Thu Jul 27 20:53:20 CST 2017登录： 用户名：zhaozheng215; 密码：pa123455;------------------
Thu Jul 27 20:53:30 CST 2017登录失败------------------
Thu Jul 27 20:53:30 CST 2017第2次登录------------------
Thu Jul 27 20:54:16 CST 2017登录： 用户名：zhaozheng215; 密码：pa123456;------------------
Thu Jul 27 20:54:18 CST 2017登录成功------------------
Thu Jul 27 20:54:23 CST 2017进入日志页面--------------
Thu Jul 27 20:54:24 CST 2017点击日志类型--------------
Thu Jul 27 20:54:26 CST 2017点击下属日志--------------
Thu Jul 27 20:54:28 CST 2017右滑手势退出日志页面--------------
Thu Jul 27 20:54:46 CST 2017进入资讯页面--------------
Thu Jul 27 20:54:56 CST 2017进入资讯-活期页面--------------
Thu Jul 27 20:55:00 CST 2017第1次滑动加载更多--------------
Thu Jul 27 20:55:03 CST 2017第2次滑动加载更多--------------
Thu Jul 27 20:55:07 CST 2017第3次滑动加载更多--------------
Thu Jul 27 20:55:10 CST 2017第4次滑动加载更多--------------
Thu Jul 27 20:55:11 CST 2017返回首页--------------
Thu Jul 27 20:55:14 CST 2017进入客户页面--------------
Thu Jul 27 20:55:26 CST 2017点击筛选--------------
Thu Jul 27 20:55:32 CST 2017点击交易筛选--------------
Thu Jul 27 20:55:33 CST 2017点击最近一月无交易--------------
Thu Jul 27 20:55:35 CST 2017确定筛选条件--------------
Thu Jul 27 20:55:56 CST 2017点击工作台--------------
Thu Jul 27 20:55:57 CST 2017进入我的页面--------------
Thu Jul 27 20:56:00 CST 2017进入我的设置页面--------------
Thu Jul 27 20:56:01 CST 2017点击退出登录页面--------------
Thu Jul 27 20:56:05 CST 2017点击确定退出按钮--------------
Thu Jul 27 20:56:06 CST 2017测试成功！--------------
```


