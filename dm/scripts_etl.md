- Insert cliend_id and report date
```
INSERT INTO result_data_mart (
    client_id, 
    report_date
)
SELECT
    DISTINCT cl.client_id,
    CURRENT_DATE AS report_date
FROM 
    dds.hub_clients cl;
```

- Gender
```
UPDATE result_data_mart rdm
SET 
    gender = subquery.gender
FROM (
    SELECT
        cl.client_id,
        scd.gender
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.sat_client_details scd ON cl.client_id = scd.client_id
    WHERE scd.load_ts = (
        SELECT MAX(load_ts) 
        FROM dds.sat_client_details 
        WHERE client_id = cl.client_id
    )
) subquery
WHERE rdm.client_id = subquery.client_id;
```

- Registration date
```
UPDATE result_data_mart rdm
SET 
    registration_date = subquery.registration_date
FROM (
    SELECT
        cl.client_id,
        -- Get the registration date from the most recent record in the satellite
        sd.registration_date
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.sat_client_details sd ON cl.client_id = sd.client_id
    WHERE 
        sd.load_ts = (
            SELECT MAX(load_ts)
            FROM dds.sat_client_details 
            WHERE client_id = cl.client_id
        )
) subquery
WHERE rdm.client_id = subquery.client_id;
```

- First and last order dates
```
UPDATE result_data_mart rdm
SET 
    first_order_date = subquery.first_order_date,
    last_order_date = subquery.last_order_date
FROM (
    SELECT
        cl.client_id,
        MIN(sod.order_ts) FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'accepted'
            )
        ) AS first_order_date,
        MAX(sod.order_ts) FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'accepted'
            )
        ) AS last_order_date
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.link_client_order lco ON cl.client_id = lco.client_id
    LEFT JOIN 
        dds.sat_order_details sod ON lco.order_id = sod.order_id
) subquery
WHERE rdm.client_id = subquery.client_id;
```

- First and last visit dates, last visit platform
```
UPDATE result_data_mart rdm
SET 
    first_visit_date = subquery.first_visit_date,
    last_visit_date = subquery.last_visit_date,
    last_visit_platform = subquery.last_visit_platform
FROM (
    SELECT
        cl.client_id,
        MIN(std_actual.visit_date) AS first_visit_date,
        MAX(std_actual.visit_date) AS last_visit_date,
        (SELECT std_actual_inner.visit_platform
         FROM dds.sat_tracker_details std_actual_inner
         WHERE std_actual_inner.client_id = cl.client_id
         AND std_actual_inner.visit_date = MAX(std_actual.visit_date)
         LIMIT 1) AS last_visit_platform
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.link_client_tracker lct ON cl.client_id = lct.client_id
    LEFT JOIN 
        dds.sat_tracker_details std_actual ON lct.track_id = std_actual.track_id
    GROUP BY 
        cl.client_id
) subquery
WHERE rdm.client_id = subquery.client_id;
```

- Premium
```
UPDATE result_data_mart rdm
SET 
    has_premium = subquery.premium
FROM (
    SELECT
        cl.client_id,
        CASE
            WHEN sd.subsc_status = 'active' THEN TRUE
            ELSE FALSE
        END AS premium
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.link_client_subscription lcs ON cl.client_id = lcs.client_id
    LEFT JOIN 
        dds.sat_subscription_details sd ON lcs.subscription_id = sd.subscription_id
) subquery
WHERE rdm.client_id = subquery.client_id;
```

