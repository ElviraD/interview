# 手写new、call、bind、apply方法

###

```js
function newOperator(func, ...args){
    if(typeof func !== 'function'){
        throw new Error('Type Error')
    }
    let newObj = Object.create(func.prototype)
    let result  = func.apply(newObj, args)
    return result instanceof Object ? result : newObj
}
```

## call

```javascript
Function.prototype.myCall =  function (context){
    // 判断类型
    if(typeof this !== 'function'){
        throw new Error('Type Error')
    }
    // 获取参数
    let args = [...arguments].slice(1)
    let result = null
    // 设置对象
    context = context|| window
    // 调用方法this为context的属性
    context.fn = this
    // 执行方法
    result = context.fn(args)
    // 删除属性
    delete context.fn
    return result
}
```

## apply

```js
Function.prototype.myApply =  function (context){
    // 判断类型
    if(typeof this !== 'function'){
        throw new Error('Type Error')
    }
    let result = null
    // 设置对象
    context = context|| window
    const fnSymbol = Symbol()
    // 调用方法this为context的属性
    context[fnSymbol] = this
    // 执行方法
    if(arguments[1]){
        result = context[fnSymbol](arguments[1])
    } else {
        result = context[fnSymbol]()
    }
    // 删除属性
    delete context[fnSymbol]
    return result
}

```

## bind

```js
Function.prototype.myBind =  function (context){
    // 判断类型
    if(typeof this !== 'function'){
        throw new Error('Type Error')
    }
    let result = null
    // 设置对象
    context = context|| window
    const args = [...arguments].slice(1)
    return funtion fn(){
        return fn.apply(this instanceof Fn ? this : context, args.concat(...arguments))
    }
}
```