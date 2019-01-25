#   Less
### 基础用法
***
####    变量
1.  值变量
``` less
@red: #e56a69;
div {
    color: @red;
}
```
2.  选择器变量
``` less
@mySelector: #wrap;
@Wrap: wrap;
@{mySelector} {
    color: blue;
}
#@{Wrap} {
    color: #ccc;
}
```
3.  属性变量
``` less
@borderStyle: border-style;
@Solid: solid;
div {
    @{borderStyle}: @Solid;
}
```

