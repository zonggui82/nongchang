# 生态农业认养商城系统开发文档

**技术栈：PHP + MySQL + 微信小程序**
**适用场景：农产品商城、农业认养、视频监控、礼品卡、乡村文旅、会员订单管理**
**已取消模块：土地种植 / 租地种植模块**

---

## 1. 文档说明

本文档基于当前小程序首页界面，重新整理为一套完整的开发文档。系统定位为“生态农业认养商城平台”，主要面向农产品销售、农业认养、视频监控、礼品卡、乡村文旅等业务场景。

本版本明确取消“土地种植 / 租地种植”相关功能，不再包含土地租赁、地块管理、农事地块记录、我的土地等内容。

---

## 2. 项目概述

### 2.1 项目名称

生态农业认养商城系统

### 2.2 项目类型

微信小程序 + PHP 后台接口 + MySQL 数据库 + PC 管理后台

### 2.3 项目目标

1. 建设一个可上线运营的生态农业商城小程序。
2. 支持农产品在线销售、购物车、订单、支付、物流、售后。
3. 支持农业认养业务，包括动物认养、果树认养、农作物认养。
4. 支持视频监控，让用户查看农场、养殖区、认养对象等实时画面。
5. 支持礼品卡购买、绑定、赠送和订单抵扣。
6. 支持乡村文旅项目展示、预约、支付、核销。
7. 支持后台统一管理商品、订单、认养、用户、视频、礼品卡、文旅项目和营销活动。

### 2.4 建设端口

| 端口          | 说明                         |
| ----------- | -------------------------- |
| 微信小程序用户端    | 面向普通用户，提供浏览、购买、认养、预约、支付等功能 |
| PHP API 服务端 | 为小程序和后台提供统一接口服务            |
| PC 管理后台     | 面向平台运营人员、客服、农场人员、管理员       |
| MySQL 数据库   | 存储用户、商品、订单、认养、支付、物流等业务数据   |

---

## 3. 技术架构

## 3.1 总体架构

```text
微信小程序用户端
      │
      │ HTTPS / JSON API
      ▼
PHP 后端接口服务
      │
      ├── MySQL 数据库
      ├── 微信登录接口
      ├── 微信支付接口
      ├── 文件存储服务
      ├── 视频监控播放地址服务
      └── 短信 / 订阅消息 / 物流接口

PC 管理后台
      │
      │ HTTPS / JSON API
      ▼
PHP 后端接口服务
```

## 3.2 推荐技术选型

| 层级     | 技术建议                                     |
| ------ | ---------------------------------------- |
| 小程序端   | 原生微信小程序 / uni-app 小程序模式                  |
| 后端语言   | PHP                                      |
| 后端框架   | Laravel / ThinkPHP / 原生 PHP 分层架构         |
| 数据库    | MySQL，兼容 MySQL 5.6 及以上版本                 |
| Web 服务 | Nginx 或 Apache                           |
| 接口格式   | RESTful API + JSON                       |
| 支付     | 微信支付                                     |
| 登录     | 微信小程序授权登录                                |
| 文件存储   | 本地存储 / 阿里云 OSS / 腾讯云 COS                 |
| 缓存     | Redis，可选                                 |
| 定时任务   | Linux Crontab                            |
| 后台前端   | Vue / Layui / Bootstrap / Ant Design Vue |

## 3.3 部署建议

### 基础部署

| 服务     | 说明                |
| ------ | ----------------- |
| 小程序    | 发布到微信小程序平台        |
| 后端 API | 部署在云服务器           |
| 管理后台   | 与 API 同服务器或独立部署   |
| 数据库    | MySQL 独立实例或同服务器部署 |
| 图片文件   | 初期可本地存储，后期建议对象存储  |
| HTTPS  | 必须配置 HTTPS 证书     |

### 目录建议

```text
/project
├── api/                    # PHP 接口服务
│   ├── controller/          # 控制器
│   ├── service/             # 业务服务层
│   ├── model/               # 数据模型层
│   ├── middleware/          # 中间件
│   ├── common/              # 公共方法
│   ├── config/              # 配置文件
│   └── public/              # 入口文件
│
├── admin/                  # PC 管理后台
│   ├── pages/
│   ├── components/
│   └── assets/
│
├── miniprogram/            # 微信小程序端
│   ├── pages/
│   ├── components/
│   ├── utils/
│   ├── images/
│   └── app.js
│
└── sql/                    # 数据库脚本
```

---

## 4. 功能范围

## 4.1 保留功能

| 模块   | 是否建设 | 说明                     |
| ---- | ---: | ---------------------- |
| 首页   |    是 | 搜索、Banner、天气、推荐入口、功能宫格 |
| 商城   |    是 | 商品、分类、购物车、订单、支付、物流、售后  |
| 农业认养 |    是 | 认养项目、认养订单、成长记录、产出领取    |
| 视频监控 |    是 | 农场监控、养殖区监控、认养监控        |
| 礼品卡  |    是 | 购买、绑定、转赠、抵扣、余额记录       |
| 乡村文旅 |    是 | 文旅项目、预约、支付、核销          |
| 会员中心 |    是 | 用户资料、订单、认养、礼品卡、优惠券、积分  |
| 后台管理 |    是 | 商品、订单、用户、认养、视频、营销、文旅等  |

## 4.2 取消功能

| 模块     | 状态 | 说明                  |
| ------ | -- | ------------------- |
| 土地种植   | 取消 | 不做租地、地块、土地订单、我的土地   |
| 租地种植   | 取消 | 首页不再展示该入口           |
| 土地管理后台 | 取消 | 后台不再维护地块信息          |
| 土地视频权限 | 取消 | 视频只保留公共农场、认养、养殖区等权限 |

---

## 5. 小程序端功能设计

## 5.1 底部导航设计

取消“土地”导航后，建议底部导航改为：

| 导航 | 功能说明             |
| -- | ---------------- |
| 首页 | 首页推荐、活动、天气、功能入口  |
| 商城 | 农产品商品、分类、购物车、下单  |
| 认养 | 农业认养项目、我的认养入口    |
| 文旅 | 乡村旅游、采摘、亲子活动预约   |
| 我的 | 订单、礼品卡、优惠券、积分、设置 |

也可以保留五个导航为：

```text
首页 / 商城 / 认养 / 视频 / 我的
```

其中“文旅”和“礼品卡”放到首页宫格入口。

## 5.2 首页

### 5.2.1 页面功能

首页是平台的综合入口，展示平台形象、业务入口、天气信息、推荐商品和活动内容。

### 5.2.2 页面组成

| 区域       | 功能说明                    |
| -------- | ----------------------- |
| 顶部搜索     | 搜索商品、认养项目、文旅项目          |
| Banner 区 | 展示平台广告、活动、专题            |
| 推荐卡片     | 农业认养、视频监控、乡村文旅、礼品卡等     |
| 天气区      | 展示当前天气、温度、湿度、风力、空气质量    |
| 功能宫格     | 商城分类、农业认养、视频监控、礼品卡、乡村文旅 |
| 推荐商品     | 展示热销农产品、绿色商品、认养产出       |
| 底部导航     | 首页、商城、认养、文旅、我的          |

### 5.2.3 首页功能入口

| 入口   | 说明             |
| ---- | -------------- |
| 农业认养 | 进入认养项目列表       |
| 视频监控 | 进入视频监控列表       |
| 礼品卡  | 进入礼品卡购买与绑定页面   |
| 乡村文旅 | 进入文旅项目列表       |
| 新鲜蔬果 | 进入商城商品分类       |
| 农场直供 | 展示平台重点商品       |
| 我的订单 | 快速进入订单列表       |
| 客服中心 | 在线客服、电话客服、常见问题 |

### 5.2.4 首页接口

```text
GET /api/home/index
```

