# Скрипты заполнения DDS-слоя из STG-слоя

## HUBS

### Hub for Clients

```
INSERT INTO dds.hub_clients (client_id, load_ts)
SELECT cl.client_id as client_id, current_timestamp() as load_ts
FROM stg.clients cl
ON CONFLICT (cl.client_id) DO UPDATE SET
    client_id = EXCLUDED.client_id,
    client_id = EXCLUDED.load_ts
WHERE hub_clients.load_ts < EXCLUDED.load_ts;
```

### Hub for Items

```
INSERT INTO dds.hub_items (item_id, load_ts)
SELECT ii.item_id as item_id, current_timestamp() as load_ts
FROM stg.items ii
ON CONFLICT (ii.item_id) DO UPDATE SET
    item_id = EXCLUDED.item_id,
    load_ts = EXCLUDED.load_ts
WHERE hub_items.load_ts < EXCLUDED.load_ts;
```

### Hub for Orders

```
INSERT INTO dds.hub_orders (order_id, load_ts)
SELECT ord.order_id as order_id, current_timestamp() as load_ts
FROM stg.orders ord
ON CONFLICT (ord.order_id) DO UPDATE SET
    order_id = EXCLUDED.order_id,
    load_ts = EXCLUDED.load_ts
WHERE hub_orders.load_ts < EXCLUDED.load_ts;
```

### Hub for Subscriptions

```
INSERT INTO dds.hub_subscriptions (subscription_id, load_ts)
SELECT sub.subscription_id as subscription_id, current_timestamp() as load_ts
FROM stg.subscriptions sub
ON CONFLICT (sub.subscription_id) DO UPDATE SET
    subscription_id = EXCLUDED.subscription_id,
    load_ts = EXCLUDED.load_ts
WHERE hub_subscriptions.load_ts < EXCLUDED.load_ts;
```

### Hub for Tracker

```
INSERT INTO dds.hub_tracker (track_id, load_ts)
SELECT tr.track_id as track_id, current_timestamp() as load_ts
FROM stg.tracker tr
ON CONFLICT (tr.track_id) DO UPDATE SET
    track_id = EXCLUDED.track_id,
    load_ts = EXCLUDED.load_ts
WHERE hub_tracker.load_ts < EXCLUDED.load_ts;
```


## LINKS

### Link between Clients and Orders

```
INSERT INTO dds.link_client_order (client_id, order_id, load_ts)
SELECT ord.client_id as client_id, ord.order_id as order_id, current_timestamp() as load_ts
FROM stg.orders ord
ON CONFLICT (ord.client_id, ord.order_id) DO NOTHING;
```

### Link between Clients and Subscriptions

```
INSERT INTO dds.link_client_subscription (client_id, subscription_id, load_ts)
SELECT sub.client_id as client_id, sub.subsription_id as subsription_id, current_timestamp() as load_ts
FROM stg.subscriptions sub
ON CONFLICT (sub.client_id, sub.subsription_id) DO NOTHING;
```

### Link between Clients and Items (Favorites)

```
INSERT INTO dds.link_client_favorites (client_id, item_id, load_ts)
SELECT fav.client_id as client_id, fav.item_id as item_id, current_timestamp() as load_ts
FROM stg.favorites fav
ON CONFLICT (fav.client_id, fav.item_id) DO NOTHING;
```

### Link between Clients and Tracker (Visits)

```
INSERT INTO dds.link_client_tracker (client_id, track_id, load_ts)
SELECT tr.client_id as client_id, tr.track_id as track_id, current_timestamp() as load_ts
FROM stg.tracker tr
ON CONFLICT (tr.client_id, tr.track_id) DO NOTHING;
```


## SATELLITES

### Satellite for Client Details

```
INSERT INTO dds.sat_client_details (client_id, gender, registration_date, src_type, op_ts, load_ts) 
SELECT distinct client_id, gender, registration_date, src_type, op_ts, current_timestamp() as load_ts 
FROM stg.orders
ON CONFLICT (client_id, load_ts) DO UPDATE SET
    gender = EXCLUDED.gender,
    registration_date = EXCLUDED.registration_date,
    src_type = EXCLUDED.src_type,
    op_ts = EXCLUDED.op_ts,
    load_ts = EXCLUDED.load_ts
WHERE sat_client_details.load_ts < EXCLUDED.load_ts;
```

### Satellite for Order Details

```
INSERT INTO dds.sat_order_details (order_id, status, order_ts, src_type, op_ts, load_ts)
SELECT distinct order_id, status, order_ts, src_type, op_ts, current_timestamp() as load_ts 
FROM stg.orders
ON CONFLICT (order_id, load_ts) DO UPDATE SET
    order_ts = EXCLUDED.order_ts,
    status = EXCLUDED.status,
    src_type = EXCLUDED.src_type,
    op_ts = EXCLUDED.op_ts,
    load_ts = EXCLUDED.load_ts
WHERE sat_order_details.load_ts > EXCLUDED.load_ts;
```

### Satellite for Item Details

```
INSERT INTO dds.sat_item_details (item_id, item_name, src_type, op_ts, load_ts) 
SELECT distinct item_id, item_name, src_type, op_ts, current_timestamp() as load_ts 
FROM stg.items
ON CONFLICT (item_id, load_ts) DO UPDATE SET
    item_name = EXCLUDED.item_name,
    src_type = EXCLUDED.src_type,
    op_ts = EXCLUDED.op_ts,
    load_ts = EXCLUDED.load_ts
WHERE sat_item_details.load_ts < EXCLUDED.load_ts;
```

### Satellite for Subscription Details

```
INSERT INTO dds.sat_subscription_details (subscription_id, subsc_status, src_type, op_ts, load_ts) 
SELECT distinct subscription_id, subsc_status, src_type, op_ts, current_timestamp() as load_ts 
FROM stg.subscriptions
ON CONFLICT (subscription_id, load_ts) DO UPDATE SET
    subsc_status = EXCLUDED.subsc_status,
    src_type = EXCLUDED.src_type,
    op_ts = EXCLUDED.op_ts,
    load_ts = EXCLUDED.load_ts
WHERE sat_subscription_details.load_ts < EXCLUDED.load_ts;
```

### Satellite for Tracker Details

```
INSERT INTO dds.sat_tracker_details (track_id, visit_date, visit_platform, src_type, op_ts, load_ts)
SELECT distinct track_id, visit_date, visit_platform, src_type, op_ts, current_timestamp() as load_ts 
FROM stg.tracker
ON CONFLICT (track_id, load_ts) DO UPDATE SET
    visit_date = EXCLUDED.visit_date,
    visit_platform = EXCLUDED.visit_platform,
    src_type = EXCLUDED.src_type,
    op_ts = EXCLUDED.op_ts,
    load_ts = EXCLUDED.load_ts
WHERE sat_tracker_details.load_ts < EXCLUDED.load_ts; 
```
