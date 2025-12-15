Table Schema

1. 建立 e_commerce 資料庫
```sql
CREATE DATABASE e_commerce;
```

2. 建立 order_sa 使用者並設定密碼
```sql
CREATE USER e_commerce_sa WITH PASSWORD 'e_commerce_sa';
```

3. 授予 e_commerce_sa 使用者對 e_commerce_sa 資料庫的完整權限
```sql
GRANT ALL PRIVILEGES ON DATABASE e_commerce TO e_commerce_sa;
```

4. 建立訂單狀態 ENUM 類型
```sql
CREATE TYPE order_status AS ENUM ('PENDING', 'PAID', 'SHIPPED', 'CANCELLED', 'REFUNDED');
```

5. User
```sql
CREATE TABLE users (
	user_id SERIAL PRIMARY KEY,                        	-- 自動遞增
	display_name VARCHAR(20) NOT null,                 	-- user display name
	email VARCHAR(100) NOT NULL UNIQUE					-- Email
);
```

6. 商鋪資訊 (Stores)
```sql
create table stores (
	store_id SERIAL PRIMARY KEY,                        -- 自動遞增
	name VARCHAR(50) NOT NULL unique,                   -- 店鋪名稱
	description text,				                    -- 店鋪描述
	country VARCHAR(50) NOT NULL	                    -- 國家
);
```

7. 商品資訊 (Products)
```sql
CREATE TABLE products (
	product_id SERIAL PRIMARY KEY,                         -- 自動遞增
	name VARCHAR(50) NOT NULL unique,                      -- 產品名稱
	description text,				                       -- 產品描述
	store_id INTEGER NOT NULL references stores(store_id), --外鍵：連結至商店資訊
	sku VARCHAR(50) NOT NULL                               -- 商品 SKU)
);
```

8. 訂單表頭 (Orders)
```sql
CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,                        -- 自動遞增
    order_no VARCHAR(20) NOT NULL UNIQUE,               -- 訂單編號
    order_date DATE NOT NULL,                           -- 訂單日期
    user_id INTEGER NOT NULL references users(user_id), -- 客戶名稱
    total_amount NUMERIC(10, 2) NOT NULL,               -- 訂單總金額
    status order_status NOT NULL,                       -- 使用 ENUM 類型
    created_at TIMESTAMP WITHOUT TIME ZONE DEFAULT NOW(),
    updated_at TIMESTAMP WITHOUT TIME ZONE DEFAULT NOW()
);
```
9. 訂單明細 (Order Details)
```sql
CREATE TABLE order_details (
    detail_id SERIAL PRIMARY KEY,                          			-- 自動遞增
    order_id INTEGER NOT NULL REFERENCES orders(order_id), 			-- 外鍵：連結至訂單表頭
    product_id INTEGER NOT NULL references products(product_id),	-- 外鍵：連結至商品資訊
    product_name_snapshot VARCHAR(50) NOT NULL,						-- sanpshot product name
    sku_snapshot VARCHAR(50) NOT NULL,								-- snapshot sku
    quantity INTEGER NOT NULL,                          			-- 數量
    unit_price NUMERIC(10, 2) NOT NULL,                 			-- 單價
    subtotal NUMERIC(10, 2) NOT NULL,                   			-- 小計
    UNIQUE (order_id, product_id)
);
```