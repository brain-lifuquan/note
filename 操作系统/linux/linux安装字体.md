# Linux安装字体

* 字符集
* 汉字编码
  * GB2312 字集 是简体字集 全称为GB2312(80)字集，共包括国标简体汉字6763个
  * BIG5字集 台湾繁体字集，包括国标繁体汉字13053个
  * GBK字集是简繁字集，包括了GB字集、BIG5字集和一些符号，共包括21003个字符
  * GB18030是国家制定的一个强制性大字集标准，全称GB18030-2000，汉字集大一统标准
* ASCII
  * American Standard Code for Information Interchange 美国信息交换标准码
  * ISO 646标准
  * ASCII字符集由控制字符和图形字符组成，在计算机存储单元中，一个ASCII码值占一个字节(8位2进制)，其最高位用作奇偶校验位
* UTF
  * Unicode 的实现方式不同于编码方式。一个字符的Unicode编码是确定的,但是在实际传输过程中，由于不同系统平台的设计不一定一致，以及出于节省空间的目的，对Unicode编码的实现方式有所不同
  * Unicode的实现方式成为Unicode转换格式(Unicode Translation Format，简称为UTF)
  * UTF-8 8bit变长编码，对大多数常用字符集(ASCII)使用单字节，对其他常用字符(特别是朝鲜和汉语会意文字)使用3字节
  * UTF-16 16bit变长编码，大致相当于20位编码，值在0到0x10FFFF之间，基本上就是Unicode编码的实现，与CPU字序有关
* 兼容性最好的编码是UTF-8

* locale
  * Linux中通过locale来设置程序运行的不同语言环境， locale由ANSI C提供支持
  * locale 的命名规则为 <语言>_<地区>.<字符集编码>
  * 如 zh_CN.UTF-8 zh表示中文，CN表示大陆地区，UTF-8表示字符集
* 在locale环境中，有一组变量，代表国际化环境中的不同设置
  * LC_COLLATE 定义该环境的排序和比较规则
  * LC_CTYPE 用于字符分类和字符串处理，控制所有字符的处理方式，包括字符编码、字符是单字节还是多字节、如何打印等
  * LC_MONETARY 货币格式
  * LC_NUMERIC 非货币的数字的显示格式
  * LC_TIME 时间和日期格式
  * LC_MESSAGES 提示信息的语言，另外还有一个LANGUAGE环境便来嗯，一旦设置，则LC_MESSAGES参数会失效
    * LANGUAGE 可以同时设置多种语言信息，如LANGUAGE="zh_CN.GB18030:zh_CN.GB2312:zh_CN"
  * LANG  LC_* 的默认值，是最低级别的设置，如果LC_* 没有设置，则使用该值， 类似于LC_ALL
  * LC_ALL 一个宏，如果该值设置了，则该值会覆盖所有LC_* 的设置值，LANG值不受该宏影响

```sh
[lfq@localhost ~]$ locale
LANG=zh_CN.UTF-8
LC_CTYPE="zh_CN.UTF-8"
LC_NUMERIC="zh_CN.UTF-8"
LC_TIME="zh_CN.UTF-8"
LC_COLLATE="zh_CN.UTF-8"
LC_MONETARY="zh_CN.UTF-8"
LC_MESSAGES="zh_CN.UTF-8"
LC_PAPER="zh_CN.UTF-8"
LC_NAME="zh_CN.UTF-8"
LC_ADDRESS="zh_CN.UTF-8"
LC_TELEPHONE="zh_CN.UTF-8"
LC_MEASUREMENT="zh_CN.UTF-8"
LC_IDENTIFICATION="zh_CN.UTF-8"
LC_ALL=
[lfq@localhost ~]$
```

* ubuntu centos和fedora的字体目录为`/usr/share/fonts`

```sh
fc-list    --help
# 查看已安装字体
fc-list
# 查看已安装的中文字体
fc-list :lang=zh
# fedora 字体目录
[lfq@localhost ~]$ ll /usr/share/fonts
```

* 从windows上获取字体
  * windows目录 `C:\Windows\Fonts`

```sh
# 复制字体文件
[lfq@localhost Fonts]$ sudo cp ./ /usr/share/fonts -r
# 更新字体索引和字体缓存
[lfq@localhost Fonts]$ cd /usr/share/fonts
[lfq@localhost fonts]$ sudo mkfontscale
# 好像所有的fon文件都报错了
[lfq@localhost fonts]$ sudo mkfontdir
Couldnt determine full name for sserifft.fon
[lfq@localhost fonts]$ fc-cache -fv
# 字体已经添加成功了
[lfq@localhost fonts]$ fc-list :lang=zh
```
