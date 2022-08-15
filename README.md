# Javascript / Typescript代码命名规范



## 1. 通用原则

### 1.1 符合英语语法

> Programs are meant to be read by humans and only incidentally for computers to execute. 
>
> -Donald Knuth

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand.
>
> -Matin Fowler

既然使用英语来命名，英文单词拼写正确和符合英文语法是最基本的。为了方便Ta人阅读和理解，你需要了解：

1.词性（包括名词/复数，动词/分词，形容词，介词，连词，量词）

2.时态



**示例**：

```typescript
// ❌
function completedTranslate(chapterIds: string[]) {
	const chapters = repository.findByChapterIdIn(chapterIds);
  chapters.forEach(chapter => completedTranslate(chapter));
  repository.saveAll(chapters);
}

// ✅
function completeTranslation() {
  ...
}
```



vscode单词拼写检查插件：https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker



### 1.2 一致性

当存在多种可能的表达时，选择一种然后统一

```typescript
// ❌
function getUsers() {}
function fetchBooks() {}
function retriveAuthors() {}

// ✅
function fetchUsers() {}
function fetchBooks() {}
function fetchAuthors() {}
```



### 1.3 精准描述

避免使用 data，info， flag，process，handle，maintain，manage，modify等这些宽泛的单词，使用具体业务上下文名称。

```typescript
// ❌
function fetchData() {}

// ✅
function fetchUsers() {}
```



### 1.4 不要使用技术术语命名

使用技术术语命名是一种基于实现细节的命名方式，而实现细节是容易变化的，所以应该使用业务语言来命名。

```typescript
// ❌
const bookList = ['战争与和平', '巴黎圣母院', '童年'];
const bookSet = new Set(['战争与和平', '巴黎圣母院', '童年']);
const bookMap = new Map([['firstBook', '战争与和平'], ['secondBook', '巴黎圣母院'], , ['thirdBook', '童年']])

// ❎
const books = ['战争与和平', '巴黎圣母院', '童年'];

// ✅
const bookNames = ['战争与和平', '巴黎圣母院', '童年'];
```



### 1.5 描述意图，而非实现细节

命名要对实现的细节进行抽象，而不是对实现细节的陈述

```typescript
// ❌
function processChapter(chapterId: string) {
    const chapter = this.repository.findByChapterId(chapterId); 
    if (chapter == null) { 
      throw new Error("Unknown chapter [" + chapterId + "]"); 
    } 
    chapter.setTranslationState(TranslationState.TRANSLATING);
    this.repository.save(chapter);
}

// ❎
function changeChapterToTranslating(chapterId: string) {
  ...
}

// ✅
function startTranslation(chapterId: string) {
  ...
}
```



### 1.6 使用足够详尽的命名

在不看函数的文档的前提下，通过名称可以尽可能地理解其意图。

```typescript
// ❌
function findBooks(author) {}

// ✅
function findBooksByAuthor(author) {}
```



### 1.7 避免缩写

不要用缩写和自己创造缩写，缩写非常影响可阅读性

```typescript
// ❌
const onItmClk = () => {}

// ✅
const onItemClick = () => {}
```



## 2. 基础原则

### 2.1 常量

**命名规范**：使用全大写的字母和下划线（**UPPER_SNAKE_CASE**）来组合命名，下划线用以分割单词。

**英文语法**：名词或名词短语

**示例**：

```javascript
const DAYS_UNTIL_TOMORROW = 1;

const PI = 3.14;

const MAX_COUNT = 100;
```



### 2.2 变量

#### 普通变量

**命名规范**：小驼峰式

**英文语法**：名词或名词短语



#### 布尔型变量

**命名规范**：小驼峰式

**英文语法**：形容词，动词的过去分词（-ed）和现在进行时（-ing），复合词，系表结构

**示例1**：

```typescript
const visible = true; // 形容词
const enabled = false; // 动词的过去分词
const loading = true; // 动词的现在进行时
const buttonDisabled = false; //复合词中形容词放在最后
```

**示例2**：

