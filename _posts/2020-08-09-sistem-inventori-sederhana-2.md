---
comments: true
---

Pada tahap ini kita akan membuat database untuk sistem inventorinya.

Sesuai gambaran dari ERD ini
<a href="/assets/images/erd-sistem-inventori.jpg" target="_blank">
    <img src="/assets/images/erd-sistem-inventori.jpg" style='width:100%;' border="0" alt="ERD">
</a>

Database ini menggunakan MySQL dan kurang lebih seperti ini:

### Tabel m_item

```
-- inventory.m_item definition

CREATE TABLE `m_item` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(250) COLLATE utf8mb4_unicode_ci NOT NULL,
  `type` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `created_by` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  `updated_by` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `m_item_FK` (`created_by`),
  KEY `m_item_FK_1` (`updated_by`),
  CONSTRAINT `m_item_FK` FOREIGN KEY (`created_by`) REFERENCES `m_user` (`username`),
  CONSTRAINT `m_item_FK_1` FOREIGN KEY (`updated_by`) REFERENCES `m_user` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=13 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### Tabel m_user

```
-- inventory.m_user definition

CREATE TABLE `m_user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(250) COLLATE utf8mb4_unicode_ci NOT NULL,
  `level` varchar(50) COLLATE utf8mb4_unicode_ci NOT NULL,
  `username` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `password` varchar(250) COLLATE utf8mb4_unicode_ci NOT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `created_by` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  `updated_by` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `m_user_UN` (`username`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### Tabel t_stock

```
-- inventory.t_stock definition

CREATE TABLE `t_stock` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `transaction_code` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `transaction_date` date NOT NULL,
  `description` text COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `created_by` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `updated_at` timestamp NULL DEFAULT NULL,
  `updated_by` varchar(100) COLLATE utf8mb4_unicode_ci DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `t_stock_FK` (`created_by`),
  KEY `t_stock_FK_1` (`updated_by`),
  CONSTRAINT `t_stock_FK` FOREIGN KEY (`created_by`) REFERENCES `m_user` (`username`),
  CONSTRAINT `t_stock_FK_1` FOREIGN KEY (`updated_by`) REFERENCES `m_user` (`username`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

### Tabel t_stock_detail

```
-- inventory.t_stock_detail definition

CREATE TABLE `t_stock_detail` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `transaction_code` varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL,
  `item_id` int(11) NOT NULL,
  `stock_in` int(11) NOT NULL DEFAULT 0,
  `stock_out` int(11) NOT NULL DEFAULT 0,
  `created_at` timestamp NOT NULL DEFAULT current_timestamp(),
  `updated_at` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `t_stock_detail_FK` (`item_id`),
  CONSTRAINT `t_stock_detail_FK` FOREIGN KEY (`item_id`) REFERENCES `m_item` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```
...

Selanjutnya kita mulai buatkan tampilan webnya.
<a href="/2020/08/11/sistem-inventori-sederhana-3.html">Selanjutnya</a>
