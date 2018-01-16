# UTF-7 系列

## 所收编码
### 实际存在
- [UTF-7](https://tools.ietf.org/html/rfc2152)
- [MUTF-7](https://tools.ietf.org/html/rfc3501)

## 解说
UTF-7 不是普通的编码，而是将 Unicode 以 ASCII 字符串的形式表示的一种编码形式。这是由于过去 SMTP 仅能传输 7 比特。

MUTF-7 作为 UTF-7 的修改版本，用于邮箱名，将一些特殊字符进行了修改。

## 与 UTF-16 的对应关系（UTF-7）
首先见 [UTF-16](https://github.com/mrhso/IshisashiEncoding/tree/master/UTF/UTF-16)。

那么对于 UTF-7 的具体编码规则的阐述如下。

### 直接编码字符
有一些字符可以不转义而直接编码，这样的字符叫「直接编码字符」。

|十六进制|字符|
|-|-|
|27|'|
|28|(|
|29|)|
|2C|,|
|2D|-|
|2E|.|
|2F|/|
|30|0|
|31|1|
|32|2|
|33|3|
|34|4|
|35|5|
|36|6|
|37|7|
|38|8|
|39|9|
|3A|:|
|3F|?|
|41|A|
|42|B|
|43|C|
|44|D|
|45|E|
|46|F|
|47|G|
|48|H|
|49|I|
|4A|J|
|4B|K|
|4C|L|
|4D|M|
|4E|N|
|4F|O|
|50|P|
|51|Q|
|52|R|
|53|S|
|54|T|
|55|U|
|56|V|
|57|W|
|58|X|
|59|Y|
|5A|Z|
|61|a|
|62|b|
|63|c|
|64|d|
|65|e|
|66|f|
|67|g|
|68|h|
|69|i|
|6A|j|
|6B|k|
|6C|l|
|6D|m|
|6E|n|
|6F|o|
|70|p|
|71|q|
|72|r|
|73|s|
|74|t|
|75|u|
|76|v|
|77|w|
|78|x|
|79|y|
|7A|z|

### 可选直接字符
有一些字符可以选择性地直接编码，这样的字符叫作「可选直接字符」。

一般地，这些字符不会直接编码。但是我们仍然可以选择去直接编码而不转义。

|十六进制|字符|
|-|-|
|21|!|
|22|"|
|23|#|
|24|$|
|25|%|
|26|&|
|2A|*|
|3B|;|
|3C|<|
|3D|=|
|3E|>|
|40|@|
|5B|[|
|5D|]|
|5E|^|
|5F|_|
|60|`|
|7B|{|
|7C|\||
|7D|}|

### 其余的 ASCII 字符讲解
对于「+」（0x2B），将会编码为「+-」（0x2B2D）。

对于「\」（0x5C）和「~」（0x7E），将不会直接编码，因为这些字符往往有特殊意义。

空格（0x20）、Tab（0x09）、CR（0x0D）、LF（0x0A）往往能够直接编码。

而其它的控制字符（0x00\~0x08, 0x0B\~0x0C, 0x0E~0x1F, 0x7F）则不能够直接编码。

### 非直接字符
不选择直接编码的字符叫作「非直接字符」。包含非 ASCII 字符。

编码时以一串连续的非直接字符串为单位编码，然后将 Base64 串放在「+」和「-」之间包裹起来。

UTF-7 使用 Base64，但是不用「=」来补全。

Base64 表：

|二进制|Base64|
|-|-|
|000000|A|
|000001|B|
|000010|C|
|000011|D|
|000100|E|
|000101|F|
|000110|G|
|000111|H|
|001000|I|
|001001|J|
|001010|K|
|001011|L|
|001100|M|
|001101|N|
|001110|O|
|001111|P|
|010000|Q|
|010001|R|
|010010|S|
|010011|T|
|010100|U|
|010101|V|
|010110|W|
|010111|X|
|011000|Y|
|011001|Z|
|011010|a|
|011011|b|
|011100|c|
|011101|d|
|011110|e|
|011111|f|
|100000|g|
|100001|h|
|100010|i|
|100011|j|
|100100|k|
|100101|l|
|100110|m|
|100111|n|
|101000|o|
|101001|p|
|101010|q|
|101011|r|
|101100|s|
|101101|t|
|101110|u|
|101111|v|
|110000|w|
|110001|x|
|110010|y|
|110011|z|
|110100|0|
|110101|1|
|110110|2|
|110111|3|
|111000|4|
|111001|5|
|111010|6|
|111011|7|
|111100|8|
|111101|9|
|111110|+|
|111111|/|

以「绝𪩘多生怪柏」为例，讲述编码过程。

首先，将非直接字符串以 UTF-16 BE 表示。

|字符|UTF-16 BE（十六进制）|
|-|-|
|绝|7EDD|
|𪩘|D86ADE58|
|多|591A|
|生|751F|
|怪|602A|
|柏|67CF|

得到十六进制串：

7EDDD86ADE58591A751F602A67CF

转成二进制并以六位为一组分列，得：

011111 101101 110111 011000 011010 101101 111001 011000 010110 010001 101001 110101 000111 110110 000000 101010 011001 111100 1111

由于最后一组不足六位，补零：

011111 101101 110111 011000 011010 101101 111001 011000 010110 010001 101001 110101 000111 110110 000000 101010 011001 111100 111100

转换成 Base64，得：

ft3Yat5YWRp1H2AqZ88

包裹起来，得到结果：

+ft3Yat5YWRp1H2AqZ88-

即十六进制：

2B667433596174355957527031483241715A38382D

再看解码过程。

将「+ft3Yat5YWRp1H2AqZ88-」去掉包裹，并从 Base64 转回二进制。然后十六位为一组分列：

0111111011011101 1101100001101010 1101111001011000 0101100100011010 0111010100011111 0110000000101010 0110011111001111 00

最后一组全为「0」构成，且不足十六位，去掉：

0111111011011101 1101100001101010 1101111001011000 0101100100011010 0111010100011111 0110000000101010 0110011111001111

然后用 UTF-16 BE 解码，就得「绝𪩘多生怪柏」。