| 前缀    | 示例                                         | 中文含义                                   | 等同                                 |
| ------- | -------------------------------------------- | ------------------------------------------ | ------------------------------------ |
| should  | shouldUpdate                                 | 是否应该更新                               |                                      |
| is      | isVisible, isHidden, isScrollable, isLoading | 是否可见，是否隐藏，是否可滚动，是否加载中 | visible, hidden, scrollable, loading |
| has     | hasEncryption                                | 是否有加密                                 |                                      |
| are     | areEqual                                     | 是否相等                                   |                                      |
| can     | canEdit                                      | 是否能编辑                                 | editable                             |
| include |                                              |                                            |                                      |
| contain |                                              |                                            |                                      |
| need    | needUpdate                                   | 是否需要                                   |                                      |



#### 数字型变量

**命名规范**：小驼峰式

**英文语法**：量词

**示例**：

有意义的词语：width, length, count，size等，可以使用 numberOfXXX或者 xxxCount形式

```typescript
// ❌ 名词的复数通常做数组列表
const rows = 100;

// ✅
const rowCount = 1000;
const pageSize = 200;
const numberOfErrors = 5;
```



#### 数组类型变量

**命名规范**：小驼峰式

**英文语法**：名词复数或名词短语复数

**示例**：

```typescript
const fruits = ['banana', 'avocado', 'tomato'];

fruits.forEach((fruit, idx) => {});
```



#### 字典（Map）类型变量

**命名规范**：小驼峰式

**英文语法**：名词复数或名词短语复数

**示例**：

```typescript
const usersByID = {
	id12345: {
    name: 'peter',
    id: 'id12345',
    age: '18'
  }
}
```



### 2.3 函数

函数通常都是执行一个动作，所以函数应该是动词开头（动宾结构），React函数组件是例外，需要用名词。



#### 返回布尔值的函数

**命名规范**：小驼峰式

**英文语法**：系表结构，和布尔型变量类似

**示例**：

```typescript
//1.使用 is, are, has, can, need,include/contain 等 作为前缀

//2.is + 形容词

//3.has + 名词

//4.can + 动词

//5.need + 动词

//6.include / contain + 名词

// value string is ISBN ?
function isISBN(value: string) {
  ...
}

// compare date A and date B
function compareDate(dataA:Date, dateB:Date){
  ...
}

// element contains iframe ?
function containsIframe(element) {
  ...
}
```



#### 返回其它值的函数

**命名规范**：小驼峰式

**英文语法**：动宾结构

**示例**：

```javascript
//1.使用 get, fetch, retrive 作为前缀
//2.+[ ? ] 需要返回的
//3.如果返回的是数组，记得复数
    
function getTemperature() { return temperature }

function getEggs() { return eggs }
```



#### 无返回值得函数

**命名规范**：小驼峰式

**英文语法**：动词或动宾结构

**示例**：

```javascript
function setLanguageModal() {}

function launch(options) {}
```



#### A/HC/LC 通用模式

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

| 示例                   | 前缀     | 动作 (A)  | High context (HC) | Low context (LC) |
| ---------------------- | -------- | --------- | ----------------- | ---------------- |
| `getUser`              |          | `get`     | `User`            |                  |
| `getUserMessages`      |          | `get`     | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`  | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display` | `Message`         |                  |



### 1.4 复杂的函数

1.识别所有任务（单一职责，可测试）

2.分别为任务命名

3.创建子函数

4.总结出最终函数的名称



### 1.5 枚举

**命名规范**：大驼峰式，枚举值使用大驼峰 或 UPPER_SNAKE_CASE

**英文语法**：名词，形容词，名词短语

**示例**：

```typescript
enum UpperCamelCase { 
  CAPITALIZED_NAMES_WITH_UNDERSCORES = 0 
}

// 枚举值为名词
enum UserResponse {
  No,
  Yes
}

// 枚举值为形容词
enum Status { 
	Loading,
  Succeeded,
  Failed
}
```

 

### 1.6 类

**大驼峰式**：大驼峰式

**英文语法**：名词或名词短语

**示例**：

```typescript
class Point { }
```



#### 类的对象

**大驼峰式**：大驼峰式