#### 返回数据

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "banners": [],
    "menus": [],
    "weather": {},
    "recommend_goods": [],
    "recommend_adopts": [],
    "notice": []
  }
}
```

---

## 5.3 搜索功能

### 5.3.1 功能说明

用户可通过搜索框搜索商品、认养项目、文旅项目。

### 5.3.2 搜索范围

| 类型 | 说明               |
| -- | ---------------- |
| 商品 | 商品名称、卖点、分类、标签    |
| 认养 | 认养项目名称、农场名称、认养类型 |
| 文旅 | 文旅项目名称、活动说明、地点   |

### 5.3.3 功能规则

1. 支持搜索历史。
2. 支持热门关键词。
3. 支持按类型筛选搜索结果。
4. 支持清空搜索历史。
5. 搜索结果按相关度、销量、价格、时间排序。

### 5.3.4 主要接口

```text
GET /api/search/hot
GET /api/search/history
POST /api/search/clear-history
GET /api/search?keyword=鸡蛋&type=goods&page=1
```

---

## 5.4 商城模块

## 5.4.1 商城首页

### 功能说明

展示商品分类、活动 Banner、推荐商品、新品、热销商品、农场直供商品。

### 页面模块

| 模块        | 说明                       |
| --------- | ------------------------ |
| 搜索栏       | 输入商品关键词搜索                |
| 分类入口      | 新鲜蔬菜、时令水果、肉禽蛋奶、粮油副食、礼盒套餐 |
| 商城 Banner | 展示促销活动                   |
| 热销商品      | 按销量推荐                    |
| 新品上市      | 按上架时间推荐                  |
| 农场直供      | 平台精选商品                   |
| 认养产出      | 认养项目产出的商品                |

### 主要接口

```text
GET /api/mall/index
GET /api/goods/categories
GET /api/goods/list
```

## 5.4.2 商品分类

### 分类建议

| 一级分类 | 二级分类示例           |
| ---- | ---------------- |
| 新鲜蔬菜 | 叶菜类、根茎类、瓜果类、菌菇类  |
| 时令水果 | 苹果、桃、梨、葡萄、草莓     |
| 肉禽蛋奶 | 鸡蛋、羊肉、牛肉、鸡鸭、鲜奶   |
| 粮油副食 | 大米、面粉、杂粮、食用油、调味品 |
| 农场礼盒 | 节日礼盒、企业福利、家庭套餐   |
| 认养产出 | 认养鸡蛋、认养水果、认养肉类   |
| 预售商品 | 季节预售、采摘预售、年货预售   |

## 5.4.3 商品列表

### 查询条件

| 条件    | 说明           |
| ----- | ------------ |
| 分类 ID | 按分类筛选        |
| 关键词   | 按商品名称搜索      |
| 价格区间  | 最低价、最高价      |
| 销量排序  | 销量升序或降序      |
| 上新排序  | 上架时间排序       |
| 商品标签  | 有机、绿色、直供、可溯源 |

### 商品卡片字段

| 字段    | 说明           |
| ----- | ------------ |
| 商品主图  | 封面图片         |
| 商品名称  | 商品标题         |
| 商品副标题 | 简短卖点         |
| 销售价   | 当前价格         |
| 原价    | 划线价，可选       |
| 销量    | 已售数量         |
| 标签    | 有机、绿色、直供、可溯源 |

### 接口

```text
GET /api/goods/list?category_id=1&keyword=鸡蛋&page=1&page_size=20
```

## 5.4.4 商品详情

### 页面模块

| 模块   | 说明                 |
| ---- | ------------------ |
| 商品轮播 | 图片、视频              |
| 商品信息 | 名称、价格、销量、库存、产地     |
| 规格选择 | SKU 规格，如重量、套餐、包装   |
| 配送说明 | 快递、冷链、自提等          |
| 商品详情 | 图文介绍               |
| 溯源信息 | 农场、批次、采摘或生产时间、检测报告 |
| 用户评价 | 评分、文字、图片           |
| 推荐商品 | 相似商品               |
| 底部操作 | 客服、收藏、购物车、立即购买     |

### 接口

```text
GET /api/goods/detail?id=1001
POST /api/goods/favorite
POST /api/goods/unfavorite
```

## 5.4.5 购物车

### 功能说明

用户可以将商品加入购物车，统一结算。

### 功能规则

1. 支持加入购物车。
2. 支持修改数量。
3. 支持删除商品。
4. 支持批量选择结算。
5. 支持库存不足提示。
6. 支持商品下架或失效提示。
7. 支持价格变更提示。

### 接口

```text
GET /api/cart/list
POST /api/cart/add
POST /api/cart/update
POST /api/cart/delete
POST /api/cart/select
```

## 5.4.6 订单确认

### 页面模块

| 模块   | 说明              |
| ---- | --------------- |
| 收货地址 | 选择或新增地址         |
| 商品清单 | 本次结算商品          |
| 配送方式 | 快递、冷链、自提        |
| 优惠券  | 选择可用优惠券         |
| 礼品卡  | 选择礼品卡抵扣         |
| 积分抵扣 | 可选使用积分          |
| 发票信息 | 可选              |
| 买家留言 | 用户备注            |
| 金额汇总 | 商品金额、运费、优惠、实付金额 |

### 订单金额计算

```text
实付金额 = 商品总金额 + 运费 - 优惠券抵扣 - 礼品卡抵扣 - 积分抵扣
```

### 接口

```text
POST /api/order/confirm
POST /api/order/create
```

## 5.4.7 支付

### 支付方式

| 支付方式  | 说明        |
| ----- | --------- |
| 微信支付  | 小程序主支付方式  |
| 礼品卡抵扣 | 支持全部或部分抵扣 |
| 积分抵扣  | 按平台规则抵扣   |
| 余额支付  | 可作为二期功能   |

### 支付流程

1. 小程序创建订单。
2. 后端生成微信支付预支付订单。
3. 小程序调用微信支付。
4. 微信支付完成后回调后端。
5. 后端校验回调签名。
6. 后端更新订单支付状态。
7. 小程序跳转支付结果页。

### 接口

```text
POST /api/pay/wechat/prepay
POST /api/pay/wechat/notify
GET /api/pay/result?order_no=xxx
```

## 5.4.8 订单管理

### 用户端订单状态

| 状态  | 说明            |
| --- | ------------- |
| 待支付 | 已下单未支付        |
| 待发货 | 已支付未发货        |
| 待收货 | 已发货未确认        |
| 已完成 | 已确认收货         |
| 已取消 | 用户取消或超时取消     |
| 售后中 | 正在退款、退货、换货、补发 |
| 已退款 | 退款完成          |

### 接口

```text
GET /api/order/list?status=paid&page=1
GET /api/order/detail?order_no=xxx
POST /api/order/cancel
POST /api/order/confirm-receive
POST /api/order/delete
```

## 5.4.9 物流

### 配送方式

| 配送方式 | 说明        |
| ---- | --------- |
| 普通快递 | 常规农产品     |
| 冷链配送 | 生鲜、肉禽、奶制品 |
| 同城配送 | 周边区域配送    |
| 农场自提 | 用户到农场领取   |

### 接口

```text
GET /api/logistics/query?order_no=xxx
```

## 5.4.10 售后

### 售后类型

| 类型   | 说明        |
| ---- | --------- |
| 仅退款  | 未发货或特殊情况  |
| 退货退款 | 用户退回商品后退款 |
| 换货   | 商品损坏、错发等  |
| 补发   | 漏发、破损补发   |

### 售后流程

```text
用户提交售后申请
    ↓
平台审核
    ↓
用户退货或补充凭证
    ↓
平台确认
    ↓
退款 / 换货 / 补发
    ↓
售后完成
```

### 接口

```text
POST /api/after-sale/apply
GET /api/after-sale/list
GET /api/after-sale/detail
POST /api/after-sale/cancel
```

---

## 5.5 农业认养模块

## 5.5.1 认养项目列表

### 功能说明

展示可认养的动物、果树、农作物等项目。

### 认养类型

| 类型    | 示例          |
| ----- | ----------- |
| 动物认养  | 羊、鸡、鸭、牛     |
| 果树认养  | 苹果树、桃树、梨树   |
| 农作物认养 | 玉米、水稻、小麦、蔬菜 |

### 列表字段

| 字段   | 说明           |
| ---- | ------------ |
| 项目图片 | 认养项目封面       |
| 项目名称 | 如认养一只羊       |
| 认养价格 | 单次或周期费用      |
| 认养周期 | 月度、季度、年度     |
| 预计产出 | 鸡蛋、水果、肉类等    |
| 剩余数量 | 可认养数量        |
| 农场名称 | 所属农场         |
| 标签   | 可监控、包配送、可续养等 |

### 接口

```text
GET /api/adopt/categories
GET /api/adopt/list
```

## 5.5.2 认养详情

### 页面模块

| 模块    | 说明            |
| ----- | ------------- |
| 认养项目图 | 项目图片、视频       |
| 基础信息  | 名称、价格、周期、剩余数量 |
| 农场介绍  | 农场环境、地址、资质    |
| 认养权益  | 产出、配送、证书、视频查看 |
| 成长记录  | 农场人员上传的记录     |
| 视频监控  | 查看对应农场或养殖区视频  |
| 认养协议  | 认养规则、风险提示     |
| 立即认养  | 下单支付          |

### 接口

```text
GET /api/adopt/detail?id=1001
POST /api/adopt/order/confirm
POST /api/adopt/order/create
```

## 5.5.3 我的认养

### 功能说明

展示用户已经认养的项目、成长记录、视频入口、产出领取情况。

### 状态设计

| 状态  | 说明        |
| --- | --------- |
| 认养中 | 正常认养周期内   |
| 待产出 | 已到产出阶段    |
| 待领取 | 用户可领取产出   |
| 配送中 | 产出已发货     |
| 已完成 | 本期认养结束    |
| 已到期 | 认养周期结束未续费 |

### 接口

```text
GET /api/user/adopt/list
GET /api/user/adopt/detail?id=xxx
GET /api/user/adopt/records?id=xxx
POST /api/user/adopt/receive-output
POST /api/user/adopt/renew
```

## 5.5.4 成长记录

### 记录类型

| 类型   | 说明            |
| ---- | ------------- |
| 图文记录 | 图片 + 文字说明     |
| 视频记录 | 短视频记录         |
| 农事记录 | 喂养、打疫苗、施肥、修剪等 |
| 产出记录 | 产蛋、结果、采摘、出栏等  |

### 后台维护规则

1. 农场人员可按认养对象上传记录。
2. 用户端按时间倒序展示。
3. 用户只能查看自己有权限的专属认养记录。
4. 公共记录可展示在认养详情中。

---

## 5.6 视频监控模块

## 5.6.1 功能说明

用户可查看农场、养殖区、认养项目相关视频画面。

### 视频类型

| 类型     | 说明              |
| ------ | --------------- |
| 公共农场监控 | 所有用户可查看         |
| 养殖区监控  | 展示动物养殖区域        |
| 认养专属监控 | 认养用户可查看对应项目相关画面 |
| 文旅现场监控 | 可展示采摘园、活动场地     |

## 5.6.2 权限规则

| 视频类型   | 访问权限          |
| ------ | ------------- |
| public | 所有用户可看        |
| login  | 登录用户可看        |
| adopt  | 购买对应认养项目的用户可看 |
| admin  | 后台管理员可看       |

## 5.6.3 页面功能

1. 视频列表。
2. 按农场、区域、类型筛选。
3. 视频播放。
4. 视频离线提示。
5. 视频收藏。
6. 视频截图，可选。

## 5.6.4 接口

```text
GET /api/video/list
GET /api/video/detail?id=xxx
GET /api/video/play-url?id=xxx
```

---

## 5.7 礼品卡模块

## 5.7.1 功能说明

用户可以购买礼品卡、绑定礼品卡、赠送礼品卡，并在订单结算时使用礼品卡抵扣金额。

## 5.7.2 礼品卡类型

| 类型    | 说明           |
| ----- | ------------ |
| 电子礼品卡 | 在线购买后生成卡号和密码 |
| 实体礼品卡 | 可配送实体卡片      |
| 企业福利卡 | 企业批量采购       |
| 指定商品卡 | 只能兑换指定商品或礼盒  |
| 储值卡   | 可在商城订单中抵扣    |

## 5.7.3 功能规则

1. 礼品卡支持面额配置。
2. 支持有效期。
3. 支持购买后自己使用或赠送好友。
4. 支持绑定到账户。
5. 支持订单结算抵扣。
6. 支持查看余额和使用明细。
7. 支持后台批量生成。
8. 礼品卡不能重复绑定。
9. 已过期或已冻结礼品卡不可使用。

## 5.7.4 接口

```text
GET /api/gift-card/list
GET /api/gift-card/detail?id=xxx
POST /api/gift-card/buy
POST /api/gift-card/bind
GET /api/user/gift-card/list
GET /api/user/gift-card/records
POST /api/order/use-gift-card
```

---

## 5.8 乡村文旅模块

## 5.8.1 功能说明

展示农场参观、采摘体验、亲子活动、农家餐饮、团建活动等文旅项目，支持在线预约和支付。

## 5.8.2 文旅项目类型

| 类型   | 示例             |
| ---- | -------------- |
| 农场参观 | 智慧农场参观、养殖区参观   |
| 采摘体验 | 草莓采摘、桃子采摘、蔬菜采摘 |
| 亲子活动 | 种植体验、喂养动物、农事课堂 |
| 农家餐饮 | 农家宴、特色餐饮       |
| 企业团建 | 农场团建、户外拓展      |

## 5.8.3 预约流程

```text
用户浏览文旅项目
    ↓
