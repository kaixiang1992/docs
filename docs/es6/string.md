# 字符串和正则表达式

#### BMP字符集 {docsify-ignore}

> `UTF-16`中，前`Math.pow(2,16)`个码位均以16位的编码单元表示。

#### 辅助平面字符 {docsify-ignore}

> 超出`Math.pow(2,16)`个码位的编码单元，属于`某个辅助平面`，由两个编码单元`32`位表示的`辅助平面字符`。

#### coePointAt()方法 {docsify-ignore}

使用`codePoinAt()`方法在字符串中检索一个字符的码位

> 检测字符占用的编码单元数量，调用字符的`codePoinAt()`方法

* false BMP字符集 
* true 非BMP字符集 

```javascript
function is32Bit(c){
    console.log(c.codePointAt(0));
    return c.codePointAt(0) > 0xFFFF;
}
console.log(is32Bit('a')); //TODO: false BMP字符集 90
console.log(is32Bit("𠮷")); //TODO: true 非BMP字符集 134071
```

#### String.formCodePoin()方法 {docsify-ignore}

使用`String.formCodePoin()`方法根据指定的码位生成一个字符

> codePointAt()方法的反向方法

```javascript
function codetoCharacter(num){
    return String.fromCodePoint(num);
}
console.log(codetoCharacter(97)); //TODO: a
console.log(codetoCharacter(134071)); //TODO: 𠮷
```

#### normalize()方法  {docsify-ignore}

> ES6为字符串添加了一个`normalize()`方法，可以提供和`Unicode`的标准化形式

* `normalize()`,[文档地址](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/normalize)，参数如下：
    * `NFC`默认选项
    * `NFD`
    * `NFKC`
    * `NFKD`

```javascript
let values = ['a','anc','abc','ae','eə','be','bee','zee','eez'];
let normalized = values.map(text => text.normalize());
normalized.sort((first,second) => {
    if(first < second){
        return -1;
    }else if(first === second){
        return 0;
    }else{
        return 1;
    }
});
// TODO: ["a", "abc", "ae", "anc", "be", "bee", "eez", "eə", "zee"]
console.log(normalized);
```

对原数组进行排序

```javascript
 let values = ['a','anc','abc','ae','eə','be','bee','zee','eez'];
values.sort((first,second) => {
    let firstnormalized = first.normalize(), secondnormalized second.normalize();
    if(firstnormalized < secondnormalized){
        return -1;
    }else if(firstnormalized == secondnormalized){
        return 0;
    }else{
        return 1;
    }
});
// TODO: ["a", "abc", "ae", "anc", "be", "bee", "eez", "eə", "zee"]
console.log(values);
```