## from表单提交

==表单具有默认的提交行为，默认是同步的，同步表单提交，浏览器会锁死（转圈），等待服务端的响应结果==

### 默认表单提交

```html
<!-- 默认表单提交 -->

<from id='', method='post' action='url'>
  <button type='submit'>提交</button>
</from>
```

### 异步（ajax）表单提交

```html
<!-- 异步（ajax）表单提交 -->

<from id='form1'>
  <button type='submit'>提交</button>
</from>

<script>
$('#form1').on('submit', function(e){
  e.preventDefault(); //阻止浏览器默认事件
  var formData = $(this).serialize(); // 表单序列化
  $.ajax({
    url:'/url',
    type:"formData",
    dataType:"json",
    success:function(data){
      console.log(data)
    }
  })
})
</script>
```



## 阻止表单默认提交

+ type 为 submit

  + 改变标签类型

    将`<input>`标签内按钮类型从`type="submit"`修改为`type="button"`

    ```html
    <input type='button'></input>
    ```

    表单内的`<button>`未指定类型时，默认的类型为submit，可以显式的修改为`<button type="button">`来阻止表单提交

    ```html
    <button type="button">
    ```

+ Type 不为 submit

  + 利用`preventDefault()`方法

    ```html
    <form action="">
    		<input type="submit" value="button" onclick="btn(event)" /> 
    </form>
    
    <script>
      function btn(event){
    		event.preventDefault();
    	}
    </script>
    ```

  + 用`onclick`点击事件来`return false `

    ```html
    <form action="">
    			<input type="submit" value="button" onclick="return btn()" /> 
      		<!--注意是onclick内是return func();而不是简单的调用func()函数-->
    </form>
    
    
    <script>
    function btn(){
      return false;
    }
    </script>
    ```

  + 利用**表单**的`onsubmit`事件 

    ```html
    <!-- 
    onsubmit事件 作用于 <form>, 所以 onsubmit 事件加在按钮上是没效果的
    -->
    
    <form action="" onsubmit="return btn()">
        <input type="submit" value="button" /> 
    </form>
    
    <script>
    function btn(){
      return false;
    }
    </script>
    ```

    

  

