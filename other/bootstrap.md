## bootstrap学习笔记
##### 1. 所有“列（column）'col-md-*'或者'col-sm-*'或者'col-lg-*'或者'col-xs-*'必须放在 ” .row 内。  
##### 2.将最外面的布局元素 .container 修改为 .container-fluid，就可以将固定宽度的栅格布局转换为 100% 宽度的布局。
```
<div class="container-fluid">
  <div class="row">
    ...
  </div>
</div>
```
##### 3.你的内容应当放置于“列（column）”内，并且，只有“列（column）”可以作为行（row）”的直接子元素。
##### 4.在标题内还可以包含 <small> 标签或赋予 .small 类的元素，可以用来标记副标题。
```
  <h1>h1. Bootstrap heading <small>Secondary text</small></h1>
```
##### 5.通过为表单添加 .form-horizontal 类，并联合使用 Bootstrap 预置的栅格类，可以将 label 标签和控件组水平并排布局。这样做将改变 .form-group 的行为，使其表现为栅格系统中的行（row），因此就无需再额外添加 .row 了。
##### 6.列偏移
  使用 .col-md-offset-* 类可以将列向右侧偏移。这些类实际是通过使用 * 选择器为当前元素增加了左侧的边距（margin）。例如，.col-md-offset-4 类将 .col-md-4 元素向右侧偏移了4个列（column）的宽度。  
  ```
  <div class="row">
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4 col-md-offset-4">.col-md-4 .col-md-offset-4</div>
</div>
 ```
##### 7. 
