# orders

## Description

<details>
<summary><strong>Table Definition</strong></summary>

```sql
CREATE TABLE `orders` (
  `id` bigint unsigned NOT NULL AUTO_INCREMENT,
  `user_id` bigint unsigned NOT NULL,
  `order_number` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `total_amount` decimal(10,2) NOT NULL,
  `status` enum('pending','processing','completed','cancelled') COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT 'pending',
  `shipping_address` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `shipping_city` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `shipping_country` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `shipping_phone` varchar(255) COLLATE utf8mb4_unicode_ci NOT NULL,
  `created_at` timestamp NULL DEFAULT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `orders_order_number_unique` (`order_number`),
  KEY `orders_user_id_foreign` (`user_id`),
  CONSTRAINT `orders_user_id_foreign` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci
```

</details>

## Columns

| Name             | Type                                                 | Default | Nullable | Extra Definition | Children                                              | Parents           | Comment |
| ---------------- | ---------------------------------------------------- | ------- | -------- | ---------------- | ----------------------------------------------------- | ----------------- | ------- |
| created_at       | timestamp                                            |         | true     |                  |                                                       |                   |         |
| id               | bigint unsigned                                      |         | false    | auto_increment   | [order_items](order_items.md) [payments](payments.md) |                   |         |
| order_number     | varchar(255)                                         |         | false    |                  |                                                       |                   |         |
| shipping_address | varchar(255)                                         |         | false    |                  |                                                       |                   |         |
| shipping_city    | varchar(255)                                         |         | false    |                  |                                                       |                   |         |
| shipping_country | varchar(255)                                         |         | false    |                  |                                                       |                   |         |
| shipping_phone   | varchar(255)                                         |         | false    |                  |                                                       |                   |         |
| status           | enum('pending','processing','completed','cancelled') | pending | false    |                  |                                                       |                   |         |
| total_amount     | decimal(10,2)                                        |         | false    |                  |                                                       |                   |         |
| updated_at       | timestamp                                            |         | true     |                  |                                                       |                   |         |
| user_id          | bigint unsigned                                      |         | false    |                  |                                                       | [users](users.md) |         |

## Constraints

| Name                       | Type        | Definition                                           |
| -------------------------- | ----------- | ---------------------------------------------------- |
| PRIMARY                    | PRIMARY KEY | PRIMARY KEY (id)                                     |
| orders_order_number_unique | UNIQUE      | UNIQUE KEY orders_order_number_unique (order_number) |
| orders_user_id_foreign     | FOREIGN KEY | FOREIGN KEY (user_id) REFERENCES users (id)          |

## Indexes

| Name                       | Definition                                                       |
| -------------------------- | ---------------------------------------------------------------- |
| PRIMARY                    | PRIMARY KEY (id) USING BTREE                                     |
| orders_order_number_unique | UNIQUE KEY orders_order_number_unique (order_number) USING BTREE |
| orders_user_id_foreign     | KEY orders_user_id_foreign (user_id) USING BTREE                 |

## Relations

```mermaid
erDiagram

"order_items" }o--|| "orders" : "FOREIGN KEY (order_id) REFERENCES orders (id)"
"payments" }o--|| "orders" : "FOREIGN KEY (order_id) REFERENCES orders (id)"
"orders" }o--|| "users" : "FOREIGN KEY (user_id) REFERENCES users (id)"

"orders" {
  timestamp created_at
  bigint_unsigned id PK
  varchar_255_ order_number
  varchar_255_ shipping_address
  varchar_255_ shipping_city
  varchar_255_ shipping_country
  varchar_255_ shipping_phone
  enum__pending___processing___completed___cancelled__ status
  decimal_10_2_ total_amount
  timestamp updated_at
  bigint_unsigned user_id FK
}
"order_items" {
  timestamp created_at
  bigint_unsigned id PK
  bigint_unsigned order_id FK
  bigint_unsigned product_id FK
  int quantity
  decimal_10_2_ subtotal
  decimal_10_2_ unit_price
  timestamp updated_at
}
"payments" {
  decimal_10_2_ amount
  timestamp created_at
  bigint_unsigned id PK
  bigint_unsigned order_id FK
  timestamp paid_at
  varchar_255_ payment_method
  enum__pending___completed___failed___refunded__ status
  varchar_255_ transaction_id
  timestamp updated_at
}
"users" {
  timestamp created_at
  varchar_255_ email
  timestamp email_verified_at
  bigint_unsigned id PK
  varchar_255_ name
  varchar_255_ password
  varchar_100_ remember_token
  timestamp updated_at
}
```

---

> Generated by [tbls](https://github.com/k1LoW/tbls)
