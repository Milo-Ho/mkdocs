# 二进制编码（应用层协议）

大端模式：高地址放低字节，低地址放高字节，跟正常的阅读习惯一致。

小端模式：高地址放高字节，低地址放低字节。

```
int32 x = 0x12345678
大端模式下：\x12\x34\x56\x78
小端模式下：\x78\x56\x34\x12
```

## Protocol Buffer
#### proto2和proto3有什么区别？

#### 压缩
- varint编码+zigzag编码
- optional类型，该变量不存在时直接不占任何字节，连TAG都不需要
- packed，对于repeated类型，把传统的TAG-VALUE1-TAG-VALUE2 变为 TAG-LENGTH-VALUE1-VALUE2，避免重复传TAG
- 没有对float和double进行压缩，所以说protobuf没有压缩到极致

#### varint
- 每个字节的最高位表示是否需要拓展到下一字节，0表示只需要读当前字节，1表示还需要拓展到下一字节
- 使用小端模式

优点：

- varint理论上可以无限长，作为开源代码，很灵活很通用

缺点：

- 每个字节都会浪费1bit，只有7bit可用（利用率只有87.5%）
- 负数的高位都是1，理论上可以有无数个1，在varint下可以占用无数个字节（例如在32位的int下，-1表示为 0xff ff ff ff）

首先是利用率：

- 当一个int32的变量在大部分情况下传输的数字都比较小（例如玩家背包里的钻石数量），使用varint可以有效的较少数据大小。

```
例如，有100个int32的变量，但大部分情况下，每个变量的数值都在1000左右

如果不使用varint，则数据大小是 8字节 * 100 = 800字节

如果使用varint，则表示数值1000只需要1字节，所有数据只有 1字节 * 100 = 100字节
```

- 当一个int32的变量在大部分情况下传输的数字都比较大（例如：时间戳），则不要使用varint编码，改用fixed32/fixed64。
<br/>
protobuf的官方文档上也有标明：
`fixd32: Always four bytes. More efficient than uint32 if values are often greater than 2^28.`

#### zigzag编码
计算方法：
<br/>
32位：`(n << 1) ^ (n >> 31)`
<br/>
64位：`(n << 1) ^ (n >> 63)`

本质：

- 把符号位从最高位移到最低位
- 其他位向左顺延一位，同时，如果是负数，其他位需要按位取反

```
以32位举例：
0  : 0x 00 00 00 00 ==> 0x 00 00 00 00
-1 : 0x ff ff ff ff ==> 0x 00 00 00 01
1  : 0x 00 00 00 01 ==> 0x 00 00 00 02
-2 : 0x ff ff ff fe ==> 0x 00 00 00 03
2  : 0x 00 00 00 02 ==> 0x 00 00 00 04
```

特点：

zigzag编码补充了varint编码在表示负数时的不足。

绝对值比较小的数字，经过zigzag编码之后会变为一个小整数，配合varint编码一起使用，可以有效压缩数据。

官方文档：
```
int32: Uses variable-length encoding. Inefficient for encoding negative numbers - if your field is likely to have negative values, use sint32 instead.
sint32: Uses variable-length encoding. Signed int value. These more efficiently encode negative numbers than regular int32s.
```


#### TAG
- TAG使用varint编码
- TAG由 字段编号 + 数据类型 组成
- TAG的最后3bit表示数据类型，前面的位表示字段编号`(field_number << 3) | wire_type`

##### 字段编号
由上述的TAG的组成方式可知，当TAG仅使用1字节时：
<br/>
第1个bit被varint编码占用，最后3bit表示数据类型，只有中间4bit可以用来表示字段编号，4bit的可以表示的数值范围是 0到15（但字段编号通常不使用0）

因此，**字段编号尽量多用 1-15** （或是，尽量保留1-15给常用字段使用，非常用字段可以考虑从16开始使用）

##### 数据类型
分类：

- 【Tag - Value】varint编码的数字
- 【Tag - Value】定长类型（float/double/fixed32/fixed64）
- 【Tag - Length - Value】变长类型（string/bytes/嵌套的message）
    * 需要额外使用一个varint表示后续Value的长度

---
- int/float/varint 等类型都用的是小端模式
- string/bytes 等类型用的是大端模式

---

基于Protobuf序列化原理分析，为了有效降低序列化后数据量的大小，可以采用以下措施：

1. 多用 optional或 repeated修饰符
<br/>
若optional 或 repeated 字段没有被设置字段值，那么该字段在序列化时的数据中是完全不存在的，即不需要进行编码，但相应的字段在解码时会被设置为默认值。

2. 字段标识号(Field_Number)尽量只使用1-15，且不要跳动使用
<br/>
Tag是需要占字节空间的。如果Field_Number>=16时，Field_Number的编码就会占用2个字节，那么Tag在编码时就会占用更多的字节；如果将字段标识号定义为连续递增的数值，将获得更好的编码和解码性能。

3. 若需要使用的字段值出现负数，请使用sint32/sint64，不要使用int32/int64。
<br/>
采用sint32/sint64数据类型表示负数时，会先采用Zigzag编码再采用Varint编码，从而更加有效压缩数据。

4. 若需要使用的字段值大多数时候比较大（如：时间戳），考虑使用fixed32/fixed64。
<br/>
varint编码中，每个字节都有1bit被占用，只有7bit用于记录数据。当数值较大时，varint无法起到压缩数据的作用，反而浪费每个字节的利用率。

5. 对于repeated字段，尽量增加packed=true修饰
<br/>
增加packed=true修饰，repeated字段会采用连续数据存储方式，即 T - L - V - V - V 方式。


## 项目组协议
第1-4个字节记录协议总长度，最大值2^32-1，理论上能编出来的最大的协议大小为4GB（类似http，协议头中记录总长度）
<br/>
第5-6个字节记录主协议号（主协议号0-65535）
<br/>
第7个字节记录子协议号（子协议号0-255）
<br/>
第8个字节开始记录数据

- 定长的数据类型，结构是 Tag + Value
- 变长的数据类型，结构是 TAG + Length + Value
    - Length是一个定长的int32
	
---
TAG由数据类型+字段编号组成

- 数据类型占第1-3位
- 第4位跟protobuf的最高位类似，用于表示是否需要拓展到下一字节
    - 第4位是0，则表示字段编号占第5-8位，共4bit，最大值15，此时TAG总共占1个字节
    - 第4位是1，则表示字段编号占第5-16位，共12bit，最大值4095，此时TAG总共占2个字节

---
TAG、字符串用的是大端模式
<br/>
int8/int16/int32/int64 等类型用的都是小端模式（包括协议头部的 总长度、主协议号、子协议号、变长类型的数据长度、定长类型的int数据）

