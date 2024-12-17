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
