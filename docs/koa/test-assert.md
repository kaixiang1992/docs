# 单元测试

使用Node自带的:book:[assert](https://www.npmjs.com/package/assert)模块，:clock1030:`2019/05/18 16:30`

1. equal 检查变量的值

```javascript
const assert = require("assert");
assert.equal(arr.length, 1, '第一项已存在'); //TODO: 断言数据添加成功
assert.equal(arr.length, 0, 'array 已清空'); //TODO: 断言清空数组成功
```

2. notEqual 找出逻辑中的问题

```javascript
const assert = require("assert");
let arr = [1, 2, 3];
arr.length = 0;
arr.push(1);
assert.notEqual(arr.length, 0, '第一项已存在'); //TODO: 断言数组是否存着数据
```

3. ok 测试异步值是否为true

```javascript
const assert = require("assert");

handler.doAsync(value => {
    assert.ok(value, 'callback shuould be passed true'); //TODO: 断言值为true，否则抛出异常
    /*
    * @description do something
    */
});
```

4. 能否正确抛出错误

```javascript
const assert = require("assert");
const todo = {
    todolist: [],
    add(item){
        this.todolist.push(item);
    }   
}

assert.throws(todo.add, /requires/); //TODO: 未传递item参数的add调用
```

:arrow_down:更多操作

```text
strictEqual  使用严格相等操作符 ===
notStictEqual 使用严格相等操作符 !===
deepEqual 和 notDeepEqual 层层深入对两个对象进行比较
```