选择日期、场次、人数
    ↓
填写联系人信息
    ↓
提交预约订单
    ↓
支付
    ↓
生成预约码
    ↓
到场核销
```

## 5.8.4 接口

```text
GET /api/travel/list
GET /api/travel/detail?id=xxx
GET /api/travel/schedules?project_id=xxx
POST /api/travel/order/create
GET /api/user/travel/order/list
GET /api/user/travel/order/detail
POST /api/travel/order/cancel
POST /api/admin/travel/verify
```

---

## 5.9 我的模块

## 5.9.1 页面功能

| 模块    | 说明             |
| ----- | -------------- |
| 用户信息  | 头像、昵称、手机号、会员等级 |
| 我的订单  | 商城订单状态入口       |
| 我的认养  | 查看认养记录         |
| 我的预约  | 查看文旅预约         |
| 我的礼品卡 | 礼品卡余额和明细       |
| 优惠券   | 可用、已用、过期优惠券    |
| 积分    | 积分余额和明细        |
| 收藏    | 收藏商品、认养项目      |
| 收货地址  | 地址新增、编辑、删除     |
| 客服中心  | 在线客服、电话客服、常见问题 |
| 设置    | 授权设置、账号安全、退出登录 |

## 5.9.2 接口

```text
GET /api/user/profile
POST /api/user/update-profile
POST /api/user/bind-phone
GET /api/user/assets
GET /api/user/address/list
POST /api/user/address/add
POST /api/user/address/update
POST /api/user/address/delete
POST /api/user/address/set-default
```

---

## 6. PC 管理后台功能设计

## 6.1 后台登录

### 功能说明

后台人员通过账号密码登录管理系统，不同角色拥有不同权限。

### 功能规则

1. 支持账号密码登录。
2. 支持验证码。
3. 支持登录失败锁定。
4. 支持角色权限控制。
5. 支持操作日志记录。

## 6.2 后台首页

### 数据概览

| 指标    | 说明       |
| ----- | -------- |
| 今日销售额 | 今日支付成功金额 |
| 今日订单数 | 今日商城订单数量 |
| 待发货订单 | 已支付未发货订单 |
| 待处理售后 | 售后待审核数量  |
| 新增用户  | 今日新注册用户  |
| 认养订单  | 今日新增认养订单 |
| 文旅预约  | 今日新增预约   |
| 礼品卡销售 | 今日礼品卡销售额 |

## 6.3 商品管理

### 功能清单

| 功能     | 说明           |
| ------ | ------------ |
| 商品列表   | 查询、筛选、查看商品   |
| 新增商品   | 新增商品基础信息     |
| 编辑商品   | 修改商品信息       |
| 商品上下架  | 控制商品销售状态     |
| 商品分类   | 维护分类结构       |
| SKU 管理 | 规格、价格、库存     |
| 商品标签   | 有机、绿色、直供、可溯源 |
| 商品详情   | 富文本图文详情      |
| 商品评价   | 查看、审核、隐藏评价   |
| 商品导入导出 | 批量维护商品       |

## 6.4 订单管理

### 功能清单

| 功能   | 说明           |
| ---- | ------------ |
| 订单列表 | 查询订单         |
| 订单详情 | 查看订单完整信息     |
| 发货   | 填写物流公司和单号    |
| 批量发货 | Excel 导入物流单号 |
| 订单备注 | 客服内部备注       |
| 退款审核 | 处理退款申请       |
| 售后管理 | 退货、换货、补发     |
| 订单导出 | 导出订单数据       |

## 6.5 用户管理

| 功能   | 说明              |
| ---- | --------------- |
| 用户列表 | 查看注册用户          |
| 用户详情 | 资料、订单、认养、礼品卡、积分 |
| 用户等级 | 会员等级维护          |
| 积分调整 | 后台调整积分          |
| 用户标签 | 标记重点用户          |
| 黑名单  | 限制异常用户          |

## 6.6 认养管理

| 功能   | 说明               |
| ---- | ---------------- |
| 认养分类 | 动物、果树、农作物分类      |
| 认养项目 | 新增、编辑、上下架项目      |
| 认养对象 | 维护具体动物、果树、作物编号   |
| 认养订单 | 查询和处理认养订单        |
| 成长记录 | 上传图文、视频、农事记录     |
| 产出管理 | 记录产出数量、领取状态、配送状态 |
| 续养管理 | 到期提醒和续费处理        |

## 6.7 视频管理

| 功能    | 说明                       |
| ----- | ------------------------ |
| 摄像头列表 | 查看摄像头                    |
| 新增摄像头 | 配置名称、位置、播放地址             |
| 视频分组  | 农场、养殖区、认养项目分组            |
| 权限配置  | public、login、adopt、admin |
| 在线状态  | 手动或接口同步状态                |
| 播放地址  | RTMP、HLS、FLV 等地址维护       |

## 6.8 礼品卡管理

| 功能      | 说明          |
| ------- | ----------- |
| 礼品卡模板   | 面额、有效期、适用范围 |
| 礼品卡生成   | 单张或批量生成     |
| 礼品卡销售   | 购买记录        |
| 礼品卡绑定   | 用户绑定记录      |
| 使用明细    | 抵扣订单明细      |
| 冻结 / 解冻 | 异常礼品卡处理     |
| 导出      | 企业福利发卡导出    |

## 6.9 文旅管理

| 功能   | 说明            |
| ---- | ------------- |
| 项目管理 | 新增、编辑、上下架文旅项目 |
| 场次管理 | 日期、时间、库存、价格   |
| 预约订单 | 查询预约订单        |
| 核销管理 | 后台或扫码核销       |
| 退款处理 | 文旅预约退款        |
| 评价管理 | 查看用户评价        |

## 6.10 营销管理

| 功能        | 说明              |
| --------- | --------------- |
| Banner 管理 | 首页、商城 Banner 配置 |
| 首页菜单      | 功能宫格配置          |
| 推荐位       | 推荐商品、认养、文旅项目    |
| 优惠券       | 满减券、折扣券、包邮券     |
| 活动管理      | 秒杀、拼团、预售，可二期建设  |
| 积分规则      | 注册、下单、评价、签到积分   |
| 会员规则      | 等级、权益、折扣        |

## 6.11 系统管理

| 功能    | 说明            |
| ----- | ------------- |
| 管理员管理 | 后台账号维护        |
| 角色管理  | 角色权限配置        |
| 菜单管理  | 后台菜单配置        |
| 系统参数  | 支付、物流、短信、存储配置 |
| 操作日志  | 记录后台操作        |
| 登录日志  | 记录登录行为        |
| 数据字典  | 订单状态、售后原因等配置  |

---

## 7. 角色权限设计

## 7.1 小程序端角色

| 角色   | 权限                     |
| ---- | ---------------------- |
| 游客   | 浏览首页、商城、商品详情、认养详情、文旅详情 |
| 注册用户 | 下单、支付、收藏、评价、购买礼品卡、预约文旅 |
| 认养用户 | 查看我的认养、成长记录、专属视频       |
| 会员用户 | 享受会员价、积分、优惠券等权益        |

## 7.2 后台角色

| 角色    | 权限                  |
| ----- | ------------------- |
| 超级管理员 | 全部权限                |
| 运营人员  | 商品、营销、Banner、内容管理   |
| 订单客服  | 订单、发货、售后、用户查询       |
| 农场人员  | 认养对象、成长记录、产出管理、视频管理 |
| 财务人员  | 订单金额、退款、礼品卡、数据导出    |
| 文旅人员  | 文旅项目、场次、预约、核销       |

---

## 8. 数据库设计

## 8.1 设计原则

1. 所有表使用统一主键 `id`。
2. 所有业务表保留 `created_at`、`updated_at`。
3. 重要业务表保留 `deleted_at`，支持软删除。
4. 金额字段使用 `decimal(10,2)`。
5. 状态字段统一使用 `tinyint` 或 `varchar`。
6. MySQL 5.6 环境下避免使用 JSON 字段，建议使用 `text` 存储扩展内容。
7. 表名统一使用小写下划线命名。

## 8.2 用户表 `users`

| 字段              | 类型            | 说明            |
| --------------- | ------------- | ------------- |
| id              | int           | 主键            |
| openid          | varchar(100)  | 微信 openid     |
| unionid         | varchar(100)  | 微信 unionid，可空 |
| nickname        | varchar(100)  | 昵称            |
| avatar          | varchar(255)  | 头像            |
| phone           | varchar(20)   | 手机号           |
| gender          | tinyint       | 性别            |
| member_level    | tinyint       | 会员等级          |
| points          | int           | 积分余额          |
| balance         | decimal(10,2) | 账户余额，可选       |
| status          | tinyint       | 状态，1正常，0禁用    |
| last_login_time | datetime      | 最近登录时间        |
| created_at      | datetime      | 创建时间          |
| updated_at      | datetime      | 更新时间          |

## 8.3 商品分类表 `goods_categories`

| 字段         | 类型           | 说明      |
| ---------- | ------------ | ------- |
| id         | int          | 主键      |
| parent_id  | int          | 父级分类 ID |
| name       | varchar(100) | 分类名称    |
| icon       | varchar(255) | 分类图标    |
| sort       | int          | 排序      |
| status     | tinyint      | 状态      |
| created_at | datetime     | 创建时间    |
| updated_at | datetime     | 更新时间    |

## 8.4 商品表 `goods`

| 字段           | 类型            | 说明              |
| ------------ | ------------- | --------------- |
| id           | int           | 主键              |
| category_id  | int           | 分类 ID           |
| title        | varchar(200)  | 商品名称            |
| subtitle     | varchar(255)  | 商品副标题           |
| cover        | varchar(255)  | 商品主图            |
| images       | text          | 商品图片，多图逗号分隔或序列化 |
| price        | decimal(10,2) | 销售价             |
| market_price | decimal(10,2) | 市场价             |
| stock        | int           | 总库存             |
| sales        | int           | 销量              |
| unit         | varchar(20)   | 单位              |
| origin       | varchar(100)  | 产地              |
| tags         | varchar(255)  | 标签              |
| detail       | text          | 商品详情            |
| trace_info   | text          | 溯源信息            |
| status       | tinyint       | 1上架，0下架         |
| is_recommend | tinyint       | 是否推荐            |
| created_at   | datetime      | 创建时间            |
| updated_at   | datetime      | 更新时间            |

## 8.5 商品 SKU 表 `goods_skus`

| 字段           | 类型            | 说明     |
| ------------ | ------------- | ------ |
| id           | int           | 主键     |
| goods_id     | int           | 商品 ID  |
| sku_name     | varchar(100)  | 规格名称   |
| sku_value    | varchar(100)  | 规格值    |
| price        | decimal(10,2) | SKU 价格 |
| market_price | decimal(10,2) | 市场价    |
| stock        | int           | 库存     |
| image        | varchar(255)  | SKU 图片 |
| status       | tinyint       | 状态     |
| created_at   | datetime      | 创建时间   |
| updated_at   | datetime      | 更新时间   |

## 8.6 购物车表 `carts`

| 字段         | 类型       | 说明     |
| ---------- | -------- | ------ |
| id         | int      | 主键     |
| user_id    | int      | 用户 ID  |
| goods_id   | int      | 商品 ID  |
| sku_id     | int      | SKU ID |
| quantity   | int      | 数量     |
| selected   | tinyint  | 是否选中   |
| created_at | datetime | 创建时间   |
| updated_at | datetime | 更新时间   |

## 8.7 订单主表 `orders`

| 字段               | 类型            | 说明                           |
| ---------------- | ------------- | ---------------------------- |
| id               | int           | 主键                           |
| order_no         | varchar(50)   | 订单号                          |
| user_id          | int           | 用户 ID                        |
| order_type       | varchar(20)   | goods、adopt、travel、gift_card |
| total_amount     | decimal(10,2) | 商品总金额                        |
| freight_amount   | decimal(10,2) | 运费                           |
| discount_amount  | decimal(10,2) | 优惠金额                         |
| gift_card_amount | decimal(10,2) | 礼品卡抵扣                        |
| points_amount    | decimal(10,2) | 积分抵扣                         |
| pay_amount       | decimal(10,2) | 实付金额                         |
| pay_status       | tinyint       | 支付状态                         |
| order_status     | varchar(30)   | 订单状态                         |
| pay_type         | varchar(30)   | 支付方式                         |
| address_id       | int           | 收货地址 ID                      |
| receiver_name    | varchar(50)   | 收货人                          |
| receiver_phone   | varchar(20)   | 收货电话                         |
| receiver_address | varchar(255)  | 收货地址                         |
| remark           | varchar(255)  | 用户备注                         |
| paid_at          | datetime      | 支付时间                         |
| shipped_at       | datetime      | 发货时间                         |
| finished_at      | datetime      | 完成时间                         |
| canceled_at      | datetime      | 取消时间                         |
| created_at       | datetime      | 创建时间                         |
| updated_at       | datetime      | 更新时间                         |

## 8.8 订单明细表 `order_items`

| 字段          | 类型            | 说明       |
| ----------- | ------------- | -------- |
| id          | int           | 主键       |
| order_id    | int           | 订单 ID    |
| goods_id    | int           | 商品 ID    |
| sku_id      | int           | SKU ID   |
| goods_title | varchar(200)  | 商品名称快照   |
| sku_name    | varchar(100)  | SKU 名称快照 |
| image       | varchar(255)  | 商品图片快照   |
| price       | decimal(10,2) | 单价       |
| quantity    | int           | 数量       |
| total_price | decimal(10,2) | 小计       |
| created_at  | datetime      | 创建时间     |

## 8.9 支付记录表 `payment_records`

| 字段             | 类型            | 说明      |
| -------------- | ------------- | ------- |
| id             | int           | 主键      |
| order_no       | varchar(50)   | 订单号     |
| user_id        | int           | 用户 ID   |
| transaction_id | varchar(100)  | 微信支付交易号 |
| pay_type       | varchar(30)   | 支付方式    |
| pay_amount     | decimal(10,2) | 支付金额    |
| status         | tinyint       | 状态      |
| notify_data    | text          | 回调原始数据  |
| paid_at        | datetime      | 支付时间    |
| created_at     | datetime      | 创建时间    |

## 8.10 收货地址表 `user_addresses`

| 字段         | 类型           | 说明    |
| ---------- | ------------ | ----- |
| id         | int          | 主键    |
| user_id    | int          | 用户 ID |
| name       | varchar(50)  | 收货人   |
| phone      | varchar(20)  | 手机号   |
| province   | varchar(50)  | 省     |
| city       | varchar(50)  | 市     |
| district   | varchar(50)  | 区县    |
| detail     | varchar(255) | 详细地址  |
| is_default | tinyint      | 是否默认  |
| created_at | datetime     | 创建时间  |
| updated_at | datetime     | 更新时间  |

## 8.11 物流表 `logistics`

| 字段           | 类型           | 说明     |
| ------------ | ------------ | ------ |
| id           | int          | 主键     |
| order_id     | int          | 订单 ID  |
| company_code | varchar(50)  | 物流公司编码 |
| company_name | varchar(100) | 物流公司名称 |
| tracking_no  | varchar(100) | 物流单号   |
| status       | varchar(50)  | 物流状态   |
| shipped_at   | datetime     | 发货时间   |
| created_at   | datetime     | 创建时间   |
| updated_at   | datetime     | 更新时间   |

## 8.12 售后表 `after_sales`

| 字段            | 类型            | 说明                            |
| ------------- | ------------- | ----------------------------- |
| id            | int           | 主键                            |
| after_sale_no | varchar(50)   | 售后编号                          |
| order_id      | int           | 订单 ID                         |
| order_item_id | int           | 订单明细 ID                       |
| user_id       | int           | 用户 ID                         |
| type          | varchar(30)   | refund、return、exchange、resend |
| reason        | varchar(100)  | 售后原因                          |
| description   | text          | 问题描述                          |
| images        | text          | 图片凭证                          |
| refund_amount | decimal(10,2) | 退款金额                          |
| status        | varchar(30)   | 状态                            |
| audit_remark  | varchar(255)  | 审核备注                          |
| created_at    | datetime      | 创建时间                          |
| updated_at    | datetime      | 更新时间                          |

## 8.13 认养分类表 `adopt_categories`

| 字段         | 类型           | 说明   |
| ---------- | ------------ | ---- |
| id         | int          | 主键   |
| name       | varchar(100) | 分类名称 |
| icon       | varchar(255) | 图标   |
| sort       | int          | 排序   |
| status     | tinyint      | 状态   |
| created_at | datetime     | 创建时间 |
| updated_at | datetime     | 更新时间 |

## 8.14 认养项目表 `adopt_projects`

| 字段              | 类型            | 说明     |
| --------------- | ------------- | ------ |
| id              | int           | 主键     |
| category_id     | int           | 分类 ID  |
| title           | varchar(200)  | 项目名称   |
| cover           | varchar(255)  | 封面图    |
| images          | text          | 图片集    |
| price           | decimal(10,2) | 认养价格   |
| cycle_days      | int           | 认养周期天数 |
| farm_name       | varchar(100)  | 农场名称   |
| farm_address    | varchar(255)  | 农场地址   |
| total_quantity  | int           | 总数量    |
| remain_quantity | int           | 剩余数量   |
| output_desc     | text          | 预计产出   |
| rights_desc     | text          | 认养权益   |
| agreement       | text          | 认养协议   |
| detail          | text          | 详情     |
| status          | tinyint       | 状态     |
| created_at      | datetime      | 创建时间   |
| updated_at      | datetime      | 更新时间   |

## 8.15 认养对象表 `adopt_objects`

| 字段         | 类型           | 说明                         |
| ---------- | ------------ | -------------------------- |
| id         | int          | 主键                         |
| project_id | int          | 认养项目 ID                    |
| object_no  | varchar(50)  | 对象编号                       |
| name       | varchar(100) | 对象名称                       |
| image      | varchar(255) | 图片                         |
| status     | varchar(30)  | available、adopted、finished |
| created_at | datetime     | 创建时间                       |
| updated_at | datetime     | 更新时间                       |

## 8.16 用户认养表 `user_adopts`

| 字段            | 类型          | 说明      |
| ------------- | ----------- | ------- |
| id            | int         | 主键      |
| adopt_no      | varchar(50) | 认养编号    |
| user_id       | int         | 用户 ID   |
| project_id    | int         | 项目 ID   |
| object_id     | int         | 认养对象 ID |
| order_id      | int         | 订单 ID   |
| start_date    | date        | 开始日期    |
| end_date      | date        | 结束日期    |
| status        | varchar(30) | 认养状态    |
| output_status | varchar(30) | 产出领取状态  |
| created_at    | datetime    | 创建时间    |
| updated_at    | datetime    | 更新时间    |

## 8.17 认养成长记录表 `adopt_records`

| 字段            | 类型           | 说明                       |
| ------------- | ------------ | ------------------------ |
| id            | int          | 主键                       |
| project_id    | int          | 项目 ID                    |
| object_id     | int          | 对象 ID，可空                 |
| user_adopt_id | int          | 用户认养 ID，可空               |
| title         | varchar(200) | 标题                       |
| content       | text         | 内容                       |
| images        | text         | 图片                       |
| video_url     | varchar(255) | 视频地址                     |
| record_type   | varchar(30)  | feed、growth、output、other |
| is_public     | tinyint      | 是否公开                     |
| created_by    | int          | 创建人                      |
| created_at    | datetime     | 创建时间                     |

## 8.18 视频监控表 `videos`

| 字段         | 类型           | 说明                       |
| ---------- | ------------ | ------------------------ |
| id         | int          | 主键                       |
| title      | varchar(100) | 视频名称                     |
| cover      | varchar(255) | 封面                       |
| farm_name  | varchar(100) | 农场名称                     |
| area_name  | varchar(100) | 区域名称                     |
| play_url   | varchar(500) | 播放地址                     |
| video_type | varchar(30)  | public、adopt、travel      |
| relate_id  | int          | 关联项目 ID，可空               |
| auth_type  | varchar(30)  | public、login、adopt、admin |
| status     | tinyint      | 1在线，0离线                  |
| sort       | int          | 排序                       |
| created_at | datetime     | 创建时间                     |
| updated_at | datetime     | 更新时间                     |

## 8.19 礼品卡模板表 `gift_card_templates`

| 字段         | 类型            | 说明    |
| ---------- | ------------- | ----- |
| id         | int           | 主键    |
| title      | varchar(100)  | 礼品卡名称 |
| cover      | varchar(255)  | 封面    |
| amount     | decimal(10,2) | 面额    |
| price      | decimal(10,2) | 售价    |
| valid_days | int           | 有效天数  |
| use_scope  | varchar(50)   | 使用范围  |
| status     | tinyint       | 状态    |
| created_at | datetime      | 创建时间  |
| updated_at | datetime      | 更新时间  |

## 8.20 礼品卡表 `gift_cards`

| 字段            | 类型            | 说明                               |
| ------------- | ------------- | -------------------------------- |
| id            | int           | 主键                               |
| template_id   | int           | 模板 ID                            |
| card_no       | varchar(50)   | 卡号                               |
| card_password | varchar(100)  | 卡密，加密存储                          |
| amount        | decimal(10,2) | 初始金额                             |
| balance       | decimal(10,2) | 当前余额                             |
| owner_user_id | int           | 当前绑定用户                           |
| status        | varchar(30)   | unused、bound、used、expired、frozen |
| expire_at     | datetime      | 过期时间                             |
| created_at    | datetime      | 创建时间                             |
| updated_at    | datetime      | 更新时间                             |

## 8.21 礼品卡使用记录表 `gift_card_records`

| 字段            | 类型            | 说明                  |
| ------------- | ------------- | ------------------- |
| id            | int           | 主键                  |
| gift_card_id  | int           | 礼品卡 ID              |
| user_id       | int           | 用户 ID               |
| order_id      | int           | 订单 ID，可空            |
| change_amount | decimal(10,2) | 变动金额                |
| balance       | decimal(10,2) | 变动后余额               |
| type          | varchar(30)   | bind、consume、refund |
| remark        | varchar(255)  | 备注                  |
| created_at    | datetime      | 创建时间                |

## 8.22 文旅项目表 `travel_projects`

| 字段         | 类型            | 说明   |
| ---------- | ------------- | ---- |
| id         | int           | 主键   |
| title      | varchar(200)  | 项目名称 |
| cover      | varchar(255)  | 封面   |
| images     | text          | 图片   |
| category   | varchar(50)   | 类型   |
| address    | varchar(255)  | 地址   |
| price      | decimal(10,2) | 基础价格 |
| detail     | text          | 详情   |
| notice     | text          | 预约须知 |
| status     | tinyint       | 状态   |
| created_at | datetime      | 创建时间 |
| updated_at | datetime      | 更新时间 |

## 8.23 文旅场次表 `travel_schedules`

| 字段            | 类型            | 说明      |
| ------------- | ------------- | ------- |
| id            | int           | 主键      |
| project_id    | int           | 文旅项目 ID |
| schedule_date | date          | 日期      |
| start_time    | time          | 开始时间    |
| end_time      | time          | 结束时间    |
| price         | decimal(10,2) | 场次价格    |
| stock         | int           | 可预约人数   |
| sold          | int           | 已预约人数   |
| status        | tinyint       | 状态      |
| created_at    | datetime      | 创建时间    |
| updated_at    | datetime      | 更新时间    |

## 8.24 文旅预约订单表 `travel_orders`

| 字段            | 类型            | 说明    |
| ------------- | ------------- | ----- |
| id            | int           | 主键    |
| order_no      | varchar(50)   | 预约订单号 |
| user_id       | int           | 用户 ID |
| project_id    | int           | 项目 ID |
| schedule_id   | int           | 场次 ID |
| contact_name  | varchar(50)   | 联系人   |
| contact_phone | varchar(20)   | 联系电话  |
| people_count  | int           | 人数    |
| pay_amount    | decimal(10,2) | 支付金额  |
| pay_status    | tinyint       | 支付状态  |
| status        | varchar(30)   | 预约状态  |
| verify_code   | varchar(50)   | 核销码   |
| verified_at   | datetime      | 核销时间  |
| created_at    | datetime      | 创建时间  |
| updated_at    | datetime      | 更新时间  |

## 8.25 优惠券表 `coupons`

| 字段         | 类型            | 说明                                 |
| ---------- | ------------- | ---------------------------------- |
| id         | int           | 主键                                 |
| title      | varchar(100)  | 优惠券名称                              |
| type       | varchar(30)   | full_reduce、discount、free_shipping |
| amount     | decimal(10,2) | 优惠金额                               |
| min_amount | decimal(10,2) | 使用门槛                               |
| start_time | datetime      | 开始时间                               |
| end_time   | datetime      | 结束时间                               |
| total      | int           | 发行数量                               |
| received   | int           | 已领取数量                              |
| status     | tinyint       | 状态                                 |
| created_at | datetime      | 创建时间                               |
| updated_at | datetime      | 更新时间                               |

## 8.26 用户优惠券表 `user_coupons`

| 字段            | 类型          | 说明                  |
| ------------- | ----------- | ------------------- |
| id            | int         | 主键                  |
| user_id       | int         | 用户 ID               |
| coupon_id     | int         | 优惠券 ID              |
| status        | varchar(30) | unused、used、expired |
| used_order_id | int         | 使用订单 ID             |
| used_at       | datetime    | 使用时间                |
| created_at    | datetime    | 创建时间                |

## 8.27 积分记录表 `points_records`

| 字段         | 类型           | 说明                                    |
| ---------- | ------------ | ------------------------------------- |
| id         | int          | 主键                                    |
| user_id    | int          | 用户 ID                                 |
| points     | int          | 积分变动                                  |
| balance    | int          | 变动后积分                                 |
| type       | varchar(30)  | register、order、evaluate、consume、admin |
| remark     | varchar(255) | 备注                                    |
| created_at | datetime     | 创建时间                                  |

## 8.28 Banner 表 `banners`

| 字段         | 类型           | 说明                     |
| ---------- | ------------ | ---------------------- |
| id         | int          | 主键                     |
| title      | varchar(100) | 标题                     |
| image      | varchar(255) | 图片                     |
| link_type  | varchar(30)  | goods、adopt、travel、url |
| link_value | varchar(255) | 跳转值                    |
| position   | varchar(50)  | home、mall              |
| sort       | int          | 排序                     |
| status     | tinyint      | 状态                     |
| start_time | datetime     | 开始时间                   |
| end_time   | datetime     | 结束时间                   |
| created_at | datetime     | 创建时间                   |
| updated_at | datetime     | 更新时间                   |

## 8.29 管理员表 `admins`

| 字段              | 类型           | 说明     |
| --------------- | ------------ | ------ |
| id              | int          | 主键     |
| username        | varchar(50)  | 用户名    |
| password        | varchar(255) | 密码哈希   |
| real_name       | varchar(50)  | 姓名     |
| role_id         | int          | 角色 ID  |
| status          | tinyint      | 状态     |
| last_login_time | datetime     | 最近登录时间 |
| created_at      | datetime     | 创建时间   |
| updated_at      | datetime     | 更新时间   |

## 8.30 操作日志表 `admin_logs`

| 字段         | 类型           | 说明     |
| ---------- | ------------ | ------ |
| id         | int          | 主键     |
| admin_id   | int          | 管理员 ID |
| action     | varchar(100) | 操作行为   |
| module     | varchar(100) | 模块     |
| content    | text         | 操作内容   |
| ip         | varchar(50)  | IP 地址  |
| created_at | datetime     | 创建时间   |

---

## 9. 核心接口设计

## 9.1 接口统一规范

### 请求方式

| 类型     | 说明            |
| ------ | ------------- |
| GET    | 查询数据          |
| POST   | 新增、修改、提交      |
| DELETE | 删除，可用 POST 替代 |

### 请求头

```text
Content-Type: application/json
Authorization: Bearer {token}
```

### 统一返回格式

```json
{
  "code": 0,
  "msg": "success",
  "data": {}
}
```

### 错误码建议

| code | 说明            |
| ---: | ------------- |
|    0 | 成功            |
|  400 | 参数错误          |
|  401 | 未登录或 token 失效 |
|  403 | 无权限           |
|  404 | 数据不存在         |
|  500 | 服务器错误         |
| 1001 | 商品库存不足        |
| 1002 | 商品已下架         |
| 2001 | 订单状态错误        |
| 3001 | 礼品卡不可用        |
| 4001 | 认养项目已售罄       |
| 5001 | 文旅场次名额不足      |

## 9.2 用户登录接口

```text
POST /api/auth/wx-login
```

### 请求参数

```json
{
  "code": "微信临时登录凭证",
  "nickname": "用户昵称",
  "avatar": "头像地址"
}
```

### 返回参数

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "token": "登录token",
    "user": {
      "id": 1,
      "nickname": "用户昵称",
      "avatar": "头像",
      "phone": "手机号"
    }
  }
}
```

