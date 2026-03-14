# unrs

控制对象状态的改变。

```shell
npm install unrs
```

- `new Unrs(target)`
  - `path`  `<Object>` 一个对象，设置数据源
  
- `instance.save()`
  - 数据更改时保存记录

- `instance.undo()`
  - 返回上一条记录

- `instance.redo()`
  - 前进一条记录

- `instance.reset()`
  - 当数据更改尚未保存时，重置为当前保存的状态

- `instance.patch(object)`
  - `path`  `<Object>` 用新数据更改

- `instance.pointIndex`
  - 指向当前记录队列的指针

- `instance.stackLength`
  - 当前保存记录长度

### Example

```ts
import Unrs from 'unrs'
const data = {
  a: 1,
  children: [
    {
      a: 2,
    }
  ]
}
const handler = new Unrs(data)
data.b = 2
data.children.push({ a: 3, })
// data { a: 1, b: 2, children: [{ a: 2, }, { a: 3, }] }
// handler.pointIndex 0 handler.stackLength 0
handler.save()
handler.undo()
// data { a: 1, children: [ { a: 2, }, ], }
// handler.pointIndex 0 handler.stackLength 1
handler.redo()
// data { a: 1, b: 2, children: [{ a: 2, }, { a: 3, }] }
// handler.pointIndex 1 handler.stackLength 1
handler.undo()
// data { a: 1, children: [ { a: 2, }, ], }
// handler.pointIndex 0 handler.stackLength 1
data.d = 5
handler.reset()
// data { a: 1, children: [ { a: 2, }, ], }
// handler.pointIndex 0 handler.stackLength 1

//patch
handler.patch({ a: 1, b: 2, children: [{ c: 3 }] })
// data { a: 1, b: 2, children: [{ c: 3 }] }
// handler.pointIndex 0 handler.stackLength 1
handler.save()
// handler.pointIndex 1 handler.stackLength 1
handler.undo()
// data { a: 1, children: [ { a: 2, }, ], }
// handler.pointIndex 0 handler.stackLength 1
```

## License

[MIT](./LICENSE) License © 2024-PRESENT [muamuamu](https://github.com/muamuamu)