**英文语法**：名词或名词短语（口诀：美小圆旧黄，法国木书房。意思是定语顺序为：美丑-大小-形状-新旧-颜色-国家-材质-用途-主体。）

**示例1**：

```typescript
// ❌
const headerFancyVideo = new Video();
// ✅
const fancyHeaderVideo = new Video();
```

**示例2**：

```typescript
// 后置定语，放在后面
const usersWithPosts = [
  {
    id: 'userId',
    name: 'userName',
    posts: []
  }
]
```



#### 类成员避免上下文冗余

```
class MenuItem {
	// ❌
  handleMenuItemClick = (event) => { ... }
  // ✅
  handleClick = (event) => { ... }
}
```



### 1.7 接口

**大驼峰式**：大驼峰式

**英文语法**：名词或名词短语，形容词

**示例**：

```typescript
interface Thenable { }

interface State { }
```



#### 不适用 `I` 作为接口名的前缀

类和接口都是某种意义上的抽象和封装，继承时不需要关心它是一个接口还是一个类。如果用`I`前缀，当一个变量的类型更改了，比如由接口变成了类，那变量名称就必须同步更改。

```typescript
// ❌
interface IState {
  name: string;
  age: number;
}

// ✅
interface State {
  name: string;
  age: number;
}
```





## 3. 名词表

DDD领域统一语言或者业务词汇表



## 4. 动词表

get 获取/         set 设置,           add 增加/          remove 删除

create 创建/      destory 移除         start 启动/        stop 停止

open 打开/       close 关闭,           read 读取/        write 写入

load 载入/        save 保存,           create 创建/       destroy 销毁

begin 开始/       end 结束,            backup 备份/      restore 恢复

import 导入/      export 导出,          split 分割/        merge 合并

inject 注入/       extract 提取,          attach 附着/      detach 脱离

bind 绑定/        separate 分离,        view 查看/        browse 浏览

edit 编辑/        modify 修改,         select 选取/       mark 标记

copy 复制/       paste 粘贴,          undo 撤销/        redo 重做

insert 插入/       delete 移除,         add 加入/         append 添加

clean 清理/       clear 清除,          index 索引/        sort 排序

find 查找/        search 搜索,         increase 增加/      decrease 减少

play 播放/        pause 暂停,         launch 启动/        run 运行

compile 编译/     execute 执行,        debug 调试/        trace 跟踪

observe 观察/    listen 监听,          build 构建/         publish 发布

input 输入/       output 输出,         encode 编码/        decode 解码

encrypt 加密/     decrypt 解密,         compress 压缩/     decompress 解压缩

pack 打包/       unpack 解包,         parse 解析/         emit 生成

connect 连接/    disconnect 断开,      send 发送/         receive 接收

download 下载/   upload 上传,         refresh 刷新/        synchronize 同步

update 更新/     revert 复原,         lock 锁定/          unlock 解锁

check out 签出/   check in 签入,       submit 提交/        commit 交付

push 推/         pull 拉,            expand 展开/        collapse 折叠

begin 起始/      end 结束,          start 开始/          finish 完成

enter 进入/       exit 退出,          abort 放弃/          quit 离开

obsolete 废弃/    depreciate 废旧,     collect 收集/        aggregate 聚集

reset 重置/



## 5. 命名翻译网站：

https://unbug.github.io/codelf

https://fanyi.phpstudyhelper.com/



## 6. 资料参考：

https://javascript.plainenglish.io/the-ultimate-guide-to-javascript-naming-conventions-f3e371efb0d1

https://javascript.plainenglish.io/javascript-naming-convention-best-practices-b2065694b7d

https://blog.frankmtaylor.com/2021/10/21/a-small-guide-for-naming-stuff-in-front-end-code/#naming-things-in-javascript

https://github.com/kettanaito/naming-cheatsheet

https://www.jianshu.com/p/3ecdb9c66a2a

https://github.com/kettanaito/naming-cheatsheet

https://www.freecodecamp.org/news/javascript-naming-conventions-dos-and-don-ts-99c0e2fdd78a/

https://guoyunhe.me/2021/03/07/programming-english-basics/

https://hackernoon.com/software-complexity-naming-6e02e7e6c8cb
