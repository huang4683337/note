## 安装

```shell
$ npm install jest
```





## 使用

**index.js**

```js
const path = require("path");


module.exports = class Test {

    /**
     * 生成测试文件名
     * @param {*} filename  代码文件名 
     */
    getTestFileName(filename) {
        const dirName = path.dirname(filename)
        const baseName = path.basename(filename)
        const extname = path.extname(filename)
        const testName = baseName.replace(extname, `.spec${extname}`)
        return path.format({
            root: dirName + '/__test__/',
            base: testName
        })
    }

}
```



```js
// index.js 同级新建 __test__ 文件夹。  __test__ 下新建 index.spec.js 文件

test('测试文件名生成', () => {
    const src = new (require('../index'))

    const ret = src.getTestFileName('/abc/class.js');

    // 断言执行 /abc/class.js 后，会生成 '/abc/__test__/class.spec.js'
    expect(ret).toBe('/abc/__test__/class.spec.js')
})
```

