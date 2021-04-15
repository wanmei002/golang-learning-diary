# strings 包的学习
## strings.Index(s, substr string) index
返回 substr 在 s 中的位置，是以 ascii 码为单位的，如果有汉字，一个汉字当三个字节

如果想把汉字也当成一个长度，可以用以下自己封装的函数
```go
func Utf8Index(str1, str2 string) int {
    i := strings.Index(str1, str2)
    if i < 0 {
        return i
    }
    
    buf := string(str1[:i])
    
    //str := string(buf)
    
    fmt.Println(buf)
    runeBuf := []rune(buf)
    
    return len(runeBuf)
}
```

## 是否存在某个字符或子串
### strings.Contains(s, substr string) bool
子串 substr 是否存在与 s 中, 存在返回true 否则返回 false

### strings.ContainsAny(s chars string) bool
chars 中任何一个 unicode 字符存在于 s , 如果存在返回true

### strings.ContainsRune(s string, r rune) bool
unicode 字符 r 在 s 中，返回 true


## 子串出现次数
### strings.Count(s, sep string) int
此方法采用 rabin-karp 算法
> 注意: 如果 sep 为空，返回的是 utf8.RuneCountInString(s)+1,
另外，此方法是计算子串在字符串中出现的无重叠的次数


## 字符串分割为 `[]string`
### strings.Fields(s string) []strings
Fields 用一个或多个连续的空格分隔字符串s， 返回子字符串的数组, 空格的定义是 `'\t', '\n', '\v', '\f', '\r', ' ', U+0085 (NEL), U+00A0 (NBSP)`

### strings.FieldsFunc(s string, f func(rune) bool) []strings
s 中的每一个字符用 `f` 方法处理, 如果返回 true， 则切割字符串

### strings.Split(s, sep string) []string
### strings.SplitN(s, sep string, n int) []string
这两个方法用 `sep` 来分割字符串`s`。SplitN 的第三个参数是返回切片的长度, Split(s, sep) 等价于 SplitN(s, sep, -1)
> SplitN 的第三个参数如果小于0，则用 sep 完全分割 s, 如果 n>0, 则分割 n个长度的切片。

### strings.SplitAfter(s, sep string) []string
### strings.SplitAfterN(s, sep string, n int) []string
这两个方法会保留 sep 字符串

## 字符串是否有某些前缀或后缀
### strings.HasPrefix(s, prefix string) bool
`s` 是否以 `prefix` 开始

### strings.HasSuffix(s, suffix string) bool
`s` 中是否以 `suffix` 结尾

## 字符或子串在字符串中出现的位置
### strings.Index(s, sep string) int
sep 在 s中第一次出现的位置 

### strings.IndexByte(s string, b byte) int
b 第一次出现在 s 中的位置

### strings.IndexAny(s, chars string) int
chars 中任何一个 unicode 字符在 s 中出现的位置

### strings.IndexFunc(s string, f func(rune) bool) int
sep 第一次在 s 中出现的位置, sep 满足 f(sep) 返回 true

### strings.IndexRune(s string, r rune) int
跟 IndexByte 类似

### strings.LastIndex(s, sep string) int
sep 最后一次在 s 中出现的位置

### strings.LastIndexByte(s string, c byte) int
### strings.LastIndexAny(s, chars string) int
### strings.LastIndexFunc(s string, f func(rune) bool) int

## 字符串连接操作
### strings.Join(sliceStr []string, sep string) string
sliceStr 切片用 sep 连接成一个字符串

## 字符串重复操作
`strings.Repeat(s string, count int) string`

将 s 重复 count 次

## 字符串过滤
`strings.Map(mapping func(rune) rune, s string) string`
s 中的字符经过 mapping 方法过滤，如果舍弃return -1

## 字符串替换
### strings.Replace(s, old, new string, n int) string
### strings.ReplaceAll(s, old , new string) string

## 字符串左右边修剪
### strings.Trim(s , cutset string) string
### strings.TrimLeft(s , cutset string) string
### func TrimRight(s string, cutset string) string
### func TrimPrefix(s, prefix string) string
### strings.TrimSuffix(s, suffix string) string
### strings.TrimSpace(s string) string
### strings.TrimFunc(s string, f func(rune) bool) string
### strings.TrimLeftFunc(s string, f func(rune) bool) string
### strings.TrimRightFunc(s string, f func(rune) bool) string

## strings 包中的结构体
### strings.Replacer
通过 NewReplacer 来创建 Replacer, 传的参数是成对出现的
```go
re := strings.NewReplacer("\n", "\\n", "\t", "\\t")
str7 := "hello \n world \t !"
fmt.Println(str7)
str6 := re.Replace(str7)
fmt.Println(str6)
```

```
// 结果:
hello 
 world 	 !
hello \n world \t !
```

### Builder 类型
这个 结构体 的作用主要做连接字符串用的，避免用 `+` 号连接字符串, 减少字符串的创建 内存的分配
```go
// 该方法向 b 写入一个字节
func (b *Builder) WriteByte(c byte) error
// WriteRune 方法向 b 写入一个字符
func (b *Builder) WriteRune(r rune) (int, error)
// WriteRune 方法向 b 写入字节数组 p
func (b *Builder) Write(p []byte) (int, error)
// WriteRune 方法向 b 写入字符串 s
func (b *Builder) WriteString(s string) (int, error)
// Len 方法返回 b 的数据长度。
func (b *Builder) Len() int
// Cap 方法返回 b 的 cap。
func (b *Builder) Cap() int
// Grow 方法将 b 的 cap 至少增加 n (可能会更多)。如果 n 为负数，会导致 panic。
func (b *Builder) Grow(n int)
// Reset 方法将 b 清空 b 的所有内容。
func (b *Builder) Reset()
// String 方法将 b 的数据以 string 类型返回。
func (b *Builder) String() string
```





