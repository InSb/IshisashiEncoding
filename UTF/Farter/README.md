# 屁牌兼容 1 字节 ASCII、2 字节陆港台日常用、单码最多 3 字节、全覆盖 Unicode 编码系列

## 所收编码
### 架空编码
- [原旧](https://zhuanlan.zhihu.com/p/33140509)
- [原新](https://zhuanlan.zhihu.com/p/33140509)
- [新新](https://zhuanlan.zhihu.com/p/33140509)

## 解说
这是一款船新的
~（且丧心病狂的）~
编码，由[屁大爷](https://github.com/farteryhr)倾心力造，堪比贪玩懒月（

具有如下特性：
- 去他喵的自同步
- 去他喵的查找假阳性
- 去他喵的分隔符兼容

能塞就塞，硬塞到三字节。

实际上原旧压根就没覆盖 BMP，到了原新就补齐了。

## 字节结构
原旧如下：

|字节数|第一字节|第二字节|第三字节|码位数|注释|
|-|-|-|-|-|-|
|单字节|0x00~0x7F|||128||
|双字节|0x80~0xEF|0x00~0xFF||28672||
|三字节|0xF0~0xFF|0x00~0xFF|0x00~0xFF|1048576||

原新和新新如下：

|字节数|第一字节|第二字节|第三字节|码位数|注释|
|-|-|-|-|-|-|
|单字节|0x00~0x7F|||128||
|双字节|0x80\~0x82, 0x84\~0xEF|0x00~0xFF||28416||
|三字节|0x83, 0xF0~0xFF|0x00~0xFF|0x00~0xFF|1114112||

## 与 Unicode 的对应关系
### U+0000~U+007F
直接表示成单字节。

### U+3000~U+9FFF（原旧）
Unicode Code Point 加上 0x5000。

### 0x8000~0xEFFF
原旧用来对映 U+3000~U+9FFF。

原新把 0x8200\~0x82FF 改来对映 U+FF00\~U+FFFF，还把 0x8300\~0x83FF 改造成三字节的 0x830000\~0x83FFFF，用来对映 BMP。

新新则在原新的基础上再把 0x82F0~0x82F7 的映射修改，分别映射到 U+2018、U+2019、U+201C、U+201D、U+2014、U+2026、U+00B7、[待定]。0x82F7 暂时没有变动，未来可能考虑修改。

### 三字节 BMP（原新、新新）
直接把 Unicode Code Point 表示成双字节，在前面加上一个字节「0x83」。

### non-BMP
Unicode Code Point 加上 0xEF0000。