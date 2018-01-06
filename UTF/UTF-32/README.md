# UTF-32 系列

## 所收编码
### 实际存在
- [UTF-32 BE](https://www.unicode.org/versions/Unicode10.0.0/ch03.pdf#G28875)
- [UTF-32 LE](https://www.unicode.org/versions/Unicode10.0.0/ch03.pdf#G36145)

## 解说
UTF-32 BE 为大端序存储的 UTF-32。常加 BOM「0x0000FEFF」。

UTF-32 LE 为小端序存储的 UTF-32。常加 BOM「0xFFFE0000」。

## 字节结构
以下码位数统计剔除过剩码位。

UTF-32 BE 如下：

|字节数|第一字节|第二字节|第三字节|第四字节|码位数|注释|
|-|-|-|-|-|-|-|
|四字节|0x00|0x00~0x10|0x00~0xFF|0x00~0xFF|1112064|0x0000D800~0x0000DFFF 通常不认为是合法码位。|

UTF-32 LE 如下：

|字节数|第一字节|第二字节|第三字节|第四字节|码位数|注释|
|-|-|-|-|-|-|-|
|四字节|0x00~0xFF|0x00~0xFF|0x00~0x10|0x00|1112064||

## 与 Unicode 的对应关系（UTF-32 BE）
直接将 Unicode Code Point 表示成四字节。

### U+D800~U+DFFF
尽管表格列出来了，但是这一块不会定义任何字符，都是拿来给 UTF-16 表示 non-BMP 的。

## 大端序和小端序
对于 UTF-32 LE，只需要把 UTF-32 BE 每四字节倒过来存储就好了。

例如：

|字符|UTF-16 BE|UTF-16 LE|
|-|-|-|
|一|0x00004E00|0x004E0000|
|𠀀|0x00020000|0x00000200|