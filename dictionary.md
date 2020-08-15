# 恒辉公众号项目数据库字典

### 目录

- 功能概述
- 数据对照表


---

**1\. 功能概述**

&emsp;&emsp;项目主要功能包括：
包含权限管理、用户管理、菜单管理、文章管理等基本模块；

---
**2\. 数据对照表**

### 通用字段说明

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| id | int(11)| 主键：该数据ID |
| listorder | int(11) | 自定义排序 |
| img_array | varchar(100) | 图片组 |
| create_time | int(11) | 创建时间 |
| update_time | int(11) | 更新时间 |
| delete_time | int(11) | 删除时间 |
| thirdapp_id | int(11) | 关联thirdapp |
| user_no | varchar(255) | 关联user |
| user_type | tinyint(2) | 用户类型0.前端2.cms |
| status | tinyint(2) | 状态:1正常；-1删除 |



### user表-update

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| login_name | varchar(100) | 登录名 |
| password | varchar(255) | 密码 |
| nickname | varchar(255) | 微信昵称 |
| openid | varchar(50) | 微信openid |
| headImgUrl | varchar(9999) | 微信头像 |
| role | int(11) | 权限角色 |
| primary_scope | int(11) | 权限级别：60.管理员;30.员工;10.用户 |
| user_type | tinyint(2) | 0,小程序用户;1,员工2,管理员; |
| user_no | varchar(255) | 用户编号 |
| staff_no | varchar(255) | 员工编号（手工设置） |
| number | varchar(255) | 用户编号（手工设置） |



### user_info表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| name | varchar(255) | 名称 |
| address | varchar(255) | 地址 |
| phone | varchar(255) | 电话 |
| behavior | tinyint(2) | 0.普通用户1.VIP |
| deadline | int(11) | 临时有效期，大于0即表明获取过免费机会 |



### third_app表-update

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| phone | varchar(255) | 客服手机号 |
| temporary | int(11) | 临时天数 |
| phone_array | text | 接收短信的电话组 |



### label表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| title | varchar(40) | 菜单名称 |
| description | varchar(255) | 描述 |
| parentid | int(11) | 父级菜单ID |
| type | tinyint(2) | 1,menu;2,menu_item;3.category;5.sku;6.sku_item |



### article表-update

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| menu_id | int(11) | 关于恒辉 |
| type | tinyint(2) | 0.无 |
| title | varchar(100) | 文章标题 |
| content | text | 公司简介 |
| passage1 | text | 企业文化 |
| mainImg | text | 主图 |
| bannerImg | text | 合作伙伴logo |

| menu_id | int(11) | 恒辉新闻/客户活动 |
| type | tinyint(2) | 1.新闻 |
| title | varchar(100) | 文章标题 |
| small_title | varchar(100) | 发布人 |
| content | text | 内容 |
| mainImg | text | 主图 |
| bannerImg | text | 视频 |

| menu_id | int(11) | 联系我们 |
| type | tinyint(2) | 0.无 |
| title | varchar(100) | 文章标题 |
| phone | varchar(255) | 电话 |
| description | varchar(255) | 地址 |
| longitude | varchar(255) | 经度 |
| latitude | varchar(255) | 纬度 |

| menu_id | int(11) | 热销产品 |
| type | tinyint(2) | 2.产品 |
| title | varchar(100) | 产品名称 |
| top | tinyint(2) | 0.正常1.置顶 |
| hot | tinyint(2) | 0.正常1.热门 |
| small_title | varchar(100) | 预期最高收益 |
| keywords | varchar(255) | 项目期限 |
| description | varchar(255) | 起投金额 |
| content | text | 内容 |
| mainImg | text | 主图 |
| bannerImg | text | 视频 |

| menu_id | int(11) | 家族财富研究/行业动态/恒辉百科 |
| type | tinyint(2) | 3.家族4.行业5.百科 |
| title | varchar(100) | 文章标题 |
| top | tinyint(2) | 0.正常1.置顶 |
| small_title | varchar(100) | 原创人 |
| content | text | 内容 |
| mainImg | text | 主图 |
| bannerImg | text | 视频 |

| menu_id | int(11) | 恒慧有约 |
| type | tinyint(2) | 6.课程 |
| title | varchar(100) | 文章标题 |
| top | tinyint(2) | 0.往期1.当期 |
| small_title | varchar(100) | 专家 |
| description | varchar(255) | 简介 |
| content | text | 演讲人 |
| passage1 | text | 总结 |
| mainImg | text | 主图 |
| bannerImg | text | 视频 |

| menu_id | int(11) | 公告 |
| type | tinyint(2) | 0.无 |
| title | varchar(100) | 文章标题 |
| content | text | 内容 |



### log表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| type | int(11) | 类别:1.收藏 |
| behavior | int(11) | 1.新闻2.热销产品 |
| relation_table | varchar(100) | Article/Product |
| relation_id | varchar(100) | 关联信息ID |



### message表-留言

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| title | varchar(255) | 姓名 |
| phone | varchar(255) | 电话 |
| content | text | 内容 |



### product表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| product_no | varchar(255) | NO编号 |
| title | varchar(255) | 商品名称 |
| description | varchar(255) | 描述 |
| content | text | 详情 |
| mainImg | text | 主图 |
| bannerImg | text | banner图 |
| category_id | int(11) | 关联label表 |
| type | int(11) | 1.普通商品 |
| sale_count | int(11) | 销量 |
| recommend | tinyint(2) | 0.正常1.精品推荐 |
| hot | tinyint(2) | 0.正常1.热门 |



### sku表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| sku_no | varchar(255) | NO编号 |
| product_no | varchar(255) | 关联product表 |
| title | varchar(255) | 商品名称 |
| price | decimal(10,2) | 会员价 |
| o_price | decimal(10,2) | 原价 |
| mainImg | text | 主图 |



### order表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| order_no | varchar(255) | 订单NO |
| pay | varchar(255) | pay方式详情 |
| price | decimal(10,2) | 订单金额 |
| pay_status | tinyint(2) | -1.退款0.未支付1.已支付 |
| type | tinyint(2) | 1.普通商品,2.赠送,6.虚拟订单 |
| order_step | tinyint(2) | 0.正常下单,1.申请撤单,2.完成撤单,3.完结 |
| transport_status | tinyint(2) | 0.未发货；1.配送中；2.已收货 |
| level | tinyint(2) | 层级：1.父级订单0.子级订单 |
| parent_no | varchar(255) | 父级订单NO |
| product_id | int(11) | 商品id |
| sku_id | int(11) | SKU id |
| title | varchar(255) | 商品名称 |
| unit_price | decimal(10,2) | 商品单价 |
| count | int(11) | 商品数量 |
| passage1 | text | 退款说明 |



### order_item表

| 字段 | 类型 | 说明 |
| ------ | :------: | ------ |
| order_no | varchar(255) | 订单NO |
| product_id | int(11) | 商品id |
| snap_product | text | 商品信息快照 |

---