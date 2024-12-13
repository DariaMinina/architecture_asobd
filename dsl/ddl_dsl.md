//Hubs

-- Hub for Clients
CREATE TABLE hub_clients (
client_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP,
record_source VARCHAR
);

-- Hub for Items
CREATE TABLE hub_items (
item_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP,
record_source VARCHAR
);

-- Hub for Orders
CREATE TABLE hub_orders (
order_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP,
record_source VARCHAR
);

-- Hub for Subscriptions
CREATE TABLE hub_subscriptions (
subscription_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP,
record_source VARCHAR
);

-- Hub for Tracker
CREATE TABLE hub_tracker (
track_id VARCHAR PRIMARY KEY,
load_ts TIMESTAMP,
record_source VARCHAR
);

// Links

-- Link between Clients and Orders
CREATE TABLE link_client_order (
client_id VARCHAR,
order_id VARCHAR,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (client_id, order_id)
);

-- Link between Clients and Subscriptions
CREATE TABLE link_client_subscription (
client_id VARCHAR,
subscription_id VARCHAR,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (client_id, subscription_id)
);

-- Link between Clients and Items (Favorites)
CREATE TABLE link_client_favorites (
client_id VARCHAR,
item_id VARCHAR,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (client_id, item_id)
);

-- Link between Clients and Tracker (Visits)
CREATE TABLE link_client_tracker (
client_id VARCHAR,
track_id VARCHAR,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (client_id, track_id)
);


// Satellites

-- Satellite for Client Details (e.g., gender, registration date)
CREATE TABLE sat_client_details (
client_id VARCHAR,
gender VARCHAR,
registration_date DATE,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (client_id, load_ts)
);

-- Satellite for Order Details (e.g., status, order dates)
CREATE TABLE sat_order_details (
order_id VARCHAR,
status VARCHAR,
order_date DATE,
accepted BOOLEAN,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (order_id, load_ts)
);

-- Satellite for Item Details (e.g., item name)
CREATE TABLE sat_item_details (
item_id VARCHAR,
item_name VARCHAR,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (item_id, load_ts)
);

-- Satellite for Subscription Details (e.g., subscription status)
CREATE TABLE sat_subscription_details (
subscription_id VARCHAR,
subsc_status VARCHAR,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (subscription_id, load_ts)
);

-- Satellite for Tracker Details (e.g., visit date, platform)
CREATE TABLE sat_tracker_details (
track_id VARCHAR,
visit_date TIMESTAMP,
visit_platform VARCHAR,
src_type VARCHAR,
op_ts TIMESTAMP,
load_ts TIMESTAMP,
record_source VARCHAR,
PRIMARY KEY (track_id, load_ts)
);

