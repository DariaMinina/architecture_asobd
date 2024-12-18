# Staging-слой

### DDL stg.clients

```
CREATE TABLE stg.clients (
  id SERIAL PRIMARY KEY,
  client_id varchar,
  gender varchar,
  registration_date date,
  src_type varchar,
  op_ts timestamp
);
```

### Пример данных в stg.clients

```
insert stg.clients (
  client_id,
  gender,
  registration_date,
  src_type,
  op_ts
) values
('CL001', 'M', DATE '2023-01-15', 'src.clients', TIMESTAMP '2023-01-15 10:00:00'),
('CL002', 'F', DATE '2023-02-20', 'src.clients', TIMESTAMP '2023-02-20 14:30:00'),
('CL003', 'M', DATE '2023-03-25', 'src.clients', TIMESTAMP '2023-03-25 09:15:00'),
('CL004', 'F', DATE '2023-04-10', 'src.clients', TIMESTAMP '2023-04-10 18:45:00');
```

### DDL stg.orders

```
CREATE TABLE stg.orders (
  id SERIAL PRIMARY KEY,
  order_id varchar,
  client_id varchar,
  item_id varchar,
  status varchar,
  order_ts timestamp,
  src_type varchar,
  op_ts timestamp
);
```

### Пример данных в stg.orders

```
INSERT INTO stg.orders (
  order_id,
  client_id,
  item_id,
  status,
  order_ts,
  src_type,
  op_ts
) VALUES
('ORD001', 'CL001', 'ITEM001', 'pending', '2023-01-15 09:59:56', 'src.orders', TIMESTAMP '2023-01-15 10:00:00'),
('ORD002', 'CL001', 'ITEM002', 'processing', '2023-01-16 11:29:57', 'src.orders', TIMESTAMP '2023-01-16 11:30:00'),
('ORD003', 'CL002', 'ITEM003', 'done', '2023-01-17 13:44:57', 'src.orders', TIMESTAMP '2023-01-17 13:45:00'),
('ORD004', 'CL003', 'ITEM004', 'cancelled', '2023-01-18 09:19:58', 'src.orders', TIMESTAMP '2023-01-18 09:20:00');

```

### DDL stg.items

```
CREATE TABLE stg.items (
  id SERIAL PRIMARY KEY,
  item_id varchar,
  item_name varchar,
  src_type varchar,
  op_ts timestamp
);
```

### Пример данных в stg.items

```
INSERT INTO stg.items (
  item_id,
  item_name,
  src_type,
  op_ts
) VALUES
('ITEM001', 'Smartphone', 'src.items', TIMESTAMP '2023-01-15 10:00:00'),
('ITEM002', 'Laptop', 'src.items', TIMESTAMP '2023-01-16 11:30:00'),
('ITEM003', 'Tablet', 'src.items', TIMESTAMP '2023-01-17 13:45:00'),
('ITEM004', 'Headphones', 'src.items', TIMESTAMP '2023-01-18 09:20:00');
```

### DDL stg.subscriptions

```
CREATE TABLE stg.subscriptions (
  id SERIAL PRIMARY KEY,
  subscription_id varchar,
  client_id varchar,
  subsc_status varchar,
  src_type varchar,
  op_ts timestamp
);
```

### Пример данных в stg.subscriptions 

```
INSERT INTO stg.subscriptions (
  subscription_id,
  client_id,
  subsc_status,
  src_type,
  op_ts
) VALUES
('SUB001', 'CL001', 'premium', 'src.subscriptions', TIMESTAMP '2023-01-15 10:00:00'),
('SUB003', 'CL002', 'premium', 'src.subscriptions', TIMESTAMP '2023-01-17 13:45:00'),
('SUB004', 'CL003', 'premium', 'src.subscriptions', TIMESTAMP '2023-01-18 09:20:00');
```

### DDL stg.favorites 

```
CREATE TABLE stg.favorites (
  id SERIAL PRIMARY KEY,
  item_id varchar,
  client_id varchar,
  src_type varchar,
  op_ts timestamp
);
```

### Пример данных в stg.favorites

```
INSERT INTO stg.favorites (
  item_id,
  client_id,
  src_type,
  op_ts
) VALUES
('ITEM001', 'CL001', 'src.favorites', TIMESTAMP '2023-01-15 10:00:00'),
('ITEM002', 'CL001', 'src.favorites', TIMESTAMP '2023-01-16 11:30:00'),
('ITEM003', 'CL002', 'src.favorites', TIMESTAMP '2023-01-17 13:45:00'),
('ITEM001', 'CL003', 'src.favorites', TIMESTAMP '2023-01-18 09:20:00');
```

### DDL stg.tracker

```
CREATE TABLE stg.tracker (
  id SERIAL PRIMARY KEY,
  track_id varchar,
  client_id varchar,
  visit_date timestamp,
  visit_platform varchar,
  src_type varchar,
  op_ts timestamp
);
```

### Пример данных в stg.tracker

```
INSERT INTO stg.tracker (
  track_id,
  client_id,
  visit_date,
  visit_platform,
  src_type varchar,
  op_ts
) VALUES
('TRK001', 'CL001', TIMESTAMP '2023-01-15 10:00:00', 'Web', 'src.tracker', TIMESTAMP '2023-01-15 10:05:00'),
('TRK002', 'CL001', TIMESTAMP '2023-01-16 11:30:00', ''Mobile App', 'src.tracker', TIMESTAMP '2023-01-16 12:00:00'),
('TRK003', 'CL002', TIMESTAMP '2023-01-17 13:45:00', 'Desktop', 'src.tracker', TIMESTAMP '2023-01-17 14:15:00')
```
