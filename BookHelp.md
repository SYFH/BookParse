### 简介
`书籍` 是将网页内容通过模板转换为适合移动设备阅读的功能, 主要服务小说, 漫画等网站.    

### 使用流程

添加模板, 万事第一步    
1. 添加网站对应的模板    
2. 设置其中之一为默认模板, 以便使用搜索书籍功能    

使用搜索添加书籍    
1. 搜索书籍, 选择对应的书籍类型    
2. 添加书籍到列表    
3. 点击书籍进行观看    

当网站对搜索进行了限制, 或搜索结果页变化多端, 也可使用书籍链接直接添加书籍    
1. 寻找可以最大提供书籍信息的链接    
2. 选择对应模板添加书籍    
3. 点击书籍进行观看    

### 模板参数

----    
最小单位, 以下用 `Parse` 代替    
对 html 进行处理所需的参数    
流程为 html -> css -> option(attr[param] -> regex) -> value    
```
// css selector 表达式
"css": ""

// 对 css selector 的结果进行操作
// 可用参数:
// text: 对结果直接获取 text 值
// attr: 使用 attribute key 获取值, key name 为 param 填写的值
// innerHTML: 获取结果对应的 innerHTML 值
// content: 获取结果的 content 值
"function": ""

// 当 function 为 attr 时有效, 当做获取结果 attribute 的 key, param 支持依次查找功能, param 格式为 `key1 > key2`, 多个 key 使用 `>` 分割, 程序会从依次使用 key 取值, 直到可以取出值为止
"param": ""

// 对最终的 value 进行正则表达式来获取一些特殊值
// 只会使用分组正则的第一个结果
"regex": ""
```    

----    
模板基本信息    
```
// 模板名称, 可以重复, 但最好做以区分
"name": "样板",

// 模板作者
"author": "norld",

// 模板版本
"version": "1.0.0",

// 模板对应的网站域名, 必填项, 不可重复, 此值会作为区分模板的关键数据
// 格式为 http(s)://domain, 之后的参数可使用 {host} 获取 host 值
"host": "https://www.qidian.com",

// 网页编码, 必填项
// 默认 utf-8, 目前支持 utf-8, GBK, GB2312 编码
"charset": "utf-8",

// 当网站需要登录时提供, 格式为 key=value;key=value;
"cookies": "SESSION=xxxxxx;USER_ID=xxxxxxx",

// 书籍类型, 仅支持 1: 小说, 2: 漫画
// 必填项, 非支持类型可能会模板解析失败
"kind": 0,
    
// 直接添加书籍的链接
// 当网站的搜索存在限制, 或搜索页过于复杂的情况下, 推荐使用直接添加功能
// 程序会根据用户输入的链接, 查找变量名对应的值, 并保存到变量中
//
// 例子:
// "direct" = "{host}/book/{category_id}/{book_id}.html"
// "input" = "https://www.qidian.com/book/1/666.html"
// 
// 程序处理后将获得变量:
// {host} = "https://www.qidian.com"
// {category_id} = "1"
// {book_id} = "666"
//
// 可用参数:
// {host} : 根据用户输入获取域名参数, 必填
// {category_id} : 根据用户输入获取书籍分类编号参数, 可选
// {book_id} : 根据用户输入获取书籍编号参数, 必填
//
// 部分网站需要 {category_id} 来确定书籍分类, 如果网站只会在目录页使用, 而书籍的详情页不会出现, 则最好使用目录页的网址进行模板化
"direct": "{host}/book/{category_id}/{book_id}.html",
```    

----    
搜索信息    
当网站的搜索存在限制, 或搜索页过于复杂的情况下, 推荐使用直接添加功能, 请完善模板的 `direct` 参数    
```
// 搜索链接, 使用搜索书籍时使用的链接
// 可用参数:
// {key_word} : 关键词, 会被输入的关键词替换
// {page} : 分页, 第几页
"start": "https://www.qidian.com/search?s={key_word}&p={page}",
        
// 搜索结果列表
// css 必须是刚好返回多个结果的表达式, 如 div > table > tr > td, 而不是 div > table
// 后面的 item 将会遍历读取这些结果进行解析
"list": Parse,
        
// 搜索结果总页数
"total": Parse,
        
// 解析搜索结果
"item": {
        
    // 书籍编号, 必填项, 如果其他地方要使用书籍编号作为模板值, 请使用 {book_id} 来代指
    "id": Parse,
            
    // 书籍详情页地址
    "url": Parse,
    
    // 书名, 必填项
    "title": Parse,
    
    // 书籍封面
    "cover": Parse,
    
    // 书籍作者
    "author": Parse,
    
    // 书籍简介
    "brief": Parse,
    
    // 书籍分类
    "category": Parse,
    
    // 书籍分类编号, 如果网站存在此项, 则必填
    // 如果网站的搜索结果, 或书籍详情链接没有此值, 但目录页存在此值, 推荐使用直接添加书籍功能, 此时, 模板 direct 值则以目录页链接作为基本模板
    "categoryID": Parse,
            
    // 更新时间
    "update": Parse,
            
    // 最后更新
    "last": Parse
}
```    

