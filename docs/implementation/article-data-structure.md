# 文章数据存储

## 存储一致性

简单明确的数据存储，鲁棒性更高。

- 数据库：内部的数据结构一致（数据库写入或者更改时，必须满足规范的数据结构）
- 前端：根节点存储的 files 数据结构一致（根节点都经过 Modal 处理，返回满足的文档类型，不同使用场景可以转换数据结构，但是不能修改根节点的文章数据结构）

## 数据来源

1、软件初始化，内置预览的文章（可控）

2、在线模式，从数据库下载的文章（可控）

3、离线模式，用户手动上传的文章（不可控，前后端需要规范）

所有的操作都是通过 model 规范化数据结构即可，如下

```javascript
const generatorDefaultData = () => {
  return {
    files: [
      {
        id: '7hts',
        name: 'text.md',
        detail: 'this is test file.this is test file.this is test file.this is test file.',
        brief: 'this is test file.',
        author: 'Mike',
        size: 300,
        tag: '历史 推断',
        cover_photo: 'https://bookcover.yuewen.com/qdbimg/349573/1037135031/90.webp',
        created_at: '',
        price: 100,
      }
    ]
  };
};
```

- id：文章标识（唯一）
- name: 文章标题（包括后缀名）
- detail: 文章全部内容
- brief：文章概要
- author：文章作者，可选
- size：文章字数（全文字符串的个数）
- tag: 文章标签，可选
- cover_photo: 文章封面图的图片链接，可选
- created_at：文章创建时间，自动生成
- price：文章价格，可选



## 数据展示

渲染文章时，根据不同类型的文章，渲染不同的阅读器

- markdown 格式的文档，不支持分章节，直接显示全部内容
- 代码格式的文档，默认按照代码解析器处理
- txt 格式的文档，根据文档的长度，支持分章节，具体分章节部分，在渲染部分完成



## 历史版本管理（TODO）

需求：实际软件开发中，由于功能更迭，某些数据对象确实需要增减参数，那么已有的历史数据也需要正常处理。

前端可以使用版本管理方式，处理不同版本下的数据结构，后期可扩展性更高（符合实际需求），伪代码如下。

~~~js
// 规范化文章内容
normalizeFile(file) {
  // 缺少文章或者缺少版本号，去掉这个数据
  if (!file || !file['version']) {
    file = null;
  }
  // 版本1的文章，使用版本1的函数处理
  else if (file['version'] === 1) {
    file = convert1(file);
  }
  // 版本2的文章，使用版本2的函数处理
  else if (file['version'] === 2) {
    file = convert2(file);
  }
  else {
    file = null;
  }
  return file;
}
~~~

