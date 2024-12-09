# Staging-слой

### DDL stg.clients

```
CREATE TABLE stg.clients (
  id int8,
  client_id varchar,
  gender varchar,
  registration_date date,
  op_ts timestamp
);
```
### Пример данных в stg.clients

```
('CL001', 'F', DATE '2023-01-15'),
('CL002', 'M', DATE '2023-03-20'),
('CL003', NULL, DATE '2023-05-10');
```
| id | client_id |  gender | registration_date | op_ts |
|---|---|---|---|---|
|   | CL001  | F |  2023-01-15 |   |
|   | CL002  | M |  2023-03-20 |   |
|   | CL003  | NULL |  2023-05-10 |   |

### DDL stg.orders

```
CREATE TABLE stg.orders (
  __connect_partition int8,
  __connect_offset int8,
	order_id varchar,
	client_id varchar,
	item_id varchar,
	status varchar,
  op_ts timestamp
);
```

### Пример данных в stg.orders
```
('ORD001', 'CL001', 'IT001', 'accepted'),
('ORD002', 'CL002', 'IT002', 'done'),
('ORD003', 'CL003', 'IT001', 'cancelled');
```

### DDL stg.items

```
CREATE TABLE stg.items (
  __connect_partition int8,
  __connect_offset int8,
	item_id varchar,
	item_name varchar,
  op_ts timestamp
);
```

### Пример данных в stg.items

```
('IT001', 'Smartphone'),
('IT002', 'Laptop'),
('IT003', 'Headphones');
```

### DDL stg.subscriptions

```
CREATE TABLE stg.subscriptions (
  __connect_partition int8,
  __connect_offset int8,
	subscription_id varchar,
	client_id varchar,
	subsc_status varchar,
  op_ts timestamp
);
```

### Пример данных в stg.subscriptions 

```
('SUB001', 'CL001', 'Active'),
('SUB002', 'CL002', 'Pending'),
('SUB003', 'CL003', 'Cancelled');
```

### DDL stg.favorites 

```
CREATE TABLE stg.favorites (
  __connect_partition int8,
  __connect_offset int8,
	item_id varchar,
	client_id varchar,
  op_ts timestamp
);
```

### Пример данных в stg.favorites

```
('IT001', 'CL001'),
('IT002', 'CL002'),
('IT003', 'CL003');
```

### DDL stg.tracker

```
CREATE TABLE stg.tracker (
  __connect_partition int8,
  __connect_offset int8,
	track_id varchar,
	client_id varchar,
	visit_date timestamp,
	visit_platform varchar,
  op_ts timestamp
);
```

### Пример данных в stg.tracker

```
('TRK001', 'CL001', TIMESTAMP '2023-06-12 10:30:00', 'Web'),
('TRK002', 'CL002', TIMESTAMP '2023-06-13 14:15:00', 'Mobile App'),
('TRK003', 'CL003', TIMESTAMP '2023-06-14 09:45:00', 'Desktop');
```
