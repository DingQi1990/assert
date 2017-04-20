# assert

```
const assert = require('assert');

//assert 模块提供了一组简单的断言测试集合，可被用于测试不变量

// assert(true);
// // 通过
// assert(1);
// // 通过
// assert(false);
// // 抛出 "AssertionError: false == true"
// assert(0);
// // 抛出 "AssertionError: 0 == true"
// assert(false, '不是真值');
// // 抛出 "AssertionError: 不是真值"



//assert.deepEqual

const obj1 = {
    a : {
        b : 1
    }
};
const obj2 = {
    a : {
        b : 2
    }
};
const obj3 = {
    a : {
        b : 1
    }
};
const obj4 = Object.create(obj1);

// assert.deepEqual(obj1, obj1);
// 通过，对象与自身相等

// assert.deepEqual(obj1, obj2);
// 抛出 AssertionError: { a: { b: 1 } } deepEqual { a: { b: 2 } }
// b 的值不同

// assert.deepEqual(obj1, obj3);
// // 通过，两个对象相等
//
// assert.deepEqual(obj1, obj4);
// 抛出 AssertionError: { a: { b: 1 } } deepEqual {}
// 原型会被忽略



//assert.deepStrictEqual(actual, expected[, message])

//大多数情况下与 assert.deepEqual() 一样，但有两个例外。 首先，原始值使用全等运算符（===）比较。 其次，对象的比较包括检查它们的原型是否全等。

assert.deepEqual({a:1}, {a:'1'});
// 通过，因为 1 == '1'

assert.deepStrictEqual({a:1}, {a:'1'});
// 抛出 AssertionError: { a: 1 } deepStrictEqual { a: '1' }
// 因为 1 !== '1' 使用全等运算符





//assert.notEqual(actual, expected[, message])
// 使用不等运算符（!=）测试是否不相等。 notEqual 为不等 1,2 不相等 所以通过  1,1 相等所以报错~
assert.notEqual(1, 2);
// 通过

assert.notEqual(1, 1);
// 抛出 AssertionError: 1 != 1

assert.notEqual(1, '1');
// 抛出 AssertionError: 1 != '1'

```