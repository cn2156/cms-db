# CMS 数据

## 数据服务

数据使用 json 保存，依靠 [json-server](https://www.npmjs.com/package/json-server) 提供服务：

- 本地：
  - 安装：`npm install`
  - 运行：`npm run db`
- 全局：
  - 安装：`npm install -g json-server`
  - 运行：`json-server --watch db.json`
    - 本例：`json-server --port 3301 --watch db.json`

## 数据结构

状态说明：

- 管理类：
  - 未审核 0 not_reviewed
  - 已启用 1 activated
  - 已禁用 2 disabled
  - 已删除 3 deleted
- 文章类：
  - 草稿 0 draft
  - 已审核 1 audited
  - 已发布 2 published
  - 未发布 3 unpublished
  - 已删除 4 deleted

### 用户 user

| 字段          | 含义             |
| ------------- | ---------------- |
| id            | ID               |
| phone         | 手机号           |
| pwd           | 密码             |
| name          | 姓名             |
| avatar        | 头像             |
| allow_backend | 是否允许登录后台 |
| source        | 来源             |
| status        | 状态             |

### 角色 role

| 字段   | 含义     |
| ------ | -------- |
| id     | ID       |
| name   | 角色名称 |
| code   | 角色编码 |
| status | 状态     |

### 资源 resource

| 字段          | 含义     |
| ------------- | -------- |
| id            | ID       |
| name          | 资源名称 |
| code          | 资源编码 |
| category_id   | 栏目 ID  |
| auth_add      | 添加权限 |
| auth_update   | 更新权限 |
| auth_retrieve | 查看权限 |
| auth_remove   | 删除权限 |
| auth_check    | 审核权限 |
| status        | 状态     |

### 用户角色 user_role

| 字段    | 含义    |
| ------- | ------- |
| id      | ID      |
| user_id | 用户 ID |
| role_id | 角色 ID |

### 角色资源 role_resource

| 字段        | 含义    |
| ----------- | ------- |
| id          | ID      |
| role_id     | 角色 ID |
| resource_id | 资源 ID |

### 栏目 category

| 字段      | 含义        |
| --------- | ----------- |
| id        | ID          |
| parent_id | 所属栏目 ID |
| name      | 栏目名称    |
| code      | 栏目编码    |
| cover_img | 栏目图片    |
| desc      | 栏目简介    |
| status    | 状态        |

### 内容 post

| 字段           | 含义        |
| -------------- | ----------- |
| id             | ID          |
| category_id    | 栏目 ID     |
| title          | 标题        |
| keywords       | 关键字      |
| author         | 作者        |
| source         | 来源        |
| post_time      | 发布时间    |
| cover_img      | 封面图片    |
| desc           | 简介        |
| content        | 内容        |
| read_count     | 查看次数    |
| favorite_count | 收藏数量    |
| likes_count    | 点赞数量    |
| comment_count  | 评论数量    |
| created_time   | 添加时间    |
| create_user_id | 添加用户 ID |
| update_time    | 更新时间    |
| update_user_id | 更新用户 ID |
| check_time     | 审核时间    |
| check_user_id  | 审核用户 ID |
| status         | 状态        |

### 收藏 favorite

| 字段          | 含义     |
| ------------- | -------- |
| id            | ID       |
| user_id       | 用户 ID  |
| post_id       | 内容 ID  |
| favorite_time | 收藏时间 |

### 点赞 likes

| 字段       | 含义     |
| ---------- | -------- |
| id         | ID       |
| user_id    | 用户 ID  |
| post_id    | 内容 ID  |
| likes_time | 点赞时间 |

### 评论 comment

| 字段          | 含义        |
| ------------- | ----------- |
| id            | ID          |
| user_id       | 用户 ID     |
| post_id       | 内容 ID     |
| content       | 评论内容    |
| created_time  | 添加时间    |
| update_time   | 更新时间    |
| check_time    | 审核时间    |
| check_user_id | 审核用户 ID |
| status        | 状态        |

## RESTful

来自[restfulapi.cn](https://restfulapi.cn/)上的介绍：

- REST：是 Representational State Transfer 的缩写，如果一个架构符合 REST 原则，就称它为 RESTful 架构
- RESTful 架构可以充分的利用 HTTP 协议的各种功能，是 HTTP 协议的最佳实践
- RESTful API 是一种软件架构风格、设计风格，可以让软件更加清晰，更简洁，更有层次，可维护性更好

API 请求设计：

- 请求 = 动词 + 宾语
  - 动词：使用五种 HTTP 方法，对应 CRUD 操作。
  - 宾语：URL 应该全部使用名词复数，可以有例外，比如搜索可以使用更加直观的 search 。
  - 过滤信息（Filtering）：如果记录数量很多，API 应该提供参数，过滤返回结果
    - 如：`?limit=10`指定返回记录的数量，`?offset=10`指定返回记录的开始位置
- 示例：
  - GET：/zoos 列出所有动物园
  - POST：/zoos 新建一个动物园
  - GET：/zoos/:id 获取某个指定动物园的信息
  - PUT：/zoos/:id 更新某个指定动物园的全部信息
  - PATCH：/zoos/:id 更新某个指定动物园的部分信息
  - DELETE：/zoos/:id 删除某个动物园
  - GET：/zoos/:id/animals 列出某个指定动物园的所有动物
  - DELETE：/zoos/:id/animals/:id 删除某个指定动物园的指定动物

API 响应设计：

- 使用 HTTP 的状态码
  - 1xx 相关信息
  - 2xx 操作成功
  - 3xx 重定向
  - 4xx 客户端错误
  - 5xx 服务器错误
- 服务器回应数据
  - 客户端请求时，要明确告诉服务器，接受 JSON 格式，请求的 HTTP 头的 ACCEPT 属性要设成 application/json
  - 服务端返回的数据，不应该是纯文本，而应该是一个 JSON 对象。服务器回应的 HTTP 头的 Content-Type 属性要设为 application/json
  - 错误处理 如果状态码是 4xx，就应该向用户返回出错信息。一般来说，返回的信息中将 error 作为键名，出错信息作为键值即可。 {error: "Invalid API key"}
  - 认证 RESTful API 应该是无状态，每个请求应该带有一些认证凭证。推荐使用 JWT 认证，并且使用 SSL
  - Hypermedia 即返回结果中提供链接，连向其他 API 方法，使得用户不查文档，也知道下一步应该做什么

## json-server 路由

路由：

- Plural routes
  - GET `/posts`
  - GET `/posts/1`
  - POST `/posts`
  - PUT `/posts/1`
  - PATCH `/posts/1`
  - DELETE `/posts/1`
- Singular routes
  - GET `/profile`
  - POST `/profile`
  - PUT `/profile`
  - PATCH `/profile`
- Filter
  - Use `.` to access deep properties
    - GET `/posts?title=json-server&author=typicode`
    - GET `/posts?id=1&id=2`
    - GET `/comments?author.name=typicode`
- Paginate
  - Use `_page` and optionally `_limit` to paginate returned data.
    - GET `/posts?_page=7`
    - GET `/posts?_page=7&_limit=20`
  - 10 items are returned by default
- Sort
  - Add `_sort` and `_order` (ascending order by default)
    - GET `/posts?_sort=views&_order=asc`
    - GET `/posts/1/comments?_sort=votes&_order=asc`
  - For multiple fields, use the following format:
    - GET `/posts?_sort=user,views&_order=desc,asc`
- Slice
  - Add \_start and \_end or \_limit (an X-Total-Count header is included in - the response)
    - GET `/posts?_start=20&_end=30`
    - GET `/posts/1/comments?_start=20&_end=30`
    - GET `/posts/1/comments?_start=20&_limit=10`
  - Works exactly as Array.slice (i.e. \_start is inclusive and \_end exclusive)
- Operators
  - Add `_gte` or `_lte` for getting a range
    - GET `/posts?views_gte=10&views_lte=20`
  - Add \_ne to exclude a value
    - GET `/posts?id_ne=1`
  - Add `_like` to filter (RegExp supported)
    - GET `/posts?title_like=server`
- Full-text search
  - Add `q`
    - GET `/posts?q=internet`
- Relationships
  - To include children resources, add `_embed`
    - GET `/posts?_embed=comments`
    - GET `/posts/1?_embed=comments`
  - To include parent resource, add `_expand`
    - GET `/comments?_expand=post`
    - GET `/comments/1?_expand=post`
  - To get or create nested resources (by default one level, add custom routes - for more)
    - GET `/posts/1/comments`
    - POST `/posts/1/comments`
