# Shell 拾遗

* 替换文件
``` 
使用 sed 替换字符串
t1 = 'abc123'
res=$(echo $var | sed 's/123/ABC/g')
得到结果: abcABC
```