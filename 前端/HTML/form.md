## from

```html
<!--表单具有默认的提交行为，默认是同步的，同步表单提交，浏览器会锁死（转圈），等待服务端的响应结果 -->


<!-- 默认表单提交 -->
<from id='', method='post' action='url'>
  <button type='submit'>提交</button>
</from>

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



