# DDS-слой

# DDL-скрипты

## HUBS

### Hub for Clients

```
CREATE TABLE dds.hub_clients (
client_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP
);
```

### Hub for Items

```
CREATE TABLE dds.hub_items (
item_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP
);
```

### Hub for Orders

```
CREATE TABLE dds.hub_orders (
order_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP
);
```

### Hub for Subscriptions

```
CREATE TABLE dds.hub_subscriptions (
subscription_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP
);
```

### Hub for Tracker

```
CREATE TABLE dds.hub_tracker (
track_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP
);
```

## LINKS

### Link between Clients and Orders

```
CREATE TABLE dds.link_client_order (
client_id VARCHAR,
order_id VARCHAR,
load_ts TIMESTAMP,
PRIMARY KEY (client_id, order_id)
);
```

### Link between Clients and Subscriptions

```
CREATE TABLE dds.link_client_subscription (
client_id VARCHAR,
subscription_id VARCHAR,
load_ts TIMESTAMP,
PRIMARY KEY (client_id, subscription_id)
);
```

### Link between Clients and Items (Favorites)

```
CREATE TABLE dds.link_client_favorites (
client_id VARCHAR,
item_id VARCHAR,
load_ts TIMESTAMP,
PRIMARY KEY (client_id, item_id)
);
```

### Link between Clients and Tracker (Visits)

```
CREATE TABLE dds.link_client_tracker (
client_id VARCHAR,
track_id VARCHAR,
load_ts TIMESTAMP,
PRIMARY KEY (client_id, track_id)
);
```

## SATELLITES

### Satellite for Client Details (e.g., gender, registration date)

```
CREATE TABLE dds.sat_client_details (
client_id VARCHAR,
gender VARCHAR,
registration_date DATE,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
PRIMARY KEY (client_id, load_ts)
);
```

### Satellite for Order Details (e.g., status, order dates)

```
CREATE TABLE dds.sat_order_details (
order_id VARCHAR,
status VARCHAR,
order_date DATE,
accepted BOOLEAN,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
PRIMARY KEY (order_id, load_ts)
);
```

### Satellite for Item Details (e.g., item name)

```
CREATE TABLE dds.sat_item_details (
item_id VARCHAR,
item_name VARCHAR,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
PRIMARY KEY (item_id, load_ts)
);
```

### Satellite for Subscription Details (e.g., subscription status)

```
CREATE TABLE dds.sat_subscription_details (
subscription_id VARCHAR,
subsc_status VARCHAR,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
PRIMARY KEY (subscription_id, load_ts)
);
```

### Satellite for Tracker Details (e.g., visit date, platform)

```
CREATE TABLE dds.sat_tracker_details (
track_id VARCHAR,
visit_date TIMESTAMP,
visit_platform VARCHAR,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
PRIMARY KEY (track_id, load_ts)
);
```

# Примеры данных

## HUBS

### Hub for Clients

```
INSERT INTO dds.hub_clients (client_id, load_ts) VALUES 
('CL001', '2024-12-17 00:00:00'),
('CL002', '2024-12-17 00:00:00'),
('CL003', '2024-12-17 00:00:00'),
('CL004', '2024-12-17 00:00:00');
```

### Hub for Items

```
INSERT INTO dds.hub_items (item_id, load_ts) VALUES 
('ITEM001', '2024-12-17 00:00:00'),
('ITEM002', '2024-12-17 00:00:00'),
('ITEM003', '2024-12-17 00:00:00'),
('ITEM004', '2024-12-17 00:00:00');
```

### Hub for Orders

```
INSERT INTO dds.hub_orders (order_id, load_ts) VALUES 
('ORD001', '2024-12-17 00:00:00'),
('ORD002', '2024-12-17 00:00:00'),
('ORD003', '2024-12-17 00:00:00'),
('ORD004', '2024-12-17 00:00:00');
```

### Hub for Subscriptions

```
INSERT INTO dds.hub_subscriptions (subscription_id, load_ts) VALUES 
('SUB001', '2024-12-17 00:00:00'),
('SUB003', '2024-12-17 00:00:00'),
('SUB004', '2024-12-17 00:00:00');
```

### Hub for Tracker

```
INSERT INTO dds.hub_tracker (track_id, load_ts) VALUES 
('TRK001', '2024-12-17 00:00:00'),
('TRK002', '2024-12-17 00:00:00'),
('TRK003', '2024-12-17 00:00:00');
```

## LINKS

### Link between Clients and Orders

```
INSERT INTO dds.link_client_order (client_id, order_id, load_ts) VALUES 
('CL001', 'OR001', '2024-12-17 00:00:00'),
('CL001', 'OR002', '2024-12-17 00:00:00'),
('CL002', 'OR003', '2024-12-17 00:00:00'),
('CL003', 'OR004', '2024-12-17 00:00:00');
```