## 9.3 商品列表接口

```text
GET /api/goods/list
```

### 请求参数

| 参数          | 必填 | 说明              |
| ----------- | -- | --------------- |
| category_id | 否  | 分类 ID           |
| keyword     | 否  | 搜索关键词           |
| sort        | 否  | sales、price、new |
| page        | 否  | 页码              |
| page_size   | 否  | 每页数量            |

## 9.4 创建订单接口

```text
POST /api/order/create
```

### 请求参数

```json
{
  "cart_ids": [1, 2],
  "address_id": 10,
  "coupon_id": 5,
  "gift_card_ids": [2],
  "use_points": 100,
  "remark": "请尽快发货"
}
```

### 处理逻辑

1. 校验用户登录状态。
2. 校验商品是否上架。
3. 校验 SKU 库存是否充足。
4. 计算商品金额、运费、优惠券、礼品卡、积分抵扣。
5. 锁定库存。
6. 创建订单。
7. 创建订单明细。
8. 返回订单号和支付金额。

## 9.5 微信支付预下单接口

```text
POST /api/pay/wechat/prepay
```

### 请求参数

```json
{
  "order_no": "202605250001"
}
```

### 返回参数

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "timeStamp": "1710000000",
    "nonceStr": "随机字符串",
    "package": "prepay_id=xxx",
    "signType": "RSA",
    "paySign": "签名"
  }
}
```

## 9.6 支付回调接口

```text
POST /api/pay/wechat/notify
```

### 处理逻辑

1. 接收微信支付回调。
2. 校验签名。
3. 解密回调报文。
4. 校验订单金额。
5. 判断订单是否已处理，避免重复回调。
6. 更新订单支付状态。
7. 扣减库存或确认库存扣减。
8. 记录支付流水。
9. 发放积分或优惠券。
10. 返回微信支付成功响应。

## 9.7 认养下单接口

```text
POST /api/adopt/order/create
```

### 请求参数

```json
{
  "project_id": 1001,
  "object_id": 2001,
  "cycle_num": 1,
  "address_id": 10,
  "remark": "认养备注"
}
```

### 处理逻辑

1. 校验认养项目是否上架。
2. 校验剩余数量。
3. 分配认养对象。
4. 创建订单。
5. 支付成功后生成用户认养记录。
6. 认养对象状态变更为 adopted。

## 9.8 礼品卡绑定接口

```text
POST /api/gift-card/bind
```

### 请求参数

```json
{
  "card_no": "GC20260001",
  "card_password": "123456"
}
```

### 处理逻辑

1. 校验卡号卡密。
2. 校验礼品卡状态。
3. 校验有效期。
4. 绑定到当前用户。
5. 生成绑定记录。

## 9.9 文旅预约接口

```text
POST /api/travel/order/create
```

### 请求参数

```json
{
  "project_id": 1,
  "schedule_id": 10,
  "people_count": 3,
  "contact_name": "张先生",
  "contact_phone": "13800000000"
}
```

### 处理逻辑

1. 校验场次是否可预约。
2. 校验库存人数是否充足。
3. 创建文旅预约订单。
4. 支付成功后扣减场次名额。
5. 生成核销码。

---

## 10. 微信小程序页面结构

## 10.1 页面目录建议

```text
miniprogram/pages
├── index/index                 # 首页
├── search/index                # 搜索页
├── mall/index                  # 商城首页
├── goods/list                  # 商品列表
├── goods/detail                # 商品详情
├── cart/index                  # 购物车
├── order/confirm               # 订单确认
├── order/list                  # 订单列表
├── order/detail                # 订单详情
├── pay/result                  # 支付结果
├── after-sale/apply            # 售后申请
├── after-sale/list             # 售后列表
├── adopt/index                 # 认养首页
├── adopt/list                  # 认养列表
├── adopt/detail                # 认养详情
├── user-adopt/list             # 我的认养
├── user-adopt/detail           # 认养详情记录
├── video/list                  # 视频列表
├── video/player                # 视频播放
├── gift-card/index             # 礼品卡首页
├── gift-card/detail            # 礼品卡详情
├── gift-card/bind              # 礼品卡绑定
├── travel/index                # 文旅首页
├── travel/detail               # 文旅详情
├── travel/confirm              # 文旅预约确认
├── user/index                  # 我的
├── user/profile                # 个人资料
├── address/list                # 地址列表
├── address/edit                # 地址编辑
├── coupon/list                 # 优惠券
├── points/list                 # 积分明细
└── customer-service/index      # 客服中心
```

## 10.2 小程序公共组件

| 组件               | 说明       |
| ---------------- | -------- |
| goods-card       | 商品卡片     |
| adopt-card       | 认养项目卡片   |
| travel-card      | 文旅项目卡片   |
| order-card       | 订单卡片     |
| price-view       | 价格显示     |
| empty-view       | 空状态      |
| loading-view     | 加载状态     |
| address-selector | 地址选择     |
| sku-popup        | SKU 选择弹窗 |
| coupon-popup     | 优惠券弹窗    |
| gift-card-popup  | 礼品卡选择弹窗  |

## 10.3 小程序全局配置

### app.json 示例

```json
{
  "pages": [
    "pages/index/index",
    "pages/mall/index",
    "pages/adopt/index",
    "pages/travel/index",
    "pages/user/index"
  ],
  "window": {
    "navigationBarTitleText": "生态农业商城",
    "navigationBarBackgroundColor": "#32B768",
    "navigationBarTextStyle": "white"
  },
  "tabBar": {
    "color": "#999999",
    "selectedColor": "#32B768",
    "list": [
      {
        "pagePath": "pages/index/index",
        "text": "首页"
      },
      {
        "pagePath": "pages/mall/index",
        "text": "商城"
      },
      {
        "pagePath": "pages/adopt/index",
        "text": "认养"
      },
      {
        "pagePath": "pages/travel/index",
        "text": "文旅"
      },
      {
        "pagePath": "pages/user/index",
        "text": "我的"
      }
    ]
  }
}
```

---

## 11. PHP 后端开发规范

## 11.1 分层规范

```text
Controller 控制器层
    ↓
