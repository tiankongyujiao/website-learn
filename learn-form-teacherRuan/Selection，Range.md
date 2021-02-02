### Range:Range 接口表示一个包含节点与文本节点的一部分的文档片段。
可以用 Document 对象的 Document.createRange 方法创建 Range，也可以用 Selection 对象的 getRangeAt 方法获取 Range。另外，还可以通过 Document 对象的构造函数 Range() 来得到 Range。
+ Range.collapsed：返回一个表示 Range 的起始位置和终止位置是否相同的布尔值。
+ Range.commonAncestorContainer：返回完整包含 startContainer 和 endContainer 的、最深一级的节点。
+ Range.endContainer：返回包含 Range 终点的节点。
+ Range.endOffset：返回一个表示 Range 终点在 endContainer 中的位置的数字
+ Range.startContainer：返回包含 Range 开始的节点。
+ Range.startOffset：返回一个表示 Range 起点在 startContainer 中的位置的数字。
以上都是只读属性。Range还有很多定位方法和编辑方法，以及其他方法，移步MDN文档：https://developer.mozilla.org/zh-CN/docs/Web/API/Range

### Selection
Selection 对象表示用户选择的文本范围或插入符号的当前位置。可以使用window.getSelection().toString()获取当前页面选择的元素内容。
通过
```
var selObj = window.getSelection();
var range  = selObj.getRangeAt(0);
```
获取一个Range


### Selection.getRangeAt()
> range = sel.getRangeAt(index)
**返回值**：返回一个包含当前选区内容的区域对象Range。  
**参数**：
+ range: 将返回 range 对象。
+ index: 该参数指定需要被处理的子集编号（从零开始计数）。如果该数值被错误的赋予了大于或等于 rangeCount 结果的数字，将会产生错误。  
```
let ranges = [];

sel = window.getSelection();

for(var i = 0; i < sel.rangeCount; i++) {
 ranges[i] = sel.getRangeAt(i);
}
/* 在 ranges 数组的每一个元素都是一个 range 对象，
 * 对象的内容是当前选区中的一个。 */
 ```
 
