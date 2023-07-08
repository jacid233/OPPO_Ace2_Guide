# OPPO Ace2 玩机指南

手机信息：

## Device configuration for OPPO Ace2

The OPPO Ace2 (codenamed *"OP4AD9"*) is a high-end smartphone from OPPO.

It was announced & released in April 2020.

### Device specifications

| Basic                   | Spec Sheet                                                   |
| ----------------------- | ------------------------------------------------------------ |
| SoC                     | Qualcomm Snapdragon 865 SM8250                               |
| CPU                     | Octa-Core (1 x 2.84 GHz Kryo 585 & 3 x 2.42 GHz Kryo 585 & 4x1.8 GHz Kryo 585) |
| GPU                     | Adreno 650                                                   |
| Memory                  | 8/12 GB RAM                                                  |
| Shipped Android Version | 10.0 with ColorOS 7.1                                        |
| Storage                 | 128/256GB                                                    |
| Battery                 | Non-removable Li-Po 4000 mAh battery                         |
| Display                 | 2400 x 1080 pixels, 6.5 inches, 90Hz, Super AMOLED (~402 ppi density) |
| Rear Camera             | 48MP (wide) / 8MP (ultra-wide) / 2MP (depth) / 2MP (depth)   |
| Front Camera            | 16MP (wide)                                                  |
| Model                   | PDHM00                                                       |
| OLED Model              | oppo19161samsung_amb655uv01_1080_2400_cmd                    |
| Touch Panel             | sec-s6sy771                                                  |

### Device picture

![Ace2](https://dsfs.oppo.com/product/2020/ace2/images/parameter/phone-img1-768-41a18d9f8d94842abf4de4c74502ab87.png "Ace2")



## 官方的更新、降级与备份工具

* GhostTool
* OplusUpgradeTool

GhostTool已经下架了，适用于ColorOS11降级到Coloros7.1以及备份之类

OplusUpgradeTool好像还在，适用于ColorOS12.1降级到ColorOS11以及备份之类

OplusUpgradeTool下载链接：https://img-oppo-cn.oss-cn-hangzhou.aliyuncs.com/uploads/util/2022/02/23/1645585834165.zip



## 禁用列表

> 适用于realmeui和coloros，在不同安卓版本基本通用

手机管家

```bash
adb shell pm uninstall --user 0 com.coloros.phonemanager
```

软件更新

```bash
adb shell pm uninstall --user 0 com.oppo.ota
```

系统升级服务

```bash
adb shell pm uninstall --user 0 com.coloros.sau
```

更新服务

```bash
adb shell pm uninstall --user 0 com.nearme.romupdate
```

> 提示：如果想优雅地关闭系统更新，其实你在官方提供的GhostTool或者OplusUpgradeTool关掉即可，就不用禁apk那么烦

视频

```bash
adb shell pm uninstall --user 0 com.heytap.yoli
```

音乐

```bash
adb shell pm uninstall --user 0 com.heytap.music
```

小游戏

```bash
adb shell pm uninstall --user 0 com.nearme.play
```

Athena

```bash
adb shell pm uninstall --user 0 com.coloros.athena
```

软件商店

```bash
adb shell pm uninstall --user 0 com.heytap.market
```

主题商店

```bash
adb shell pm uninstall --user 0 com.heytap.themestore
```

浏览器

```bash
adb shell pm uninstall --user 0 com.heytap.browser
```

### 如何恢复？

```bash
 pm install-existing --user 0 包名
```

### 需要注意的问题：

主题、软件商店、手机管家在禁用前不能打开或者对手机进行联网，否则就禁用失败



## 内核

官方开源的内核，目前只开了两个版本

* 安卓10，ColorOS7.1
* 安卓12.0，ColorOS12.1
* 安卓13，ColorOS13 直接使用 ColorOS12.1的内核即可

至于安卓11，官方没有开源，但是早期版本（ColorOS11, C11)可以用findx2的内核源码。由于ColorOS11的内核源码SELinux部分被魔改了，因此无法切换permissive。当然办法也有，那就是用rebase kernel。



## 线刷

线刷方式主要有两种：

* fastboot线刷

* 9008端口线刷

  

