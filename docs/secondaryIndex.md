## Create Listing columnfamily

    create columnfamily listings (
        listingid varchar,
        sellerid varchar,
        skuid varchar,
        productid varchar,
        mrp int,
        ssp int,
        sla int,
        stock int,
        title text,
        primary key ((sellerid, skuid), stock, ssp)    
    );

### Load data to listings from CSV file
    copy listings (sellerid, stock, ssp, listingid, mrp, productid, skuid, sla, title) from 'listing.csv';

### Create secondary index on productid
    create index on listings(productid);

### Only EQ operator allowed on index. IN clause is not allowed
    select * from listings where productid = 'COM1';

    select * from listings where productid IN ('COM1', 'COM3');     // NOT ALLOWED

### RANGE Queries not allowed in Secondary index (Supported with ALLOW FILTERING)

### CONTAINS and CONTAINS KEY works for SET and MAP columns with secondary index