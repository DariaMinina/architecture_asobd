# Источник (сторонняя база)

### DDL src.clients
```
CREATE TABLE src.clients (
	client_id varchar NOT NULL,
	gender varchar NULL,
	registration_date date NULL,
	CONSTRAINT clients_pk PRIMARY KEY (client_id)
);
```
### Пример данных в src.clients

```
INSERT INTO src.clients (client_id, gender, registration_date)
VALUES 
('CL001', 'F', DATE '2023-01-15'),
('CL002', 'M', DATE '2023-03-20'),
('CL003', NULL, DATE '2023-05-10');
```

### DDL src.orders
CREATE TABLE src.orders (
	order_id varchar NOT NULL,
	client_id varchar NOT NULL,
	item_id varchar NOT NULL,
	status varchar NULL
);

### Пример данных в src.orders
```
INSERT INTO src.orders (order_id, client_id, item_id, status)
VALUES 
('ORD001', 'CL001', 'IT001', 'accepted'),
('ORD002', 'CL002', 'IT002', 'done'),
('ORD003', 'CL003', 'IT001', 'cancelled');
```

### DDL src.items

```
CREATE TABLE src.items (
	item_id varchar NOT NULL,
	item_name varchar NOT null,
	CONSTRAINT items_pk PRIMARY KEY (item_id)
);
```

### Пример данных в src.items

INSERT INTO src.items (item_id, item_name)
VALUES 
('IT001', 'Smartphone'),
('IT002', 'Laptop'),
('IT003', 'Headphones');


### DDL src.subscriptions

```
CREATE TABLE src.subscriptions (
	subscription_id varchar NOT NULL,
	client_id varchar NOT NULL,
	subsc_status varchar null,
	CONSTRAINT subscriptions_pk PRIMARY KEY (subscription_id)
);
```

### Пример данных в src.subscriptions 

```
INSERT INTO src.subscriptions (subscription_id, client_id, subsc_status)
VALUES 
('SUB001', 'CL001', 'Active'),
('SUB002', 'CL002', 'Pending'),
('SUB003', 'CL003', 'Cancelled');
```

### DDL src.favorites 

```
CREATE TABLE src.favorites (
	item_id varchar NOT NULL,
	client_id varchar NOT null
);
```

### Пример данных в src.favorites

```
INSERT INTO src.favorites (item_id, client_id)
VALUES 
('IT001', 'CL001'),
('IT002', 'CL002'),
('IT003', 'CL003');
```

### DDL src.tracker

```
CREATE TABLE src.tracker (
	track_id varchar NOT NULL,
	client_id varchar NOT NULL,
	visit_date timestamp NOT NULL,
	visit_platform varchar NULL
);
```

### Пример данных в src.tracker

```
INSERT INTO src.tracker (track_id, client_id, visit_date, visit_platform)
VALUES 
('TRK001', 'CL001', TIMESTAMP '2023-06-12 10:30:00', 'Web'),
('TRK002', 'CL002', TIMESTAMP '2023-06-13 14:15:00', 'Mobile App'),
('TRK003', 'CL003', TIMESTAMP '2023-06-14 09:45:00', 'Desktop');
```