### 售后线刷固件(.ofp)

> 下面的fastboot线刷和9008线刷都是在售后泄漏的线刷固件解压镜像镜像刷写的！售后线刷固件一般都是被打包成 .ofp 的加密压缩格式！

根据我使用的经验，网上流传的几乎是ColorOS7.1和ColorOS11的售后线刷固件，然而ColorOS7.1的售后线刷固件<font color=red>刷了会炸</font>！我只保存有ColorOS11 C17的售后线刷固件，它可以在非ColorOS7.1的系统的背景下（意思是刷机前的系统不是7.1！）正常刷入。

顺便分享线刷固件等工具（百度云盘）：

链接：https://pan.baidu.com/s/1ood2skiB0IJevjIi9u_eGg 
提取码：ace2 



### fastboot 线刷

以下是我的线刷脚本内容：

```bash
fastboot flash boot boot.img

fastboot flash recovery recovery.img

fastboot flash dtbo dtbo.img

fastboot flash opporeserve2 opporeserve2.img

fastboot flash modem NON-HLOS.bin

fastboot flash vbmeta vbmeta.img --disable-verity --disable-verification

fastboot flash vbmeta_system vbmeta_system.img

fastboot flash vbmeta_vendor vbmeta_vendor.img

fastboot flash super super.0.59d26645.img

fastboot flash super super.1.f862e808.img

fastboot flash super super.2.6836a383.img

```

其中，NON-HLOS.bin 是基带，如果是用着ColorOS11的话可以不用刷，如果刷机后IMEI丢失，请刷入它吧！

### 9008线刷

> 也许很多人说，9008线刷要去售后搞呀，毕竟官方的9008线刷工具是绑定设备的。其实现在也不用这么做了，也可以自己进入9008通过EMT刷机工具来线刷！

EMT刷机工具线刷方法是最近才有的。之所以能通过EMT来免售后9008刷机，是因为865的9008引导最近才被破解！