- Orders for last n days
```
UPDATE result_data_mart rdm
SET 
    orders_last_7_days = subquery.orders_last_7_days,
    orders_last_30_days = subquery.orders_last_30_days,
    orders_last_365_days = subquery.orders_last_365_days,
    total_orders = subquery.total_orders
FROM (
    SELECT
        cl.client_id,
        COUNT(sod.order_id) FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'accepted'
            ) AND sod.order_ts >= CURRENT_DATE - INTERVAL '7 DAYS'
        ) AS orders_last_7_days,
        COUNT(sod.order_id) FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'accepted'
            ) AND sod.order_ts >= CURRENT_DATE - INTERVAL '30 DAYS'
        ) AS orders_last_30_days,
        COUNT(sod.order_id) FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'accepted'
            ) AND sod.order_ts >= CURRENT_DATE - INTERVAL '365 DAYS'
        ) AS orders_last_365_days,
        COUNT(sod.order_id) FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'accepted'
            )
        ) AS total_orders
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.link_client_order lco ON cl.client_id = lco.client_id
    LEFT JOIN 
        dds.sat_order_details sod ON lco.order_id = sod.order_id
) subquery
WHERE rdm.client_id = subquery.client_id;
```

- GMV for the last n days
```
UPDATE result_data_mart rdm
SET 
    gmv_last_7_days = subquery.gmv_last_7_days,
    gmv_last_30_days = subquery.gmv_last_30_days,
    gmv_last_365_days = subquery.gmv_last_365_days,
    total_gmv = subquery.total_gmv
FROM (
    SELECT
        cl.client_id,
        SUM(
          SELECT sid_actual.item_price 
          FROM dds.sat_item_details sid_actual
          WHERE sid_actual.item_id = soi.item_id
          AND sid_actual.load_ts <= sod.order_ts
          ORDER BY sid_actual.load_ts DESC
          LIMIT 1)
    FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'done'
            ) AND sod.order_ts >= CURRENT_DATE - INTERVAL '7 DAYS'
        ) AS gmv_last_7_days,
        SUM(
          SELECT sid_actual.item_price 
          FROM dds.sat_item_details sid_actual
          WHERE sid_actual.item_id = soi.item_id
          AND sid_actual.load_ts <= sod.order_ts
          ORDER BY sid_actual.load_ts DESC
          LIMIT 1)
        FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'done'
            ) AND sod.order_ts >= CURRENT_DATE - INTERVAL '30 DAYS'
        ) AS gmv_last_30_days,
        SUM(
          SELECT sid_actual.item_price 
          FROM dds.sat_item_details sid_actual
          WHERE sid_actual.item_id = soi.item_id
          AND sid_actual.load_ts <= sod.order_ts
          ORDER BY sid_actual.load_ts DESC
          LIMIT 1)
        FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'done'
            ) AND sod.order_ts >= CURRENT_DATE - INTERVAL '365 DAYS'
        ) AS gmv_last_365_days,
        SUM(
          SELECT sid_actual.item_price 
          FROM dds.sat_item_details sid_actual
          WHERE sid_actual.item_id = soi.item_id
          AND sid_actual.load_ts <= sod.order_ts
          ORDER BY sid_actual.load_ts DESC
          LIMIT 1)
        FILTER (
            WHERE EXISTS (
                SELECT 1 
                FROM dds.sat_order_details sod_inner 
                WHERE sod_inner.order_id = sod.order_id 
                  AND sod_inner.status = 'done'
            )
        ) AS total_gmv
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.link_client_order lco ON cl.client_id = lco.client_id
    LEFT JOIN 
        dds.sat_order_details sod ON lco.order_id = sod.order_id
    LEFT JOIN 
        dds.link_order_item loi ON sod.order_id = loi.order_id
    LEFT JOIN 
        dds.sat_item_details sid ON loi.item_id = sid.item_id
) subquery
WHERE rdm.client_id = subquery.client_id;
```

- Amount of favourites
```
UPDATE result_data_mart rdm
SET 
    favourite_items_count = subquery.favourite_items_count
FROM (
    SELECT
        cl.client_id,
        -- Count the number of favorite items for each client
        COUNT(DISTINCT lcf.item_id) AS favourite_items_count
    FROM 
        dds.hub_clients cl
    LEFT JOIN 
        dds.link_client_favorites lcf ON cl.client_id = lcf.client_id
    GROUP BY 
        cl.client_id
) subquery
WHERE rdm.client_id = subquery.client_id;
```