Service 业务服务层
    ↓
Model 数据模型层
    ↓
MySQL 数据库
```

### Controller 层

负责：

1. 接收请求参数。
2. 调用参数校验。
3. 调用 Service。
4. 返回统一 JSON。

### Service 层

负责：

1. 业务规则处理。
2. 订单金额计算。
3. 支付回调处理。
4. 库存扣减。
5. 事务控制。

### Model 层

负责：

1. 数据库查询。
2. 数据保存。
3. 数据更新。
4. 关联查询。

## 11.2 命名规范

| 类型   | 示例                  |
| ---- | ------------------- |
| 控制器  | GoodsController.php |
| 服务类  | GoodsService.php    |
| 模型类  | GoodsModel.php      |
| 数据表  | goods_categories    |
| 接口路径 | /api/goods/list     |
| 变量名  | goods_id、order_no   |

## 11.3 事务处理场景

以下业务必须使用数据库事务：

1. 创建订单。
2. 支付成功回调。
3. 取消订单释放库存。
4. 退款处理。
5. 礼品卡抵扣。
6. 认养下单并分配认养对象。
7. 文旅预约扣减名额。

### 订单创建事务逻辑

```text
开启事务
    ↓
校验商品和库存
    ↓
创建订单主表
    ↓
创建订单明细
    ↓
