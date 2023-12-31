# 浏览器本地存储

## 简介

使用浏览器本地存储，保存离线模式下的小说文本，已经用户自定义设置的阅读器样式，文件夹树结构等。

## 实现细节

### 技术选型

使用 localStorage 实现浏览器本地存储。项目中使用第三方库 localforage 封装的 API，存储空间更大，存储数据结构更灵活，不需要直接操作底层的 API。

### 模块实现

增加存储模块，包括获取和设置存储的函数，存储字段的 keys，代码如下

```js
// src/utils/store.js
import localforage from "localforage";

// 注意 异步函数获取本地存储
async function getLocalValue(key) {
  return await localforage.getItem(key);
}

function setLocalValue(key, value) {
  localforage.setItem(key, value);
}

export const NOVEL_READER_STYLE_SAVE_KEY = "novel-reader-style";
export const LOCAL_NOVELS_SAVE_KEY = "local-novels";
export const FOLDER_TREE_SAVE_KEY = "folder-tree";

export { getLocalValue, setLocalValue };
```

其中不同存储字段的 key 和对应的 value 含义如下

| key                         | 中文含义                 | value                            |
| --------------------------- | ------------------------ | -------------------------------- |
| NOVEL_READER_STYLE_SAVE_KEY | 用户设置的页面的样式配置 | '{"opacity":0.8,"width":"20px"}' |
| LOCAL_NOVELS_SAVE_KEY       | 离线模式下小说的存储     | todo                             |
| FOLDER_TREE_SAVE_KEY        | 用户设置的目录结构       | todo                             |

### 接口设计与实现

系统存储模块，对外暴露两个存储函数 getLocalValue, setLocalValue，以及存储字段 keys。

系统功能界面，调用存储函数设置或获取本地存储。

```js
// App.js
import {
  getLocalValue,
  setLocalValue,
  NOVEL_READER_STYLE_SAVE_KEY,
} from "./utils/store";

// 1、界面初始化，从本地存储获取配置；如果没有配置，使用默认配置；
initDataFromLocalStore = () => {
  getLocalValue(NOVEL_READER_STYLE_SAVE_KEY).then((localStyleStr) => {
    if (localStyleStr) {
      this.setState({
        style: JSON.parse(localStyleStr) || DEFAULT_STYLE,
      });
    }
  });
};

// 2、用户实现增删后，保存样式到本地存储
setLocalValue(NOVEL_READER_STYLE_SAVE_KEY, JSON.stringify(style));
```

### 单元测试

单元测试，包括对各个模块进行的单元测试和测试结果等。

因为 localStorage 是浏览器中的存储，单元测试基于 node 环境实现，所以暂不支持单元测试。

附：node 环境服务端存储可以考虑 node-localstorage 实现。

## 版本历史

1.0 版本，支持浏览器原生的 localStorage 存储阅读器样式。

1.3 版本，支持第三方库 localforage 存储阅读器样式。

未来，使用第三方库 localforage 存储本地小说等数据。

## 参考文献

- https://www.npmjs.com/package/localforage
