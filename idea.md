[TOC]



# 问题

## 1.mac 修改 idea.vmoptions 导致软件打不开

```
IDEA中“.vmoptions”文件有2份
“Help | Edit Custom VM Options”菜单编辑的是Config下的副本文件
针对mac的说明：
不要直接编辑".vmoptions"和".properties"文件（原始文件位于/Applications/<Product>.app/Contents/bin文件夹,
对于较早的IDE版本，为/Applications/<Product>.app/bin），这会违反应用程序签名，请始终在IDE配置目录下创建文件的副本，然后编辑该副本。即在 “Help | Edit Custom VM Options”菜单中执行编辑。
3）Mac OS Config副本文件地址
/Users/yunan10/Library/Application Support/JetBrains/IntelliJIdea2020.1/idea.vmoptions
```

## 2.import本项目的类失败

![img](/Users/yunan10/Desktop/150/md/idea.assets/img.png)

```
如图，本项目的类是存在的，pom也引入对应的包，但还是找不到
解决：菜单中选择File - Invalidate Caches/Restart...
```

## 3.注释模板

### 类模版：

## 4.idea debug 项目一直debug启动不了,不报错，run项目却可以启动

```
存在方法断点
```

## 5.增加idea内存

Help Edit Custom VM Options

![image-20220512165120778](/Users/yunan10/Desktop/150/md/idea.assets/image-20220512165120778.png)

# 插件

## 1.RestFulTool

快速搜接口

# 快捷键

| 快捷键       | 说明         |
| ------------ | ------------ |
| `Ctrl+Alt+C` | 将值变成常量 |
| Shift + f6   | 批量重命名   |
|              |              |

# 设置

## 1.多个项目放在一个窗口

效果图如下

![多个项目在同一个窗口中打开](https://img2020.cnblogs.com/blog/1400924/202102/1400924-20210228192003995-1419673918.png)

![image-20220512163854174](/Users/yunan10/Desktop/150/md/idea.assets/image-20220512163854174.png)

Mac 还需设置一下

![mac 设置](https://img2020.cnblogs.com/blog/1400924/202102/1400924-20210228192004882-1466913943.png)