锁定或扣减库存
    ↓
记录优惠券和礼品卡占用
    ↓
提交事务
```

### 异常处理

```text
发生异常
    ↓
回滚事务
    ↓
记录错误日志
    ↓
返回友好错误提示
```

---

## 12. 订单状态机

## 12.1 商品订单状态

```text
pending_payment  待支付
paid             已支付 / 待发货
shipped          已发货 / 待收货
finished         已完成
canceled         已取消
after_sale       售后中
refunded         已退款
```

## 12.2 状态流转

```text
待支付
  ├── 支付成功 → 待发货
  ├── 用户取消 → 已取消
  └── 超时未支付 → 已取消

待发货
  ├── 后台发货 → 待收货
  └── 用户申请退款 → 售后中

待收货
  ├── 用户确认收货 → 已完成
  ├── 系统自动确认 → 已完成
  └── 用户申请售后 → 售后中

已完成
  └── 可评价 / 可申请售后
```

## 12.3 认养订单状态

```text
pending_payment  待支付
adopting         认养中
waiting_output   待产出
waiting_receive  待领取
shipping         产出配送中
finished         已完成
expired          已到期
```

## 12.4 文旅订单状态

```text
pending_payment  待支付
paid             已预约
verified         已核销
canceled         已取消
refunded         已退款
```

---

## 13. 库存设计

## 13.1 商品库存

### 库存扣减方式

建议采用“支付成功扣减库存”或“下单锁库存”。

| 方式    | 优点     | 缺点        |
| ----- | ------ | --------- |
| 下单锁库存 | 避免超卖   | 未支付订单占用库存 |
| 支付扣库存 | 库存利用率高 | 高并发下需防止超卖 |

建议：普通农产品采用下单锁库存，未支付订单 15 分钟自动取消并释放库存。

## 13.2 认养库存

认养项目需要控制剩余数量：

1. 用户下单后锁定一个认养对象。
2. 支付成功后认养对象变为已认养。
3. 订单超时未支付，释放认养对象。
4. 认养项目剩余数量同步减少或恢复。

## 13.3 文旅库存

文旅场次库存按照人数扣减：

1. 下单时锁定预约人数。
2. 支付成功后确认扣减。
3. 超时未支付释放名额。
4. 退款成功后可按规则恢复名额。

---

## 14. 定时任务设计

| 任务     | 执行频率   | 说明              |
| ------ | ------ | --------------- |
| 取消超时订单 | 每 5 分钟 | 取消超过 15 分钟未支付订单 |
| 自动确认收货 | 每天 1 次 | 发货后指定天数自动完成     |
| 优惠券过期  | 每天 1 次 | 更新过期优惠券状态       |
| 礼品卡过期  | 每天 1 次 | 更新过期礼品卡状态       |
| 认养到期提醒 | 每天 1 次 | 提醒用户续养或领取产出     |
| 文旅预约提醒 | 每天 1 次 | 活动前一天提醒用户       |
| 数据统计汇总 | 每天 1 次 | 汇总销售、订单、用户数据    |

---

## 15. 微信能力接入

## 15.1 微信登录

### 流程

1. 小程序调用 `wx.login()` 获取 code。
2. 小程序将 code 发送到后端。
3. 后端调用微信接口换取 openid。
4. 后端查询或创建用户。
5. 后端生成 token 返回小程序。
6. 小程序保存 token。

## 15.2 手机号绑定

### 流程

1. 用户点击授权手机号。
2. 小程序获取手机号授权凭证。
3. 后端解密或调用微信接口获取手机号。
4. 绑定到用户表。

## 15.3 微信支付

### 支付对象

| 订单类型   | 是否支付 |
| ------ | ---- |
| 商城订单   | 是    |
| 认养订单   | 是    |
| 礼品卡订单  | 是    |
| 文旅预约订单 | 是    |

## 15.4 订阅消息

### 消息类型

| 类型     | 场景      |
| ------ | ------- |
| 支付成功通知 | 用户支付成功  |
| 发货通知   | 商城订单发货  |
| 售后处理通知 | 售后审核和退款 |
| 认养记录更新 | 成长记录更新  |
| 认养到期提醒 | 认养即将到期  |
| 文旅预约提醒 | 活动开始前提醒 |

---

## 16. 文件与图片管理

## 16.1 文件类型

| 类型    | 场景        |
| ----- | --------- |
| 商品图片  | 商品主图、详情图  |
| 认养图片  | 项目图、成长记录图 |
| 文旅图片  | 项目展示图     |
| 礼品卡图片 | 礼品卡封面     |
| 用户头像  | 微信头像      |
| 售后凭证  | 售后申请图片    |

## 16.2 上传规则

1. 限制图片大小。
2. 限制文件格式，如 jpg、png、webp。
3. 上传后生成唯一文件名。
4. 后台上传图片需要权限校验。
5. 图片建议压缩后展示。

---

## 17. 安全设计

## 17.1 接口安全

1. 所有接口使用 HTTPS。
2. 用户端接口使用 token 鉴权。
3. 后台接口使用管理员 token 鉴权。
4. 重要接口增加权限验证。
5. 防止 SQL 注入。
6. 防止 XSS。
7. 防止重复提交。
8. 支付回调必须验签。

## 17.2 数据安全

1. 密码使用哈希加密。
2. 礼品卡卡密加密存储。
3. 用户手机号脱敏展示。
4. 后台操作记录日志。
5. 数据库定期备份。

## 17.3 业务风控

| 场景      | 风控措施          |
| ------- | ------------- |
| 订单重复提交  | 使用请求唯一标识或订单锁  |
| 支付重复回调  | 判断订单支付状态，幂等处理 |
| 礼品卡重复使用 | 使用事务锁定礼品卡余额   |
| 库存超卖    | 行锁或库存条件更新     |
| 恶意售后    | 记录用户售后次数，后台审核 |
| 后台误操作   | 权限控制 + 操作日志   |

---

## 18. 数据统计与报表

## 18.1 首页数据

| 指标        | 说明              |
| --------- | --------------- |
| 首页访问量     | PV、UV           |
| Banner 点击 | 每个 Banner 点击次数  |
| 功能入口点击    | 认养、视频、礼品卡、文旅点击量 |
| 商品曝光      | 首页推荐商品曝光        |

## 18.2 商城数据

| 指标    | 说明          |
| ----- | ----------- |
| 商品浏览量 | 商品详情访问次数    |
| 加购次数  | 加入购物车次数     |
| 下单数量  | 创建订单数量      |
| 支付订单数 | 支付成功订单      |
| 销售额   | 支付成功金额      |
| 客单价   | 销售额 / 支付订单数 |
| 退款金额  | 退款订单金额      |

## 18.3 认养数据

| 指标      | 说明        |
| ------- | --------- |
| 认养项目浏览量 | 项目详情访问    |
| 认养下单数   | 认养订单数     |
| 认养支付数   | 支付成功认养订单  |
| 认养转化率   | 支付数 / 浏览量 |
| 续养率     | 到期后续费比例   |

## 18.4 文旅数据

| 指标      | 说明     |
| ------- | ------ |
| 文旅项目浏览量 | 文旅详情访问 |
| 预约订单数   | 提交预约数量 |
| 预约支付数   | 支付成功数量 |
| 核销人数    | 到场核销人数 |
| 退款率     | 退款订单比例 |

---

## 19. 业务流程

## 19.1 商城购物流程

```text
进入首页 / 商城
    ↓
