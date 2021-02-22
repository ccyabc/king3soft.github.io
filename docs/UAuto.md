---
navbar: false
sidebar: auto
title: 自动化测试
---
 
## U3Driver安装方法
https://king3soft.github.io/u3driver

## python端与dll包实现连接的方式
1. 在cmd输入命令adb devices即可获得当前连接的设备的设备号
<img :src="$withBase('/09.png')" alt="09">

2. 在控制台输入命令，-s与-i参数获取测试机的设备号与ip地址（argparse库）
<img :src="$withBase('/03.png')" alt="03">

::: tip 注意：
一般只需要输入-s 设备号即可，脚本会通过adb找到输入设备号的ip地址，部分机型除外（三星）
:::

## 基本的python指令
::: tip 注意：
u3driver以函数形式提供触屏手机所需要的动作指令，python端以字符串形式通过TCP发送给在游戏里的dll包（仅可对NGUI或UGUI控件进行操作）
:::

最常用的指令包括录制（debug_mode()）、回放（文件名.AutoRun(udriver)）、获取游戏物体（find_object()）、判断物体是否存在（object_exist()）、获取父物体下所有子物体的名字（find_child()）、按索引获取父物体下的子物体(child_index())、按名字获取父物体的直接子物体（find()）,具体指令可通过上述u3driver笔记链接查看
<img :src="$withBase('/10.png')" alt="10">

示例1. 对路径为"//Canvas//StartButton"的游戏对象进行点击
``` js
udriver.find_object(By.PATH,"//Canvas//StartButton").tap()
```

示例2. 如果存在路径为"//UICamera//Loadout//OpenLeaderboard"的游戏对象，进行点击
```js
if udriver.object_exist(By.PAth,"//UICamera//Loadout//OpenLeaderboard")
    udriver.find_object(By.PATH,"//UICamera//Loadout//OpenLeaderboard").tap()
```

示例3. 发现路径为"//Canvas//Background//TabsSwitch//Character"的游戏物体下的所有子物体
```js
udriver.find_child("//Canvas//Background//TabsSwitch")
```

示例4. 得到路径为"//Canvas//Background//TabsSwitch//Character"的游戏物体的文本
```js
udriver.find_object(By.PATH, "//UICamera//Loadout//SettingPopup//Background//MasterSlider//Text").get_text()
```

示例5. 发现路径为"//UI//UI_ShopNew//PnlStory//ToggleTab//List"的游戏物体下的索引为3的子物体，找到该子物体里面的Text，得到Text的文本
```js
udriver.find_child("//Canvas//Background//TabsSwitch")
```


## 常用的库-uiautomator2
:::
使用python对Android设备进行UI自动化的库，无NGUI控件时用于模拟屏幕操作，更多uiautomator2操作可查看介绍文档https://github.com/openatx/uiautomator2
注：不同设备的屏幕坐标可能不同
:::
1. 导入库
``` js
import uiautomator2 as u2
```

2. 初始化uiautomator2
``` js
uiauto = u2.connect(udriver.appium_driver)
```

3. 常用的操作
``` js
uiauto.click(x,y)  # 模拟点击操作

uiauto.long_click(x,y,0.5) # 模拟长按操作

uiauto.drag(x1,y1,x2,y2,0.5) # 模拟拖动操作

uiauto.swipe(x1,y1,x2,y2,0.5) # 模拟滑动操作
```

## 常见报错汇总
1. 设备未找到，解决方法：检查设备号输入是否有误或重新拔插usb数据线


2. 显示与ip地址获取有关的错误是因为设备使用adb获取ip地址的方式比较特殊，解决方法：手动输入 -i IP地址

3. 未找到该物体，解决方法：查看该控件是否在当前的游戏界面中，可通过debug_mode录制确认路径是否有误

4. 越界错误，一般出现在寻找父物体下的子物体时，父物体下无该索引的子物体即报错，解决方法：修改child_index()里的索引进行寻找，要确保该父物体下是有子物体存在的

5. 目标计算机拒绝连接，可能是测试机13000端口已被占用、未打开游戏，ip地址输入错误，或者测试机终止了进程，解决方法：检查输入ip地址，或杀掉游戏进程，重新打开游戏

## 数据库操作

## python环境打包