---
title: JavaScript 异步编程：从回调到 async/await
date: 2026-03-15 14:00:00
categories:
  - 前端
tags:
  - JavaScript
  - 异步编程
  - Promise
cover: https://cdn.jsdelivr.net/gh/jerryc127/butterfly_cdn@2.1.0/top_img/cover3.webp
---

JavaScript 的异步编程经历了从回调地狱到 Promise，再到 async/await 的演进。本文梳理这三种方式的差异和适用场景。

<!-- more -->

## 1. 回调函数（Callback）

最早的异步处理方式，简单但容易导致"回调地狱"。

```javascript
// 示例：读取文件后处理数据
fs.readFile('data.json', 'utf8', function(err, data) {
  if (err) {
    console.error('读取失败:', err);
    return;
  }
  
  processData(data, function(result) {
    saveResult(result, function(err) {
      if (err) {
        console.error('保存失败:', err);
      }
      console.log('完成！');
    });
  });
});
```

**问题**：多层嵌套导致代码难以维护，错误处理分散。

## 2. Promise

Promise 让异步代码可以链式调用，告别嵌套地狱。

```javascript
fetch('/api/user')
  .then(response => response.json())
  .then(user => {
    console.log('用户信息:', user);
    return fetch(`/api/posts?userId=${user.id}`);
  })
  .then(response => response.json())
  .then(posts => {
    console.log('文章列表:', posts);
  })
  .catch(err => {
    console.error('请求失败:', err);
  });
```

**Promise 状态**：
- `pending`：初始状态
- `fulfilled`：操作成功
- `rejected`：操作失败

### 常用 API

```javascript
// 并发执行，全部成功才继续
Promise.all([fetch('/api/a'), fetch('/api/b')])
  .then(([resA, resB]) => { /* 全部完成 */ });

// 并发执行，取最快的结果
Promise.race([fastRequest(), slowRequest()])
  .then(result => { /* 最先完成的结果 */ });

// 并发执行，全部结束（不论成败）
Promise.allSettled([req1, req2])
  .then(results => results.forEach(r => console.log(r.status)));
```

## 3. async / await

语法糖，让异步代码看起来像同步代码，可读性最佳。

```javascript
async function loadUserPosts(userId) {
  try {
    const response = await fetch(`/api/user/${userId}`);
    const user = await response.json();
    
    const postsRes = await fetch(`/api/posts?userId=${user.id}`);
    const posts = await postsRes.json();
    
    return { user, posts };
  } catch (err) {
    console.error('加载失败:', err);
    throw err;
  }
}

// 调用
const { user, posts } = await loadUserPosts(123);
```

### 并发陷阱

```javascript
// ❌ 顺序执行，慢！
const a = await fetchA(); // 等 fetchA 完成
const b = await fetchB(); // 再等 fetchB 完成

// ✅ 并发执行，快！
const [a, b] = await Promise.all([fetchA(), fetchB()]);
```

## 选择建议

| 场景 | 推荐方式 |
|------|----------|
| 简单单次异步 | `async/await` |
| 链式操作 | `Promise.then` 或 `async/await` |
| 并发请求 | `Promise.all` |
| 旧代码 / 回调库 | 封装成 Promise |

> **原则**：始终使用 `async/await`，需要并发时配合 `Promise.all`。

---

掌握这三种模式，JavaScript 异步编程就没有什么难题了。
