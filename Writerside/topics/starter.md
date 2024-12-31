# 商店
```sql
CREATE TABLE Shop (
    shop_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '商店的唯一标识符',
    name VARCHAR(255) NOT NULL COMMENT '商店名称',
    description TEXT COMMENT '商店描述'
);

CREATE TABLE ShopCategory (
    category_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '商店分类的唯一标识符',
    shop_id INT COMMENT '关联的商店ID',
    name VARCHAR(255) NOT NULL COMMENT '分类名称',
    FOREIGN KEY (shop_id) REFERENCES Shop(shop_id)
);

CREATE TABLE Product (
    product_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '商品的唯一标识符',
    shop_id INT COMMENT '关联的商店ID',
    name VARCHAR(255) NOT NULL COMMENT '商品名称',
    description TEXT COMMENT '商品描述',
    FOREIGN KEY (shop_id) REFERENCES Shop(shop_id)
);
CREATE TABLE StockKeepingUnit (
    sku_id INT PRIMARY KEY AUTO_INCREMENT COMMENT 'SKU的唯一标识符',
    product_id INT COMMENT '关联的商品ID',
    specification_id INT COMMENT '关联的商品规格ID',
    sku_code VARCHAR(255) UNIQUE NOT NULL COMMENT '唯一的SKU代码',
    price DECIMAL(10, 2) NOT NULL COMMENT '商品价格',
    stock_quantity INT NOT NULL COMMENT '库存数量',
    FOREIGN KEY (product_id) REFERENCES Product(product_id),
    FOREIGN KEY (specification_id) REFERENCES ProductSpecification(specification_id)
);
CREATE TABLE ProductCategory (
    category_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '商品分类的唯一标识符',
    name VARCHAR(255) NOT NULL COMMENT '分类名称'
);

CREATE TABLE ProductSpecification (
    specification_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '商品规格的唯一标识符',
    product_id INT COMMENT '关联的商品ID',
    name VARCHAR(255) NOT NULL COMMENT '规格名称（例如：颜色、尺寸）',
    value VARCHAR(255) NOT NULL COMMENT '规格值（例如：红色、XL）',
    FOREIGN KEY (product_id) REFERENCES Product(product_id)
);

CREATE TABLE ProductDetail (
    detail_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '商品详情的唯一标识符',
    product_id INT COMMENT '关联的商品ID',
    detail_key VARCHAR(255) NOT NULL COMMENT '详情键',
    detail_value TEXT NOT NULL COMMENT '详情值',
    FOREIGN KEY (product_id) REFERENCES Product(product_id)
);

CREATE TABLE ProductCoupon (
    coupon_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '商品优惠券的唯一标识符',
    product_id INT COMMENT '关联的商品ID',
    coupon_type_id INT COMMENT '关联的优惠券类型ID',
    discount_amount DECIMAL(10, 2) COMMENT '折扣金额',
    discount_percentage DECIMAL(5, 2) COMMENT '折扣百分比',
    start_date DATE COMMENT '优惠券开始日期',
    end_date DATE COMMENT '优惠券结束日期',
    FOREIGN KEY (product_id) REFERENCES Product(product_id),
    FOREIGN KEY (coupon_type_id) REFERENCES CouponType(coupon_type_id)
);

CREATE TABLE CouponType (
    coupon_type_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '优惠券类型的唯一标识符',
    name VARCHAR(255) NOT NULL COMMENT '优惠券类型名称'
);

CREATE TABLE ShoppingCart (
    cart_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '购物车的唯一标识符',
    user_id INT NOT NULL COMMENT '关联的用户ID'
);

CREATE TABLE ShoppingCartItem (
    item_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '购物车项的唯一标识符',
    cart_id INT COMMENT '关联的购物车ID',
    sku_id INT COMMENT '关联的SKU ID',
    quantity INT NOT NULL COMMENT '商品数量',
    FOREIGN KEY (cart_id) REFERENCES ShoppingCart(cart_id),
    FOREIGN KEY (sku_id) REFERENCES StockKeepingUnit(sku_id)
);

CREATE TABLE Order (
    order_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '订单的唯一标识符',
    user_id INT NOT NULL COMMENT '关联的用户ID',
    address_id INT COMMENT '关联的收货地址ID',
    order_date DATETIME NOT NULL COMMENT '订单日期',
    total_amount DECIMAL(10, 2) NOT NULL COMMENT '订单总金额',
    FOREIGN KEY (address_id) REFERENCES Address(address_id)
);

CREATE TABLE OrderDetail (
    detail_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '订单详情的唯一标识符',
    order_id INT COMMENT '关联的订单ID',
    sku_id INT COMMENT '关联的SKU ID',
    quantity INT NOT NULL COMMENT '商品数量',
    price DECIMAL(10, 2) NOT NULL COMMENT '商品价格',
    FOREIGN KEY (order_id) REFERENCES Order(order_id),
    FOREIGN KEY (sku_id) REFERENCES StockKeepingUnit(sku_id)
);

CREATE TABLE OrderCoupon (
    order_coupon_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '订单优惠券的唯一标识符',
    order_id INT COMMENT '关联的订单ID',
    coupon_id INT COMMENT '关联的商品优惠券ID',
    discount_amount DECIMAL(10, 2) COMMENT '折扣金额',
    FOREIGN KEY (order_id) REFERENCES Order(order_id),
    FOREIGN KEY (coupon_id) REFERENCES ProductCoupon(coupon_id)
);

CREATE TABLE Address (
    address_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '收货地址的唯一标识符',
    user_id INT NOT NULL COMMENT '关联的用户ID',
    address_line1 VARCHAR(255) NOT NULL COMMENT '地址第一行',
    address_line2 VARCHAR(255) COMMENT '地址第二行',
    city VARCHAR(100) NOT NULL COMMENT '城市',
    state VARCHAR(100) NOT NULL COMMENT '州/省',
    postal_code VARCHAR(20) NOT NULL COMMENT '邮政编码',
    country VARCHAR(100) NOT NULL COMMENT '国家'
);
CREATE TABLE Payment (
    payment_id INT PRIMARY KEY AUTO_INCREMENT COMMENT '支付记录的唯一标识符',
    order_id INT NOT NULL COMMENT '关联的订单ID',
    user_id INT NOT NULL COMMENT '关联的用户ID',
    payment_method VARCHAR(50) NOT NULL COMMENT '支付方式（例如：信用卡、支付宝、微信等）',
    payment_status VARCHAR(50) NOT NULL COMMENT '支付状态（例如：待支付、已支付、支付失败等）',
    amount DECIMAL(10, 2) NOT NULL COMMENT '支付金额',
    transaction_id VARCHAR(255) UNIQUE COMMENT '支付平台交易号',
    payment_date DATETIME NOT NULL COMMENT '支付日期',
    FOREIGN KEY (order_id) REFERENCES Order(order_id),
    FOREIGN KEY (user_id) REFERENCES User(user_id)
);


```
商店相关
商店（Shop）
商店分类（ShopCategory）
商品相关
商品（Product）
商品规格（ProductSpecification）
商品详情（ProductDetail）
商品分类（ProductCategory）
商品库存（ProductStock）
SKU（StockKeepingUnit）
优惠券相关
商品优惠券（ProductCoupon）
优惠券类型（CouponType）
购物车相关
购物车（ShoppingCart）
购物车项（ShoppingCartItem）
订单相关
订单（Order）
订单详情（OrderDetail）
订单优惠券（OrderCoupon）
收货地址
收货地址（Address）
```Mermaid
erDiagram
    Shop {
        int shop_id PK "商店的唯一标识符"
        varchar(255) name "商店名称"
        text description "商店描述"
    }

    ShopCategory {
        int category_id PK "商店分类的唯一标识符"
        int shop_id FK "关联的商店ID"
        varchar(255) name "分类名称"
    }

    Product {
        int product_id PK "商品的唯一标识符"
        int shop_id FK "关联的商店ID"
        varchar(255) name "商品名称"
        text description "商品描述"
    }

    StockKeepingUnit {
        int sku_id PK "SKU的唯一标识符"
        int product_id FK "关联的商品ID"
        int specification_id FK "关联的商品规格ID"
        varchar(255) sku_code UK "唯一的SKU代码"
        decimal(10) price "商品价格"
        int stock_quantity "库存数量"
    }

    ProductCategory {
        int category_id PK "商品分类的唯一标识符"
        varchar(255) name "分类名称"
    }
    
    ProductSpecification {
        int specification_id PK "商品规格的唯一标识符"
        int product_id FK "关联的商品ID"
        varchar(255) name "规格名称（例如：颜色、尺寸）"
        varchar(255) value "规格值（例如：红色、XL）"
    }
    
    ProductDetail {
        int detail_id PK "商品详情的唯一标识符"
        int product_id FK "关联的商品ID"
        varchar(255) detail_key "详情键"
        text detail_value "详情值"
    }
    
    ProductCoupon {
        int coupon_id PK "商品优惠券的唯一标识符"
        int product_id FK "关联的商品ID"
        int coupon_type_id FK "关联的优惠券类型ID"
        decimal(10) discount_amount "折扣金额"
        decimal(5) discount_percentage "折扣百分比"
        date start_date "优惠券开始日期"
        date end_date "优惠券结束日期"
    }
    
    CouponType {
        int coupon_type_id PK "优惠券类型的唯一标识符"
        varchar(255) name "优惠券类型名称"
    }
    
    ShoppingCart {
        int cart_id PK "购物车的唯一标识符"
        int user_id "关联的用户ID"
    }
    
    ShoppingCartItem {
        int item_id PK "购物车项的唯一标识符"
        int cart_id FK "关联的购物车ID"
        int sku_id FK "关联的SKU ID"
        int quantity "商品数量"
    }
    
    Order {
        int order_id PK "订单的唯一标识符"
        int user_id "关联的用户ID"
        int address_id FK "关联的收货地址ID"
        datetime order_date "订单日期"
        decimal(10) total_amount "订单总金额"
    }
    
    OrderDetail {
        int detail_id PK "订单详情的唯一标识符"
        int order_id FK "关联的订单ID"
        int sku_id FK "关联的SKU ID"
        int quantity "商品数量"
        decimal(10) price "商品价格"
    }
    
    OrderCoupon {
        int order_coupon_id PK "订单优惠券的唯一标识符"
        int order_id FK "关联的订单ID"
        int coupon_id FK "关联的商品优惠券ID"
        decimal(10) discount_amount "折扣金额" 
    }
    
    Address {
        int address_id PK "收货地址的唯一标识符"
        int user_id "关联的用户ID"
        varchar(255) address_line1 "地址第一行"
        varchar(255) address_line2 "地址第二行"
        varchar(100) city "城市"
        varchar(100) state "州/省"
        varchar(20) postal_code "邮政编码"
        varchar(100) country "国家"
    }
    
    Payment {
        int payment_id PK "支付记录的唯一标识符"
        int order_id FK "关联的订单ID"
        int user_id FK "关联的用户ID"
        varchar(50) payment_method "支付方式（例如：信用卡、支付宝、微信等）"
        varchar(50) payment_status "支付状态（例如：待支付、已支付、支付失败等）"
        decimal(10) amount "支付金额"
        varchar(255) transaction_id UK "支付平台交易号"
        datetime payment_date "支付日期"
    }

Shop ||--o{ ShopCategory : "包含"
Shop ||--o{ Product : "销售"
Product ||--o{ StockKeepingUnit : "拥有"
Product ||--o{ ProductSpecification : "具有"
Product ||--o{ ProductDetail : "详细信息"
Product ||--o{ ProductCoupon : "提供"
ShoppingCart ||--o{ ShoppingCartItem : "包含"
StockKeepingUnit ||--o{ OrderDetail : "属于"
Order ||--o{ OrderDetail : "包含"
Order ||--o{ OrderCoupon : "使用"
Address ||--|| Order : "关联"
Order ||--o{ Payment : "支付"


```
```rest
### Shop 相关接口

#### 获取所有商店
- **URL**: `/api/shops`
- **Method**: `GET`
- **Description**: 获取所有商店的信息。
- **Response**:

```