浏览商品 / 搜索商品
    ↓
进入商品详情
    ↓
选择规格
    ↓
加入购物车 / 立即购买
    ↓
确认订单
    ↓
微信支付
    ↓
后台发货
    ↓
用户确认收货
    ↓
评价 / 售后
```

## 19.2 农业认养流程

```text
进入认养频道
    ↓
浏览认养项目
    ↓
查看认养详情
    ↓
选择认养对象或系统分配
    ↓
确认认养协议
    ↓
提交认养订单
    ↓
微信支付
    ↓
生成我的认养
    ↓
查看成长记录和视频
    ↓
产出领取 / 续养 / 完成
```

## 19.3 礼品卡流程

```text
用户购买礼品卡
    ↓
微信支付
    ↓
生成礼品卡
    ↓
自用绑定 / 赠送好友
    ↓
订单结算时选择礼品卡
    ↓
抵扣金额
    ↓
记录使用明细
```

## 19.4 文旅预约流程

```text
进入乡村文旅
    ↓
选择项目
    ↓
选择日期和场次
    ↓
填写联系人和人数
    ↓
提交预约订单
    ↓
支付
    ↓
生成核销码
    ↓
到场核销
    ↓
评价
```

---

## 20. 管理后台菜单结构

```text
后台首页
├── 数据概览

商品管理
├── 商品列表
├── 商品分类
├── 商品评价

订单管理
├── 商城订单
├── 发货管理
├── 售后管理
├── 退款管理

认养管理
├── 认养分类
├── 认养项目
├── 认养对象
├── 认养订单
├── 成长记录
├── 产出管理

视频管理
├── 视频列表
├── 视频分组
├── 权限配置

