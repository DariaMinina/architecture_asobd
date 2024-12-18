
```
CREATE TABLE result_data_mart (
client_id VARCHAR PRIMARY KEY,                          
gender VARCHAR,                                         
registration_date DATE,                                 
first_order_date DATE,                                  
last_order_date DATE,                                   
last_visit_date TIMESTAMP,                              
last_visit_platform VARCHAR,                            
has_premium BOOLEAN,                                    

orders_last_7_days INT,                                
orders_last_30_days INT,                               
orders_last_365_days INT,                              
total_orders INT,                                      

gmv_last_7_days DECIMAL(10, 2),                        
gmv_last_30_days DECIMAL(10, 2),                       
gmv_last_365_days DECIMAL(10, 2),                      
total_gmv DECIMAL(10, 2),                              

favorite_items_count INT,                              

load_ts TIMESTAMP DEFAULT CURRENT_TIMESTAMP,           
);
```
