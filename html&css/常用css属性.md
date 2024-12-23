# 常用css属性

### box-sizing
- content-box：默认属性，元素宽高 = 内容宽高
标准盒模型，一个块的总宽度 = width + margin + padding + border
- border-box：元素宽高 = 内容宽高 + 内边距 + 边框
IE盒模型，一个块的总宽度= width + margin （其中width已经包含了padding和border）

### position
>用于指定一个元素在文档中的定位方式，并配合top/right/bottom/left决定元素最终位置
- 默认定位static：默认值，此时元素的top/bottom/left/right/z-index声明都将无效。
- 相对定位relative：元素不脱离文档流，占据原来的空间，并在原来的位置上进行偏移。
- 绝对定位absolute：元素脱离文档流，依据最近的非static定位祖先元素进行偏移。
- 固定定位fixed：元素脱离文档流，依据屏幕视口进行偏移。当元素祖先的 transform、perspective、filter 或 backdrop-filter 属性非 none 时，容器由视口改为该祖先。该属性会创建层叠上下文。
- 粘性定位sticky：元素不脱离文档流，会依据其最近滚动祖先和最近块级祖先进行定位，在阈值前会随着文档流移动，阈值后则根据祖先进行偏移。该属性会创建层叠上下文。
