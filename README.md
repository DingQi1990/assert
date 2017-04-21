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
.
```




# util (实用工具)

`util` 模块主要用于支持 Node.js 内部 API 的需求。 大部分实用工具也可用于应用程序与模块开发者。 它可以通过以下方式使用：

```
const util = require('util');
```

`util.debuglog()` 方法用于创建一个函数，基于 `NODE_DEBUG` 环境变量的存在与否有条件地写入调试信息到 `stderr`。 如果 `section` 名称在环境变量的值中，则返回的函数类似于 [`console.error()`](http://nodejs.cn/api/console.html#console_console_error_data_args)。 否则，返回的函数是一个空操作。

例子：

```
const util = require('util');
const debuglog = util.debuglog('foo');

debuglog('hello from foo [%d]', 123);

```

如果程序在环境中运行时带上 `NODE_DEBUG=foo`，则输出类似如下：

```
FOO 3245: hello from foo [123]

```

其中 `3245` 是进程 id。 如果运行时没带上环境变量集合，则不会打印任何东西。

`NODE_DEBUG` 环境变量中可指定多个由逗号分隔的 `section` 名称。 例如：`NODE_DEBUG=fs,net,tls`。



## util.deprecate(function, string)

`util.deprecate()` 方法会包装给定的 `function` 或类，并标记为废弃的。

```
const util = require('util');

exports.puts = util.deprecate(function() {
  for (var i = 0, len = arguments.length; i < len; ++i) {
    process.stdout.write(arguments[i] + '\n');
  }
}, 'util.puts: 使用 console.log 代替');

```

当被调用时，`util.deprecate()` 会返回一个函数，这个函数会使用 `process.on('warning')` 事件触发一个 `DeprecationWarning`。 默认情况下，警告只在首次被调用时才会被触发并打印到 `stderr`。 警告被触发之后，被包装的 `function` 会被调用。

如果使用了 `--no-deprecation` 或 `--no-warnings` 命令行标记，或 `process.noDeprecation` 属性在首次废弃警告之前被设为 `true`，则 `util.deprecate()` 方法什么也不做。

如果设置了 `--trace-deprecation` 或 `--trace-warnings` 命令行标记，或 `process.traceDeprecation` 属性被设为 `true`，则废弃的函数首次被调用时会把警告与堆栈追踪打印到 `stderr`。

如果设置了 `--throw-deprecation` 命令行标记，或 `process.throwDeprecation` 属性被设为 `true`，则当废弃的函数被调用时会抛出一个异常。

`--throw-deprecation` 命令行标记和 `process.throwDeprecation` 属性优先于 `--trace-deprecation` 和 `process.traceDeprecation`。





## util.format(format[, ...args])

- `format` 一个类似 `printf` 的格式字符串。

`util.format()` 方法返回一个格式化后的字符串，使用第一个参数作为一个类似 `printf` 的格式。

第一个参数是一个字符串，包含零个或多个占位符。 每个占位符会被对应参数转换后的值所替换。 支持的占位符有：

- `%s` - 字符串。
- `%d` - 数值（包括整数和浮点数）。
- `%j` - JSON。如果参数包含循环引用，则用字符串 `'[Circular]'` 替换。
- `%%` - 单个百分号（`'%'`）。不消耗参数。

如果占位符没有对应的参数，则占位符不被替换。

```
util.format('%s:%s', 'foo');
// 返回: 'foo:%s'

```

如果传入 `util.format()` 方法的参数比占位符的数量多，则多出的参数会被强制转换为字符串（对于对象和符号，使用 `util.inspect()`），然后拼接到返回的字符串，参数之间用一个空格分隔。

```
util.format('%s:%s', 'foo', 'bar', 'baz'); // 'foo:bar baz'

```

如果第一个参数不是一个格式字符串，则 `util.format()` 返回一个所有参数用空格分隔并连在一起的字符串。 每个参数都使用 `util.inspect()` 转换为一个字符串。

```
util.format(1, 2, 3); // '1 2 3'
```

