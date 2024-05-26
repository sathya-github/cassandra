## Creates

### Describe Keyspaces
    describe keyspaces;

### Create Keyspace
    create keyspace catalog with replication={'class': 'SimpleStrategy', 'replication_factor':'3'};

### Use Keyspace
    use catalog;

### Create Columnfamily
    create columnfamily product (
        productId   varchar,
        title       text,
        brand       varchar,
        publisher   varchar,
        length      int,
        breadth     int,
        height      int,
        primary key (productId)
    );

### describe columnfamilies
    describe columnfamilies;

### Get more details about columnfamily
    describe <column family name>;
    describe product;

### Add a column
    alter columnfamily product add modelId varchar;

### Alter a property value
    alter columnfamily product with gc_grace_seconds=86400;

### Insert data
    insert into product (productId, brand, modelId) values ('MOB1', 'Samsung', 'GalaxyS6');
    insert into product (productId, title, publisher) values ('BOK1', 'The Kite Runner', 'Riverhead Books');
    insert into product (productId, title, breadth, length) values ('POST2', 'Led Zeppelin', 22, 36) using timestamp 1716706366;

## Complex Column Types

### Adding a Set
    alter columnfamily product add keyfeatures set<text>;

### Inserting row with Set
    insert into product (productid, title, brand, keyfeatures) 
    values ('COM1', 'Acer One', 'Acer', { 'detachable keyboard', 'multitouch display'});

### Adding a List
    alter columnfamily product add service_type list<text>;

### Inserting row with List
    insert into product (productId, title, brand, service_type)
    values ('SOFA1', 'Urban Living Derby Sofa', 'Urban Living', ['Needs to call Seller', 'Service Engineer will come to the site']);

### Adding map
    alter columnfamily product add camera map<text, text>;

### Inserting row with Map
    insert into product (productId, title, brand, camera)    
    values ('COM1', 'Acer One', 'Acer', {'front': 'VGA', 'rear': '2MP'});

## Counter type
Can only be added to a new columnfamily as the only column, in addition to the primary key

    create columnfamily product_view_count (
        productId text, 
        viewCount counter,
        primary key (productId)
    );

### Inserting data to counter column
Both the insert and update are done using the same update command.

    update product_view_count set viewCount = viewCount + 1 where productId = 'COM1';


## Create SKU Columnfamily

    create columnfamily skus (
        sellerid varchar,
        productid varchar,
        skuid varchar,
        title text,
        listingid varchar,
        islistingcreated boolean,
        timeofskucreation text,
        primary key ((sellerid, skuid), timeofskucreation, productid)
    );

### Copy command to load CSV file to skus table
    copy skus(sellerid, productid, skuid, title, listingid, islistingcreated, timeofskucreation) from 'skus.csv';

### Using token() function for range queries on partition key
    select * from skus where token(sellerid, skuid) > token('Decor', 'DEKORSKU');

### ORDER BY on partition keys will fail
    select * from skus where sellerid in ('Chroma', 'Decor') and skuid in ('CHROMASKU1', 'DECORSKU') order by sellerid;

## Clustering Keys

### Out of order clustering key based filtering is not supported. Below will 
### fail since it does not have filtering by timeofskucreation
    select * from skus where sellerid = 'Fab' and skuid = 'FABSKU' and productid = 'SOFA1';

### Adding timeofskucreation will work
    select * from skus where sellerid = 'Fab' and skuid = 'FABSKU' and timeofskucreation = '2015-12-11 12:21' and productid = 'SOFA1';