### Link between Clients and Subscriptions  

```
INSERT INTO dds.link_client_subscription (client_id, subscription_id, load_ts) VALUES 
('CL001', 'SUB001', '2024-12-17 00:00:00'),
('CL002', 'SUB003', '2024-12-17 00:00:00'),
('CL003', 'SUB004', '2024-12-17 00:00:00');
```

### Link between Clients and Items (Favorites)

```
INSERT INTO dds.link_client_favorites (client_id, item_id, load_ts) VALUES 
('CL001', 'ITEM001', '2024-12-17 00:00:00'),
('CL001', 'ITEM002', '2024-12-17 00:00:00'),
('CL002', 'ITEM003', '2024-12-17 00:00:00'),
('CL003', 'ITEM001', '2024-12-17 00:00:00');
```

### Link between Clients and Tracker (Visits)

```
INSERT INTO dds.link_client_tracker (client_id, track_id, load_ts) VALUES 
('CL001', 'TRK001', '2024-12-17 00:00:00'),
('CL001', 'TRK002', '2024-12-17 00:00:00'),
('CL002', 'TRK003', '2024-12-17 00:00:00');
```

## SATELLITES

### Satellite for Client Details

```
INSERT INTO dds.sat_client_details (client_id, gender, registration_date, src_type, op_ts, load_ts) VALUES 
('CL002', 'F', DATE '2023-02-20', 'src.clients', TIMESTAMP '2023-02-20 14:30:00', '2024-12-17 00:00:00'),
('CL003', 'M', DATE '2023-03-25', 'src.clients', TIMESTAMP '2023-03-25 09:15:00', '2024-12-17 00:00:00'),
('CL004', 'F', DATE '2023-04-10', 'src.clients', TIMESTAMP '2023-04-10 18:45:00', '2024-12-17 00:00:00');
```

### Satellite for Order Details

```
INSERT INTO dds.sat_order_details (order_id, status, order_date, accepted, src_type, op_ts, load_ts) VALUES 
('ORD001', 'pending', '2023-01-15 09:50:00', TRUE, 'src.orders', TIMESTAMP '2023-01-15 10:00:00', '2024-12-17 00:00:00'),
('ORD002', 'processing', '2023-01-16 11:29:56', TRUE, 'src.orders', TIMESTAMP '2023-01-16 11:30:00', '2024-12-17 00:00:00'),
('ORD003', 'done', '2023-01-17 13:44:58', TRUE, 'src.orders', TIMESTAMP '2023-01-17 13:45:00', '2024-12-17 00:00:00'),
('ORD004', 'cancelled', '2023-01-18 09:00:00', FALSE, 'src.orders', TIMESTAMP '2023-01-18 09:20:00', '2024-12-17 00:00:00');
```

### Satellite for Item Details

```
INSERT INTO dds.sat_item_details (item_id, item_name, src_type, op_ts, load_ts) VALUES 
('ITEM001', 'Smartphone', 'src.items', TIMESTAMP '2023-01-15 10:00:00', '2024-12-17 00:00:00'),
('ITEM002', 'Laptop', 'src.items', TIMESTAMP '2023-01-16 11:30:00', '2024-12-17 00:00:00'),
('ITEM003', 'Tablet', 'src.items', TIMESTAMP '2023-01-17 13:45:00', '2024-12-17 00:00:00'),
('ITEM004', 'Headphones', 'src.items', TIMESTAMP '2023-01-18 09:20:00', '2024-12-17 00:00:00');
```

### Satellite for Subscription Details

```
INSERT INTO dds.sat_subscription_details (subscription_id, subsc_status, src_type, op_ts, load_ts) VALUES 
('SUB001', 'premium', 'src.subscriptions', TIMESTAMP '2023-01-15 10:00:00', '2024-12-17 00:00:00'),
('SUB003', 'premium', 'src.subscriptions', TIMESTAMP '2023-01-17 13:45:00', '2024-12-17 00:00:00'),
('SUB004', 'premium', 'src.subscriptions', TIMESTAMP '2023-01-18 09:20:00', '2024-12-17 00:00:00');
```

### Satellite for Tracker Details

```
INSERT INTO dds.sat_tracker_details (track_id, visit_date, visit_platform, src_type, op_ts, load_ts) VALUES 
('TRK001', TIMESTAMP '2023-01-15 10:00:00', 'Web', 'src.tracker', TIMESTAMP '2023-01-15 10:05:00', '2024-12-17 00:00:00'),
('TRK002', TIMESTAMP '2023-01-16 11:30:00', ''Mobile App', 'src.tracker', TIMESTAMP '2023-01-16 12:00:00', '2024-12-17 00:00:00'),
('TRK003', TIMESTAMP '2023-01-17 13:45:00', 'Desktop', 'src.tracker', TIMESTAMP '2023-01-17 14:15:00', '2024-12-17 00:00:00');
```
