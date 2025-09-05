erDiagram

    STORE {
        int store_id PK
        string name
        string address
        string city
        string state
        string postal_code
    }
    ORDER {
        bigint order_id PK
        string customer_id
        datetime order_date
        enum status
        enum order_type
        enum order_delivery_type
        bigint order_shipment_id FK
        int pickup_store_id FK
        float total_amount
        int payment_id FK
    }
    ORDER_SHIPMENT {
        bigint shipment_id PK
        string shipment_reference
        string address
        string post_code
        string city
        string region
        string track_url
        enum shipment_status
    }
    ORDERITEM {
        bigint order_item_id PK
        bigint order_id FK
        bigint book_id 
        int quantity
        float unit_price
    }
    PAYMENT_DETAILS {
        int payment_id PK
        string payment_reference
        enum payment_type
        string payment_status
    }

    %% Relationships
    ORDER ||--o{ ORDERITEM : contains
    ORDER }o--|| STORE : "pickup at"
    ORDER }|--|| ORDER_SHIPMENT : "has shipment"
    ORDER }|--|| PAYMENT_DETAILS : "paid by"
    ORDERITEM }|--|| ORDER : "belongs to"
