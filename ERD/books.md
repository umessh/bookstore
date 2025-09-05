erDiagram

    BOOK {
        bigint book_id PK
        string title
        string author
        string description
        string isbn
        float price
        string format
        string digital_asset_url
        date publication_date
        string publisher
        string cover_image_url
    }
    CATEGORY {
        string category_id PK
        string name
        string description
    }
    BOOKCATEGORY {
        string book_id FK
        string category_id FK
    }


    BOOK ||--o{ BOOKCATEGORY : "classified as"
    CATEGORY ||--o{ BOOKCATEGORY : "includes"