----
详情信息    
只有直接添加书籍时才会使用此参数    
```
// 书籍详情页链接模板, 如果网站中书籍的详情和目录在同一个界面, 则此值可能与书籍目录模板的 start 值相同
"start": "{host}/book/{book_id}.html",

// 书名, 必填项
"title": Parse,

// 书籍封面
"cover": Parse
```    

----
目录信息    
```
// 书籍目录页链接模板, 如果网站中书籍的详情和目录在同一个界面, 则此值可能与书籍详情模板的 start 值相同
// 必填项
"start": "{host}/book/{book_id}.html",

// 获取章节组
// 部分网站的目录会包含卷等信息, 卷信息没有 id 和链接, 只有 title, 此时目录模板 item -> title 的 function 推荐使用 attr, param 使用 content 来同时获取章节名和卷名
"list": Parse,

// 对章节组的结果进行解析
"item": {

    // 章节编号
    // 其他模板中可用 {index_item_id} 来获取章节编号值
    "id": Parse,
    
    // 章节名
    "title": Parse,
    
    // 章节链接
    "url": Parse
}
```    

----
内容信息    
```
// 书籍内容链接
// 可用参数:
// {host} : 模板中 host 值, 域名
// {category_id} : 搜索模板中 item -> categoryID 值, 分类编号
// {book_id} : 书籍编号
// {index_item_id} : 目录模板中 item -> id 值, 目录编号
// {page:0} : 分页, 从 0 开始
// {page:1} : 分页, 从 1 开始
"start": "{host}/book/{category_id}/{book_id}/{index_item_id}.html",

// 对文字类书籍内容解析, 一般为小说
// 一般情况下, function 为 innerHTML, 程序会自动处理文字内容, 如去除多余的空行与空格
"text": Parse,

// 对图片类书籍内容解析, 一般为漫画
// 此值适用于 content -> start 链接只有一张图的情况, 如漫画的每一页都是新的链接
"image": Parse,

// 当前内容含有图片的总数
"total": Parse,

// 对图片类书籍内容解析, 一般为漫画
// 此值适用于 content -> start 链接有多张图的情况, 如漫画的每一话中可以直接可以拿到本话所有图片链接
// 当小说中某一章只有图片时可以使用, 如轻小说中插图章节, 也可以使用此模板
"gallery": Parse
```    

### 注意事项与其他

#### 关于云同步
云同步功能基于 Apple 提供的 iCloud 功能实现, 用户添加的书籍, 模板, 阅读进度都会储存到 iCloud 中, 此数据为私有数据, 只有 iCloud 账户所属用户拥有, 即使是开发者也无法查看.    

云同步数据与 iCloud 账号绑定, 即使更换手机数据也依然存在.    

#### 关于模板
模板只提供了网站数据如何解析的信息, 未对网站进行除浏览外的操作.    

模板分享的二维码, 是使用上面模板信息 json 数据压缩后的二进制, 在通过 base64 编码后的数据生成的, 无其他加密手段.    

#### 关于模板中的变量

| 变量名 | 说明 |
|---|---|
| {host} | 域名 |
| {book_id} | 书籍编号 |
| {category_id} | 书籍分类编号 |
| {index_item_id} | 书籍目录编号 |
| {page} | 分页, search -> start 中使用 |
| {page:0} | 分页, content -> start 中使用 |
| {page:1} | 分页, content -> start 中使用 |

#### 关于不正确模板参数
当模板中的参数不正确时, 轻则出现空白数据, 重则导致 App 崩溃, 此时不必惊慌, 将模板参数修改正确即可.    

#### 关于网络问题
有的网站内容可能加载较慢, 经常出现加载失败的情况, 请查看设备的网络设置是否有问题, 或更换其他更好的网站.    

有的网站会对访问者的 ip 进行限制访问, 请自行配置网络环境.    

