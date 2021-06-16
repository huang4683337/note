```js
class Log {
    print(msg) {
        console.log(msg);
    }
}


const dec = (target, property) => {
    // 保存原来的方法
    const old = target.prototype[property];

    // 重写方法
    target.prototype[property] = msg => {
        console.log('执行 print');

        msg = `{${msg}}`;
        
        // 执行原来的方法
        old(msg)
    }
}

dec(Log, 'print');
const log = new Log();

log.print('hello');
```

