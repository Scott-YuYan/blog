* 1.top设置为-50%无效的原因

对于下面的代码：
```
<div class="skin">
    <div class="mouth"></div>
</div>
```

```
.skin {}

.mouth{
   left:50%
   top:50%
}
```

可以发现mouth只会在水平方向上居中，高度方向不会，原因在于其父节点没有设置高度。

* 2.所有的操作系统，其滚动条的宽度，大致为14px-19px，一般为17px;