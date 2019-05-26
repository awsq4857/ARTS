# [用 Python 分析所有微信好友](https://mp.weixin.qq.com/s/IvnFTvzSEBsGcf5EFFnx0A)

这两天，看到一个比较好玩的东西，Python 调用微信 API。简单学习了一下，以后没准会用到。

## 1.微信登录

* 实现登录

```
import itchat

itchat.auto_login(hotReload=True)
itchat.dump_login_status()
```

* 好友信息获取

```
we_friend = itchat.get_friends(update=True)[:]
```

```
UserName	微信系统内的用户编码标识
NickName	好友昵称
Sex	性别
Province	省份
City	城市
HeadImgUrl	微信系统内的头像URL
RemarkName	好友的备注名
Signature	个性签名
```

* 发送消息

```
// 搜索获取
author = itchat.search_friends(nickName='LittleCoder')[0]
author.send('greeting, littlecoder!')

// itchat发送
itchat.send_msg(u'哈哈😆',toUserName=friend["UserName"])
```

* 用户搜索

```
# 获取自己的用户信息，返回自己的属性字典
itchat.search_friends()
# 获取特定UserName的用户信息
itchat.search_friends(userName='@abcdefg1234567')
# 获取任何一项等于name键值的用户
itchat.search_friends(name='littlecodersh')
# 获取分别对应相应键值的用户
itchat.search_friends(wechatAccount='littlecodersh')
# 三、四项功能可以一同使用
itchat.search_friends(name='LittleCoder机器人', wechatAccount='littlecodersh')
```

## 2.好友性别

```
total = len(we_friend[1:])
for fri_info in we_friend[1:]:
    sex = fri_info['sex']
    # 如果sex=1 代表男性 sex=2代表女性
    if sex == 1:
        man += 1
    elif sex == 2:
        woman += 1
    else:
        other += 1
```

* 绘制饼图

```
man_ratio = int(man)/total * 100
woman_ratio = int(woman)/total * 100
other_ratio = int(other)/total * 100

plt.rcParams['font.sans-serif'] = ['SimHei']    # 用来正常显示中文标签
plt.rcParams['axes.unicode_minus'] = False  # 用来正常显示负号
plt.figure(figsize=(5, 5))  # 绘制的图片为正圆
sex_li = ['男', '女', '其他']
radius = [0.01, 0.01, 0.01]  # 设定各项距离圆心n个半径
colors = ['red', 'yellowgreen', 'lightskyblue']
proportion = [man_ratio, woman_ratio, other_ratio]

plt.pie(proportion, explode=radius, labels=sex_li, colors=colors, autopct='%.2f%%')   # 绘制饼图

# 加入图例 loc =  'upper right' 位于右上角 bbox_to_anchor=[0.5, 0.5] # 外边距 上边 右边 borderaxespad = 0.3图例的内边距
plt.legend(loc="upper right", fontsize=10, bbox_to_anchor=(1.1, 1.1), borderaxespad=0.3)

# 绘制标题
plt.title('微信好友性别比例')    

# 展示
plt.show()
```

**如何解决matplotlib中文乱码**

* 准备好想要使用的中文字体，这里我用的是SimHei
* 找到matplotlib的文件位置

```
import matplotlib
print(matplotlib.matplotlib_fname())    # 查看路径
```

* 进入上方打印的路径
* 把刚才下载的字体文件解压放入/usr/local/lib/python3.5/dist-packages/matplotlib/mpl-data/fonts/ttf 目录
* 返回上级目录，修改matplotlibrc文件，取消相关注释，并在font.serif加入刚才下载的字体

```
font.family        : sans-serif
font.serif         : SimHei, DejaVu Serif, Bitstream Vera Serif, New Century Schoolbook, Century Schoolbook L, Utopia, ITC Bookman, Bookman, Nimbus Roman No9 L, Times New Roman, Times, Palatino, Charter, serif
```

* 删除matplotlib缓存

```
在terminal中：cd ~/.cache/matplotlib

把.cache下面的matplotlib文件夹删除。

$ rm -rf matplotlib
```

[ItChat](https://github.com/littlecodersh/ItChat)
