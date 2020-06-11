# 前端业务中常见的异步场景处理
## 背景
随着前端应用的复杂度提升，应用中的异步场景也越来越多。虽然 ES6 中 Promise、generator、async/await 语法能简化异步代码的编写，但是一些业务场景下还是需要花点心思去处理。

比如：

* 异步循环：需要循环拉取分页数据，直至数据为空。
* 异步取消：组件销毁后，异步才完成，需要避免更新组件的操作。
* 后续异步发生，忽略前面未完成异步：同一接口携带不同参数多次请求的竞态问题，先发后至，后发先至。
* 异步未完成时，后续相同异步等待并一起完成：同一幂等接口多次请求。
* 异步未完成时，后续异步等待：异步队列。

## 异步循环

```
async function requestAll() {
    let hasNext = true;

    while (hasNext) {
        const res = await axios(...);
        
        hasNext = res.hasNext;
    }
}

requestAll();
```
