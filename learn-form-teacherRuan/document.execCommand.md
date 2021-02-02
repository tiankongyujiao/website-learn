### document.execCommand
使用这个命令来操作可编辑区域的元素，可编辑区域：contenteditable为true时为可编辑。  
当使用contentEditable时，调用 execCommand() 将影响当前活动的可编辑元素。
> bool = document.execCommand(aCommandName, aShowDefaultUI, aValueArgument)

**返回值**：一个 Boolean ，如果是 false 则表示操作不被支持或未被启用。
**参数**：
+ 1. aCommandName：一个 DOMString ，命令的名称，有如下命令：
  + （1）backColor：修改背景颜色
  + （2）bold：开启或关闭选中文字或插入点的粗体字效果
  + （3）ClearAuthenticationCache：清除缓存中的所有身份验证凭据。
  + （4）contentReadOnly：通过传入一个布尔类型的参数来使能文档内容的可编辑性。(IE浏览器不支持)
  + （5）copy：拷贝当前选中内容到剪贴板，有浏览器兼容性问题。
  + （6）createLink：将选中内容创建为一个锚链接。这个命令需要一个hrefURI字符串作为参数值传入。URI必须包含至少一个字符，例如一个空格。（浏览器会创建一个空链接）
  + （7）cut：剪贴当前选中的文字并复制到剪贴板，有浏览器兼容性问题。
  + （8）decreaseFontSize：给选中文字加上 <small> 标签，或在选中点插入该标签。(IE浏览器不支持)
  + （9）defaultParagraphSeparator：更改在可编辑文本区域中创建新段落时使用的段落分隔符
  + （10）delete：删除选中部分.
  + （11）enableAbsolutePositionEditor：启用或禁用允许移动绝对定位元素的抓取器
  + （12）enableInlineTableEditing：启用或禁用表格行和列插入和删除控件。(IE浏览器不支持)
  + （13）enableObjectResizing：启用或禁用图像和其他对象的大小可调整大小手柄。(IE浏览器不支持)
  + （14）fontName：在插入点或者选中文字部分修改字体名称. 需要提供一个字体名称字符串 (例如："Arial")作为参数。
  + （15）fontSize：在插入点或者选中文字部分修改字体大小. 需要提供一个HTML字体尺寸 (1-7) 作为参数。
          想要通过css定义任意字体大小可以使用如下办法：
          ```
          document.execCommand('styleWithCSS', null, true);
            // 先将文字大小设置成1-7号中的任何一个大小
          document.execCommand('fontSize', false, 1);
              // 这时候浏览器会默认将文字包裹一层span，然后css设置在span上,然后我们再去寻找这个span，将其css修改成我们实际想要的字体大小
              let $parent = document.querySelector('正在编辑的元素的父容器');
              let $spans = $parent.querySelectorAll('span');
              let value = '48px'; // 假设我们要将字体大小设置成48px
              $spans.forEach(($span) => {
                let fontSize = $span.style.fontSize;
                 if ('x-small' == fs) {
                    $span.style.fontSize = value;
                 }
          });
          // 其它样式调整也可以用该方法找到对应的span后修改style值实现，只是最后要把font-size='x-small'的样式清空。  
          ```
  + （16）foreColor：在插入点或者选中文字部分修改字体颜色. 需要提供一个颜色值字符串作为参数。
  + （17）formatBlock：添加一个HTML块式标签在包含当前选择的行, 如果已经存在了，更换包含该行的块元素 ，有浏览器兼容性问题。
  + （18）forwardDelete：删除光标所在位置的字符。 和按下删除键一样。
  + （19）heading：添加一个标题标签在光标处或者所选文字上。
  + （20）hiliteColor：更改选择或插入点的背景颜色。需要一个颜色值字符串作为值参数传递。 UseCSS 必须开启此功能。(IE浏览器不支持)
  + （21）increaseFontSize：在选择或插入点周围添加一个BIG标签。(IE浏览器不支持)
  + （22）indent：缩进选择或插入点所在的行， 在 Firefox 中, 如果选择多行，但是这些行存在不同级别的缩进, 只有缩进最少的行被缩进。
  + （23）insertBrOnReturn：控制当按下Enter键时，是插入 br 标签还是把当前块元素变成两个。(IE浏览器不支持)
  + （24）insertHorizontalRule：在插入点插入一个水平线（删除选中的部分）
  + （25）insertHTML：在插入点插入一个HTML字符串（删除选中的部分）。需要一个个HTML字符串作为参数。(IE浏览器不支持)
  + （26）insertImage：在插入点插入一张图片（删除选中的部分）。需要一个 URL 字符串作为参数。这个 URL 图片地址至少包含一个字符。空白字符也可以（IE会创建一个链接其值为null）
  + （27）insertOrderedList：在插入点或者选中文字上创建一个有序列表
  + （28）insertUnorderedList：在插入点或者选中文字上创建一个无序列表。
  + （29）insertParagraph：在选择或当前行周围插入一个段落。(IE会在插入点插入一个段落并删除选中的部分.)
  + （30）insertText：在光标插入位置插入文本内容或者覆盖所选的文本内容。
  + （31）italic：在光标插入点开启或关闭斜体字。 (Internet Explorer 使用 EM 标签，而不是 I )
  + （32）justifyCenter：对光标插入位置或者所选内容进行文字居中。
  + （33）justifyFull：对光标插入位置或者所选内容进行文本对齐。
  + （34）justifyLeft：对光标插入位置或者所选内容进行左对齐。
  + （35）justifyRight：对光标插入位置或者所选内容进行右对齐。
  + （36）outdent：对光标插入行或者所选行内容减少缩进量。
  + （37）paste：在光标位置粘贴剪贴板的内容，如果有被选中的内容，会被替换。剪贴板功能必须在 user.js 配置文件中启用。
  + （38）redo：重做被撤销的操作。
  + （39）removeFormat：对所选内容去除所有格式。
  + （40）selectAll：选中编辑区里的全部内容。
  + （41）strikeThrough：在光标插入点开启或关闭删除线。
  + （42）subscript：在光标插入点开启或关闭下角标。
  + （43）superscript：在光标插入点开启或关闭上角标。
  + （44）underline：在光标插入点开启或关闭下划线。
  + （45）undo：撤销最近执行的命令。
  + （46）unlink：去除所选的锚链接的<a>标签
  + （47）useCSS：已经废弃，使用 styleWithCSS 代替。
  + （48）styleWithCSS：用这个取代 useCSS 命令。 参数如预期的那样工作, i.e. true modifies/generates 风格的标记属性, false 生成格式化元素。
+ 2. aShowDefaultUI：一个 Boolean， 是否展示用户界面，一般为 false。Mozilla 没有实现。
+ 3. aValueArgument：一些命令（例如insertImage）需要额外的参数（insertImage需要提供插入image的url），默认为null。
