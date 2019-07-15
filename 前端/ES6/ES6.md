# ES6

### 1、find

```javascript
var list = [
  {name:'name1',id:1},
  {name:'name2',id:2},
  {name:'name3',id:3},
]

//接受一个函数作为参数
var itemData = list.find( (item)=>{
  return item.id === 2;  // 当某个遍历项符合条件时 终止遍历 且返回对应的项，否则返回undefined
})

console.log(itemData); //{name: "name2", id: 2}
```



### 2、findIndex

```javascript
var list = [
  {name:'name1',id:1},
  {name:'name2',id:2},
  {name:'name3',id:3},
]

//接受一个函数作为参数
var itemIndex = list.findIndex( (value, index)=>{
  return value.id === 2;  // 当某个遍历项符合条件时 终止遍历 且返回对应的项，否则返回undefined
})

console.log(itemIndex); // 1
```