礼品卡管理
├── 礼品卡模板
├── 礼品卡列表
├── 购买记录
├── 使用记录

文旅管理
├── 文旅项目
├── 场次管理
├── 预约订单
├── 核销管理

营销管理
├── Banner 管理
├── 首页菜单
├── 推荐位
├── 优惠券
├── 积分规则
├── 会员规则

用户管理
├── 用户列表
├── 会员管理
├── 积分明细

系统管理
├── 管理员
├── 角色权限
├── 系统参数
├── 操作日志
```

---

## 21. MVP 首期开发范围

## 21.1 一期必须完成

| 模块   | 功能                     |
| ---- | ---------------------- |
| 首页   | Banner、搜索、天气、功能入口、推荐商品 |
| 商城   | 分类、列表、详情、购物车、订单、支付、物流  |
| 用户   | 微信登录、手机号、地址、个人中心       |
| 订单   | 下单、支付、取消、发货、确认收货       |
| 后台商品 | 分类、商品、SKU、上下架          |
| 后台订单 | 订单查询、发货、退款基础处理         |
| 认养   | 认养列表、详情、下单、我的认养        |
| 视频   | 视频列表、播放、基础权限           |
| 管理后台 | 管理员、角色、基础配置            |

## 21.2 二期建议完成

| 模块   | 功能             |
| ---- | -------------- |
| 礼品卡  | 购买、绑定、抵扣、明细    |
| 优惠券  | 领取、使用、过期       |
| 积分会员 | 积分、会员等级、会员价    |
| 认养增强 | 成长记录、产出领取、续养提醒 |
| 售后增强 | 退货、换货、补发完整流程   |
| 数据统计 | 销售报表、用户报表、认养报表 |

## 21.3 三期建议完成

| 模块    | 功能             |
| ----- | -------------- |
| 文旅    | 项目、场次、预约、支付、核销 |
| 企业福利  | 礼品卡批量采购、企业订单   |
| 农场直播  | 实时直播、短视频内容     |
| 智能推荐  | 商品和认养项目推荐      |
| 多农场管理 | 多农场、多仓库、多发货点   |

---

## 22. 开发排期建议

## 22.1 8 周开发计划

| 周期    | 工作内容                  |
| ----- | --------------------- |
| 第 1 周 | 需求确认、原型确认、数据库设计、接口规范  |
| 第 2 周 | 项目框架搭建、微信登录、用户模块、后台登录 |
| 第 3 周 | 商品分类、商品管理、商城首页、商品列表详情 |
| 第 4 周 | 购物车、订单确认、创建订单、微信支付    |
| 第 5 周 | 订单管理、发货、物流、用户订单中心     |
| 第 6 周 | 认养项目、认养下单、我的认养、视频基础功能 |
| 第 7 周 | 礼品卡基础、优惠券基础、后台营销配置    |
| 第 8 周 | 联调测试、修复问题、上线准备、验收交付   |

## 22.2 团队配置建议

| 角色       |  人数 | 职责              |
| -------- | --: | --------------- |
| 产品经理     |   1 | 需求、原型、验收        |
| UI 设计师   |   1 | 小程序和后台界面设计      |
| 小程序开发    | 1-2 | 微信小程序前端开发       |
| PHP 后端开发 | 1-2 | API、业务逻辑、数据库    |
| 后台前端开发   |   1 | PC 管理后台         |
| 测试人员     |   1 | 功能测试、支付测试、兼容测试  |
| 运维人员     | 0.5 | 服务器、域名、HTTPS、部署 |

---

## 23. 测试方案

## 23.1 功能测试

| 模块  | 测试点                 |
| --- | ------------------- |
| 登录  | 微信登录、token 过期、手机号绑定 |
| 商品  | 分类、搜索、详情、SKU、库存     |
| 购物车 | 加购、修改数量、删除、结算       |
| 订单  | 创建、取消、支付、发货、确认收货    |
| 支付  | 预下单、支付成功、支付失败、重复回调  |
| 认养  | 认养项目、下单、支付、我的认养     |
| 视频  | 视频列表、权限、播放、离线提示     |
| 礼品卡 | 购买、绑定、抵扣、余额变化       |
| 文旅  | 场次、预约、支付、核销         |
| 后台  | 管理员、权限、商品、订单、认养、营销  |

## 23.2 异常测试

| 场景       | 预期结果        |
| -------- | ----------- |
| 库存不足下单   | 提示库存不足      |
| 商品下架后购买  | 提示商品已下架     |
| 支付中关闭小程序 | 可继续支付或订单待支付 |
| 支付回调重复   | 订单不重复处理     |
| 礼品卡余额不足  | 提示不可抵扣或部分抵扣 |
| 文旅名额不足   | 提示名额不足      |
| 视频无权限    | 提示暂无查看权限    |
| token 失效 | 跳转登录        |

## 23.3 兼容测试

1. iOS 微信。
2. Android 微信。
3. 不同屏幕尺寸。
4. 弱网络环境。
5. 后台 Chrome / Edge 浏览器。

---

## 24. 上线准备

## 24.1 微信小程序准备

1. 注册并认证微信小程序。
2. 配置小程序服务器域名。
3. 配置业务域名。
4. 配置微信支付商户号。
5. 配置订阅消息模板。
6. 提交小程序审核。

## 24.2 服务器准备

1. 云服务器。
2. 域名。
3. HTTPS 证书。
4. MySQL 数据库。
5. PHP 运行环境。
6. Nginx / Apache。
7. 定时任务。
8. 数据库备份策略。

## 24.3 上线检查清单

| 检查项       | 是否完成 |
| --------- | ---- |
| HTTPS 可访问 | 待确认  |
| API 域名已配置 | 待确认  |
| 微信登录正常    | 待确认  |
| 微信支付正常    | 待确认  |
| 支付回调正常    | 待确认  |
| 商品可正常下单   | 待确认  |
| 后台可正常发货   | 待确认  |
| 图片上传正常    | 待确认  |
| 视频播放正常    | 待确认  |
| 数据库备份正常   | 待确认  |
| 小程序审核通过   | 待确认  |

---

## 25. 验收标准

## 25.1 用户端验收

1. 首页能正常展示 Banner、天气、功能入口、推荐商品。
2. 用户能正常微信登录。
3. 用户能浏览商品分类、商品列表、商品详情。
4. 用户能选择 SKU 加入购物车。
5. 用户能提交订单并完成微信支付。
6. 用户能查看订单状态、物流信息。
7. 用户能确认收货并评价。
8. 用户能提交售后申请。
9. 用户能浏览认养项目并完成认养支付。
10. 用户能查看我的认养和成长记录。
11. 用户能查看有权限的视频监控。
12. 用户能购买、绑定和使用礼品卡。
13. 用户能浏览并预约文旅项目。

## 25.2 后台验收

1. 管理员能正常登录后台。
2. 后台能维护商品分类、商品、SKU、库存。
3. 后台能查看订单并发货。
4. 后台能处理退款和售后。
5. 后台能维护认养项目、认养对象、成长记录。
6. 后台能维护视频监控地址和权限。
7. 后台能生成和管理礼品卡。
8. 后台能维护文旅项目和场次。
9. 后台能配置 Banner、首页菜单、推荐位、优惠券。
10. 后台操作日志可查询。

## 25.3 数据验收

1. 订单金额计算准确。
2. 库存扣减和释放准确。
3. 支付流水记录准确。
4. 礼品卡余额变化准确。
5. 积分变化准确。
6. 认养状态流转准确。
7. 文旅预约名额扣减准确。

---

## 26. 风险与注意事项

| 风险       | 说明         | 建议           |
| -------- | ---------- | ------------ |
| 生鲜库存不准   | 农产品库存变化快   | 后台支持快速调库存    |
| 冷链配送成本高  | 生鲜商品配送复杂   | 商品单独配置配送规则   |
| 认养交付周期长  | 用户期待高      | 定期上传成长记录和提醒  |
| 视频播放不稳定  | 摄像头和网络影响体验 | 增加离线提示和备用画面  |
| 礼品卡资金风险  | 涉及余额和抵扣    | 加强加密、日志和财务对账 |
| 微信支付回调异常 | 影响订单状态     | 增加主动查询支付结果   |
| 售后纠纷     | 生鲜商品易损耗    | 明确售后规则和凭证要求  |

---

## 27. 交付物清单

| 交付物    | 说明                 |
| ------ | ------------------ |
| 需求文档   | 当前文档               |
| 原型图    | 小程序和后台原型           |
| UI 设计稿 | 首页、商城、认养、我的、后台关键页面 |
| 数据库脚本  | MySQL 建表 SQL       |
| 后端源码   | PHP API 服务         |
| 小程序源码  | 微信小程序前端代码          |
| 后台源码   | PC 管理后台代码          |
| 接口文档   | API 请求和返回说明        |
| 部署文档   | 服务器部署说明            |
| 测试报告   | 功能测试和缺陷记录          |
| 上线说明   | 小程序发布和服务器上线说明      |

---

## 28. 版本说明

### 当前版本

| 版本   | 说明                                  |
| ---- | ----------------------------------- |
| V1.0 | PHP + MySQL + 微信小程序完整开发文档，已取消土地种植模块 |

### 与上一版相比的调整

1. 删除“土地种植 / 租地种植”全部功能。
2. 删除“土地”底部导航。
3. 删除“土地管理后台”。
4. 删除“我的土地”。
5. 删除土地相关数据库表。
6. 删除土地相关视频权限。
7. 明确技术栈为 PHP + MySQL + 微信小程序。
8. 增加接口设计、数据库设计、PHP 后端规范、测试方案、上线清单。
