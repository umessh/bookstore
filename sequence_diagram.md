sequenceDiagram
    participant Cust as Customer
    box aws
    participant BCS as Book Catalog
    participant OS as Order API Service
    participant OQ as Orders Queue
    participant OP as Order Processing
    participant CDQ as Content Delivery Queue
    participant CDP as Content Delivery Processor
    participant S3 as Digital Books S3
    participant ORS as Order Service
    participant RDS as AWS RDS
    participant SNS as Amazon SNS
    end
    participant Notify as Sendgrid/Twilio/App notifications
    participant ERP as ERP
    participant PCS as Postal/Courier System
    participant ThirdP as Thirdparty notification
    participant Store as Store

    Cust->>BCS: Browse through the catalog
    Cust->>OS: Add the books to the cart (select delivery type)
    OS->>OQ: Add order to the queue
    OP->>OQ: Subscribe to queue

    rect rgb(227 246 255)
    alt Digital Book Delivery
        OP->>CDQ: Forward to content delivery queue
        CDP->>CDQ: Subscribe to content delivery queue
        CDP->>S3: Generate presigned URL
        CDP->>RDS: Update URL against order
        CDP->>ORS: Update order status via orders API
        ORS->>RDS: Update order status in DB
        CDP->>SNS: Send notification
        SNS->>Notify: Notify customer
        Notify->>Cust: Notifications
    end
    end

    rect rgb(255 251 227)
    alt Physical Book Delivery
        OP->>ERP: Check inventory & update inventory
        OP->>PCS: Create delivery order
        PCS->>ORS: Update order status
        ORS->>RDS: Update order status in DB
        ORS->>SNS: Send delivery/tracking details
        SNS->>Notify: Notify customer
        ThirdP->>OP: Update on delivery from courier system
    end
    end

    rect rgb(227 255 230)
    alt Physical Book Pickup
        OP->>ERP: Check inventory & update inventory
        OP->>ORS: Update order status
        ORS->>RDS: Update order status in DB
        ORS->>SNS: Send pickup details
        SNS->>Notify: Notify customer
        Notify->>Cust: Notify customer
        Cust->>Store: Pickup from store
        Store->>ORS: Order status update
    end
    end