[EMT 刷机工具官网](http://www.emegsm.com/cn/Index.asp)

也许玩家们一开始听说oddo能免售后通过9008线刷挺高兴的，结果看了一下这个工具发现是收费软件：<font color=red>价格：468 RMB</font>

有了工具别高兴得太早，你还缺少包呢，用官方售后的**.ofp**包好像是不行的，这要看有谁用EMT刷机工具把系统镜像给备份下来的，反正当然有人备份有镜像。不过9008刷机有个问题，就是指纹需要重新校验！



## TWRP相关

> 这东西是真的恶臭，因为data解密方案没人愿意公开，甚至搞twrp收费的恶臭事

### TWRP通用那些事

目前我发现，Ace2的TWRP和findx2系列是通用的，唯一不同是机型校验，Ace2是OP4AD9，findx2是OP4A7A。Ace2的data加密方案，和findx2，一加8，Realme x50p是一致的。也许realme x50p的也能用（一加8是A/B分区不适合此设备）！

### TWRP-ColorOS7.1

其实我是不想搬运它的，因为ColorOS7.1的系统架构和后面的差异很大（在ColorOS7.1的背景下，刷入ColorOS11/12.1或者ColorOS11/12.1刷ColorOS7.1会炸机），不利于搞机。我用的是wzsx150的。

### TWRP-ColorOS11

我是用残心的，因为残心作者对twrp进行了加密，刷入需要用它给的工具。为了方便，我就直接用 `dd` 命令把它的twrp备份出来，这样就无需解密了。

### TWRP-ColorOS12.1

因为目前已经没有人给ace2适配twrp了，发现findx2的和ace2通用，所以直接用findx2的。

### TWRP-ColorOS13.0

继续使用findx2的

### TWRP收集

* ColorOS7.1 (wzsx150): https://wwfg.lanzoum.com/i8rcF09kvzta
* ColorOS11 (残芯): https://wwfg.lanzoum.com/i9mPA0ztcggj

<br/>

> 注意：ColorOS12.1和ColorOS13的并没有Ace2的twrp，因为findx2的twrp能兼容Ace2，所以就去隔壁Findx2找了一份过来。

* ColorOS12.1(pomelohan):https://wwfg.lanzoum.com/iqLv10tzdqze
* ColorOS13.0/1(pomelohan):https://wwfg.lanzoum.com/iW1Xs0tzdeda

## root与vbmeta校验去除

注意：从H.13开始，不再提供修补的magiskroot！

root的话，自己用面具给boot.img打补丁就行，然后在fastboot 模式下把修改过的boot.img刷入就行

刷入命令如下：

```bash
fastboot flash boot boot.img
```



vbmeta校验去除，是必须的，提取vbmeta.img通过如下命令关闭校验：

```bash
fastboot flash vbmeta vbmeta.img --disable-verity --disable-verification
```



## 官方OTA更新收录

问：这些OTA包是怎么收录的？

答：通过官方渠道以及ota缓存数据提取的



问：如何通过ota缓存提取包？

答：在/data/data/com.oplus.ota/databases/ota.db 里面有直链、md5、ota包存放位置等信息。



问：全吗？

答：不全，但是这些暂时没有撤包，由于我入坑太晚，ColorOS11和ColorOS12.1的很多都没有办法从ota更新收录



### ColorOS7.1 (安卓10)

其实可以在官网寻找: https://www.coloros.com/rom/firmware?id=1905308017187561910

| 版本 | 链接                                                         |
| ---- | ------------------------------------------------------------ |
| A16  | https://fsopen.coloros.com/3/oppowww/androidrom/pdhm00/PDHM00_11_OTA_0160_all_6e4GglZlOcoC.ozip |
| A17  | https://fsopen.coloros.com/3/oppowww/androidrom/pdhm00/PDHM00_11_OTA_0170_all_GjzlZSrvKPeZ.ozip |
| A18  | https://fsopen.coloros.com/3/oppowww/androidrom/pdhm00/PDHM00_11_OTA_0180_all_O8maZj8A220p.ozip |
| A19  | https://coloroswebsitefs.coloros.com/coloroswebsite-coloros-com/grs_20200628203933/PDHM00_11_OTA_0190_all_ugqL6CUiO2vY.ozip |
| A21  | https://coloroswebsitefs.coloros.com/coloroswebsite-coloros-com/grs_20200728093652/PDHM00_11_OTA_0210_all_xtM1G3FmS9Tr.ozip |
| A23  | https://coloroswebsitefs.coloros.com/coloroswebsite-coloros-com/grs_20200825092834/PDHM00_11_OTA_0230_all_l3Y9ZiHf2Mbj.ozip |
| A24  | 撤包                                                         |
| A25  | https://coloroswebsitefs.coloros.com/coloroswebsite-coloros-com/grs_20201104195555/PDHM00_11_OTA_0250_all_iyi66E9mjnZC.ozip |
| A26  | 需要提取                                                     |



### ozip的解包

> 看到官方ColorOS7.1的ota包，ozip打包是不是看得很烦？

如何解包？

Github有解包工具：[bkerler/oppo_ozip_decrypt: Oppo Firmware .ozip decrypter (github.com)](https://github.com/bkerler/oppo_ozip_decrypt)



### ColorOS11 (安卓11)

#### C.11 (有个bug: 有时候识别成 C.10)

名称：PDHM00_11_OTA_11101510_all_efZPf90otXBz_9ac9da4.ozip

描述：https://otaafs-cost.coloros.com/PDHM00/PDHM00_11.A.11_11101510_202102241120/html/CHN/PDHM00/PDHM00_11.A.11_11101510_202102241120/ZH-CN_ccac_NEW_Template.html?logoType=1&fontType=oppposans

链接(active-url)：https://otafs-cost.coloros.com/PDHM00/PDHM00_11.A.11_11101510_202102241120/patch/CHN/PDHM00/PDHM00_11.A.11_11101510_202102241120/PDHM00_11_OTA_11101510_all_efZPf90otXBz_9ac9da4.ozip

 链接：https://otaafs-cost.coloros.com/PDHM00/PDHM00_11.A.11_11101510_202102241120/patch/CHN/PDHM00/PDHM00_11.A.11_11101510_202102241120/PDHM00_11_OTA_11101510_all_efZPf90otXBz_9ac9da4.ozip

md5：10b98c93980ac1e7374d37e4aa6c2708



### C.22 (ColorOS11最后一个版本)

| id   | 名称          | 链接(active-url)                                             | 链接                                                         | md5                              |
| ---- | ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- |
| 1    | my_manifest   | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/1b803457ba3747e789f5eae6e52c08dd.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/1b803457ba3747e789f5eae6e52c08dd.zip | eb7618148f1eb75f1be6cb653d1144ad |
| 2    | my_stock      | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/bbc8651c4e0a4517bd13d5579ac70cc7.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/bbc8651c4e0a4517bd13d5579ac70cc7.zip | 6a44eef983c285a08124fd7dfd3acf2a |
| 3    | my_heytap     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/0e865cf427894b368cac6422bb8aed78.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/0e865cf427894b368cac6422bb8aed78.zip | 722389e961a1b4309b2dc645fb6f013b |
| 4    | my_carrier    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/1ad819ca898f45dd8324c06f657288c2.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/1ad819ca898f45dd8324c06f657288c2.zip | 724a584ed54e341d8f9f8fe685893236 |
| 5    | system_vendor | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/f2ded5e7c0d34a22a7617700b036a5e4.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/f2ded5e7c0d34a22a7617700b036a5e4.zip | 1bef20fa63a90c91dcab98f15728f6d0 |
| 6    | my_region     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/a2dd0c39a42f48d1bc6f7e2123990625.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-85a3c7c541ef918294817ce17f1021e2/component-ota/22/02/15/a2dd0c39a42f48d1bc6f7e2123990625.zip | 1a845c91cf28fc810d1f4085c423d1ad |



### ColorOS12.1 (安卓12.0)

#### F.08

| id   | 名称          | 链接(active-url)                                             | 链接                                                         | md5                              |
| ---- | ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- |
| 1    | my_manifest   | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/94a5b952bb9a41df8bdbfd47f13a6793.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/94a5b952bb9a41df8bdbfd47f13a6793.zip | 63f4073b5429d6905552bff1e1288920 |
| 2    | my_product    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/ed9cdd997b7a4a19a3918952afeef737.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/ed9cdd997b7a4a19a3918952afeef737.zip | b9ba510d9db5c9dc68de4f01164013f2 |
| 3    | my_bigball    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/c8bef2f6125f4b4c941eaf2795c26252.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/c8bef2f6125f4b4c941eaf2795c26252.zip | 5daecd834c3e47eaa216804c89cce808 |
| 4    | my_stock      | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/341f7821f4c04be0ad5673394a6b846f.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/341f7821f4c04be0ad5673394a6b846f.zip | 4e3a87e9aafde78b1afcc832bf312dee |
| 5    | my_heytap     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/6f5ecead5dfd439f897f5a7bb83e0249.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/6f5ecead5dfd439f897f5a7bb83e0249.zip | 9290ee1dcd734c8a30468d7b5bea8580 |
| 6    | my_carrier    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/7cdf51c740d5493ca4803b9e76b4aa2a.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/7cdf51c740d5493ca4803b9e76b4aa2a.zip | 43cccb04c322470bb353238866d6c84b |
| 7    | system_vendor | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/36bbd2907e0d482b992e45f6a8500f37.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/36bbd2907e0d482b992e45f6a8500f37.zip | a96e695ad4b34ec6f7ee875f05590453 |
| 8    | my_region     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/6a89f6da593d42fca43c9f28664cefba.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-50e8dc4e4b2525c604f82722bd1c56ee/component-ota/22/04/12/6a89f6da593d42fca43c9f28664cefba.zip | 4009c38b09a6edd7ad90243f970ad754 |



#### F.09

| id   | 名称          | 链接(active-url)                                             | 链接                                                         | md5                              |
| ---- | ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- |
| 1    | my_manifest   | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/7c70e4b1733b48f8b2ba5e3996f4af2d.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/7c70e4b1733b48f8b2ba5e3996f4af2d.zip | 3075aa748d506df60fcd82cf7e4bb212 |
| 2    | my_product    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/0d825d6c1dd742c2ba147afc6520d57f.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/0d825d6c1dd742c2ba147afc6520d57f.zip | 0f0397d6a2f69a2359a15ea169d5a65d |
| 3    | my_bigball    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/999bf2917bdd472a93addcdb77f0e756.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/999bf2917bdd472a93addcdb77f0e756.zip | 747ffe8a9847b20c42ac91e46af2fcb4 |
| 4    | my_stock      | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/04f4b76948b3438abd141362c2d343e3.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/04f4b76948b3438abd141362c2d343e3.zip | b79cdc101a1551109470d428b89eb59c |
| 5    | my_heytap     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/1abcbe6c98fb44179c39ca2a77101967.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/1abcbe6c98fb44179c39ca2a77101967.zip | 89411c3c927b4634c3273b693f520290 |
| 6    | my_carrier    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/e794bf52f2054d66b95daa3f63e5995d.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/e794bf52f2054d66b95daa3f63e5995d.zip | 1607fe9c8c2aab0c8aed28a8e09a2f4a |
| 7    | system_vendor | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/2d6c7228fa904b84831c17e7ad90d924.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/2d6c7228fa904b84831c17e7ad90d924.zip | 2c244b7f4b098f7f67cdc64ba02a5be1 |
| 8    | my_region     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/4942276e6cc3409ba60c37737b031e8f.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-ef9a901485f216ce5991c44d87fef64b/component-ota/22/05/19/4942276e6cc3409ba60c37737b031e8f.zip | 0b21963caed946d8be798d322ee186f7 |



#### F.10

| id   | 名称          | 链接(active-url)                                             | 链接                                                         | md5                              |
| ---- | ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- |
| 1    | my_manifest   | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/8008554eefd94b3aa21989f42a229453.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/8008554eefd94b3aa21989f42a229453.zip | 6c795256b465ff4a0919ef9a5b6d2734 |
| 2    | my_product    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/2c5fdbdb233a4a908571dc8665ad0ae8.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/2c5fdbdb233a4a908571dc8665ad0ae8.zip | 59afb84bf01ee78310336f4814dbaa64 |
| 3    | my_bigball    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/f12e7c0e5d3e409baa50d67a97c62e6f.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/f12e7c0e5d3e409baa50d67a97c62e6f.zip | 82f3f9e2c45ba0103cb2a2be4d0f7b16 |
| 4    | my_stock      | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/6aac4b85a3444601be694dbcb772ee2a.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/6aac4b85a3444601be694dbcb772ee2a.zip | d25da704f3b48bc5a36910151a33670d |
| 5    | my_heytap     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/b29a411a3f864992a138b0b14321f0ae.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/b29a411a3f864992a138b0b14321f0ae.zip | 856ee08c932c8703e7a253d716e0b856 |
| 6    | my_carrier    | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/979ce1a60c514ac7a9ac997ac09d7f3e.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/979ce1a60c514ac7a9ac997ac09d7f3e.zip | 2858da7cd2c570bdc0b9a5a849e8f708 |
| 7    | system_vendor | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/fa76dbcc3e2d4b3aa77bf5b093ec4346.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/fa76dbcc3e2d4b3aa77bf5b093ec4346.zip | b1a874f58c3871bfbbda0d77fc681318 |
| 8    | my_region     | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/2618983ff48b4fada2b695871b2cd9dc.zip | https://gauss-compotacostauto-cn.allawnfs.com/remove-642e031be517bbbd371dd27ffc734c1c/component-ota/22/07/27/2618983ff48b4fada2b695871b2cd9dc.zip | 22fb477e1026fdd136bd4b751147fd6a |


#### F.12

| id   | 名称          | 链接(active-url)                                             | 链接                                                         | md5                              |
| ---- | ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------- |
| 1    | my_manifest   | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/c6b2cb964155493abf7ae9cbd43dd373.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/c6b2cb964155493abf7ae9cbd43dd373.zip | dbcbe58b17f63b52ae30de2f9c53f7c9 |
| 2    | my_product    | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/3586f2b9ca9044e9b3d88ce430e9ce61.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/3586f2b9ca9044e9b3d88ce430e9ce61.zip | d848e8bdfb91ad4e6b4c1e168e96ad91 |
| 3    | my_bigball    | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/45e9346b136a42acb28984bf08ab73f9.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/45e9346b136a42acb28984bf08ab73f9.zip | 7e0b5584023e7b876c8869f66e3e0999 |
| 4    | my_stock      | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/de20796bb92c4d41bcd45ed14efc3ba4.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/de20796bb92c4d41bcd45ed14efc3ba4.zip | f15005d9c1826cdeaf3926c1bde6c53f |
| 5    | my_heytap     | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/fae17ae97ec94b8baeff5f2284664ec7.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/fae17ae97ec94b8baeff5f2284664ec7.zip | 4afda1f4c4f34152c32ef29fc80f77de |
| 6    | my_carrier    | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/d604b216948f4cfeb5fc4b3306fb0b6a.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/d604b216948f4cfeb5fc4b3306fb0b6a.zip | 6dc412bff6f85be93db6848ad2565406 |
| 7    | system_vendor | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/62c3b40d9074417b98e5df576a74b613.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/62c3b40d9074417b98e5df576a74b613.zip | c503ac93f9300a3366aa500fbd5371d1 |
| 8    | my_region     | https://gauss-compotacostauto-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/032da4dadfee453fb5a124899f9eee54.zip | https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a617869abb832d769e26ff49306cd43c/component-ota/22/11/21/032da4dadfee453fb5a124899f9eee54.zip | 287cbc1994ce5072c1770aa867de57a5 |


### ColorOS13.0 (安卓13.0)

#### H.04(内测)

下载链接（2选1，包是一样区别在于带宽速度）：

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-ef1f60545eaab26e6538a8b59fc07182/component-ota/22/10/18/a0e1a93a0e8f49e6b43f2a65b09456ce.zip

https://gauss-compotacostauto-cn.allawnfs.com/remove-ef1f60545eaab26e6538a8b59fc07182/component-ota/22/10/18/a0e1a93a0e8f49e6b43f2a65b09456ce.zip



#### H.06(内测)

下载链接

增量包（没啥用，仅用于参考改动了啥，685.3MB）

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-f0930fdfc4216b8f9cf0ee4f5b167b6c/component-ota/22/11/02/43eeab5c3eb04afdb2560b23b1f30f1f.zip

全量包（下载这个吧，5.647GB）

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-f0930fdfc4216b8f9cf0ee4f5b167b6c/component-ota/22/11/02/09862692a32f4e909b528a111598a93d.zip

更新了啥，我只知道更新了xbl的bootloader解锁提示，就是开机那个橙色的提示，其实也不要慌，看看谷歌的文档是咱解释的

[启动流程  | Android 开源项目  | Android Open Source Project (google.cn)](https://source.android.google.cn/docs/security/features/verifiedboot/boot-flow)

[启动流程  | Android 开源项目  | Android Open Source Project](https://source.android.com/docs/security/features/verifiedboot/boot-flow)


#### H.07(内测)

下载链接（2选1，包是一样区别在于带宽速度，5.663GB）

https://gauss-compotacostauto-cn.allawnfs.com/remove-586039cb72d6523950031868e2391ec8/component-ota/22/11/10/10e51f96e1d545ba97b212155c8cca64.zip

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-586039cb72d6523950031868e2391ec8/component-ota/22/11/10/10e51f96e1d545ba97b212155c8cca64.zip

md5：392efd0412e4ff481ee72a5c359ae762


#### H.09(内测)

下载链接（2选1，包是一样区别在于带宽速度，5.663GB）

https://gauss-compotacostauto-cn.allawnfs.com/remove-6833b341e01e4d8ab260388b9da8542a/component-ota/22/11/17/0e80accab4f2408c9c4f35408b4a01a9.zip

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-6833b341e01e4d8ab260388b9da8542a/component-ota/22/11/17/0e80accab4f2408c9c4f35408b4a01a9.zip

md5：6e1fd591ef62f4e4da9de30c8d61e5c0

好像有人反馈，刷了后亮度条变黑


#### H.10(内测)

下载链接（2选1，包是一样区别在于带宽速度，5.663GB）

https://gauss-compotacostauto-cn.allawnfs.com/remove-311538015ebe4c973b63953f9510a9a7/component-ota/22/11/23/1c929d76447b46819829a4e9135a5171.zip

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-311538015ebe4c973b63953f9510a9a7/component-ota/22/11/23/1c929d76447b46819829a4e9135a5171.zip

md5：0c1410eeb9c953ea462d9df4aa7c946e


#### H.13(内测)

下载链接（2选1，包是一样区别在于带宽速度）

https://gauss-compotacostauto-cn.allawnfs.com/remove-1bd562ceb933d8099df3ba518623c4a9/component-ota/22/12/01/fa29bdf59c664e929f50e8387bb242ab.zip

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-1bd562ceb933d8099df3ba518623c4a9/component-ota/22/12/01/fa29bdf59c664e929f50e8387bb242ab.zip

md5：4b039bafd9780e4ace16a888be174d71


#### H.12(公测)

下载链接

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-f39201c43e2fde2b3e82dbca905f1a4b/component-ota/22/11/23/8723a5d30adb4dbd8d2f29a91c3c74ff.zip


#### H.15(公测)

下载链接

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-1c46ce2000b89698568e00531bfae0af/component-ota/22/12/09/7f2d33dbdea14cc1b459e4f2a57dabc8.zip 


#### H.16(内测)

下载链接

https://gauss-compotacostauto-cn.allawnfs.com/remove-492a6dcf2c7b214ecdff381421b7bb65/component-ota/22/12/14/b3dc5320726b4b7ebd9e7a1d3b2a6078.zip


#### H.19(正式版)

下载链接

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-b13b6a5b3015cbe3025d12d6cfceb190/component-ota/22/12/30/7e6f8f3e296f4e7b998d4ef18620ba06.zip 

官方更新描述

https://gauss-compotacostauto-cn.allawnfs.com/remove-b13b6a5b3015cbe3025d12d6cfceb190/component-ota/23/01/09/1699570c24d9425885a9a0bcb474b419.html 


#### H.20(正式版)

下载链接(2选1，区别在速度)

https://gauss-compotacostauto-cn.allawnfs.com/remove-1b42e6d0683bceedfb90e1a45ac13c09/component-ota/23/01/17/d516ee2e3b8a49479485bc1e3e88d0d0.zip

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-1b42e6d0683bceedfb90e1a45ac13c09/component-ota/23/01/17/d516ee2e3b8a49479485bc1e3e88d0d0.zip

#### H.21(正式版)

下载链接

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-dbd9798f0d096374aa512cafb0cbe9f3/component-ota/23/03/06/432a0ebfa3ff4b5fa9417124a25d4224.zip



### ColorOS13.1 (安卓13.0)

#### H.27(2选1,区别在速度)

下载链接

https://gauss-compotacostauto-cn.allawnfs.com/remove-cf575d5bcef7451b15c273f3244f3c1f/component-ota/23/05/16/2deb36c85a9d4ebba3709ae99ec86973.zip

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-cf575d5bcef7451b15c273f3244f3c1f/component-ota/23/05/16/2deb36c85a9d4ebba3709ae99ec86973.zip

#### H.28(2选1,区别在速度)

下载链接

https://gauss-compotacostauto-cn.allawnfs.com/remove-537c777b380eb1b3fda05c54b6c0124d/component-ota/23/06/16/5a3bf59f31f24382a85548227c8737be.zip

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-537c777b380eb1b3fda05c54b6c0124d/component-ota/23/06/16/5a3bf59f31f24382a85548227c8737be.zip

#### H.29

下载链接

https://gauss-componentotacostmanual-cn.allawnfs.com/remove-a007d1b2eb1f6c4c97c6a3c88359db03/component-ota/23/06/27/243179fafcea433fb020f6da2dad004f.zip

更新说明

https://gauss-compotacostauto-cn.allawnfs.com/remove-a007d1b2eb1f6c4c97c6a3c88359db03/component-ota/23/06/28/d9f4bfb819ed45c1bce2dc96b01f91d7.html

## Credits

* 感谢逝去的天空：提供大部分内测包
* 花橋：分享H.15；H.19
* 由QQ用户 2919277661 提供 H.20 的 ota.db 并提取直链
* 不吃辣椒多放香菜：分享H.28
