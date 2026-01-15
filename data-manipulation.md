---
layout: default
title: 2 - Data manipulation
nav_order: 3
parent: Workshop Activities
customjs: http://code.jquery.com/jquery-1.4.2.min.js
output: 
  md_document:
    variant: gfm        # GitHub-friendly markdown
    preserve_yaml: TRUE # keep Jekyll front-matter
---

# Data manipulation

- [1. Selecting specific columns](#1-selecting-specific-columns)
- [2. Select specific rows based on a
  condition](#2-select-specific-rows-based-on-a-condition)
- [3. Modify a data frame with
  `mutate()`](3-modify-a-data-frame-with-mutate)
- [4. Sorting data with `arrange()`](#4-sorting-data-with-arrange)
- [5. Summarizing variables with
  `summarise()`](#5-summarizing-variables-with-summarise)
- [6. Analyzing groups with
  `group_by()`](#6-analyzing-groups-with-group_by)

<style type="text/css">
div.html-widget {
  overflow-x: auto;
}
&#10;table {
  display: block;
  max-width: none;
  white-space: nowrap;
}
</style>

<img src="images/tidyverse-logo.png" alt="tidyverse logo" style="float:right;width:180px;"/>

IMPROVE INTRODUCTORY TEXT

When we work with data, it can be useful to work with smaller sections
of data.

In the remainder of activity 5, we will look at ways to select subsets
of our data to make it easier to work with.

- We will use piping to filter productData based on different
  conditions, such as:
  - Previewing only the column names that begin with `Product`
  - Previewing only the purchases from Tampa Bay
  - Previewing only the purchases that are corporate orders
  - Previewing only the purchases from Tampa that aren’t critical
    priority

Before we begin to filter, we need to look at Operators.

**Definition - “Operators”:** Special symbols or keywords used to
perform operations on arguments - logical operators specifically
designed for connecting or modifying boolean (true/false) logic
statements. <br>

**Operators**

- Logical operators
  - \< means “less than”
  - \<= means “less than or equal to”
  - \> means “greater than”
  - \>= means “greater than or equal to”
  - == means “exactly equal to”
  - != means “not equal to”
- Connecting logical statements:
  - x \| y means “x or y”
  - x & y means “x and y”

## 1. Selecting specific columns

The commands in this section will not create new objects as we won’t be
using them later on.

**Note: End each command in this section with `%>% head(5)`**

- Not using that to end commands that extract full columns of data will
  display a LOT.

- This will make things easier for you.

- If you were working with your own data, you would not need to add the
  `%>% head(5)`

To get a specific column, use piping and the `select()` function on your
data set.

- The parameter of `select()` is the name of the column you want to
  access.

<div class="task-box" markdown="1">

⭐ <u>Task 2-1</u>

**View a column**

Preview the values in the Row ID column.

- Column name: `Row_ID`

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
#data set %>% select the column titled `Row ID` and view the first 5 items.
purchaseData %>%
  select(Row_ID) %>%
  head(5)
```

    ##   Row_ID
    ## 1  40098
    ## 2  26341
    ## 3  25330
    ## 4  13524
    ## 5  47221

</details>

{::options parse_block_html='false'/}

*Hint:* Begin with the name of the data set, followed by your select
function passing in the column name as the parameter.

</div>

To select all of the columns from our data set that do *not* start with
specific text, we do the inverse,

- Again using the `select()` function
- The parameter has a minus sign `-` before the string value we want to
  exclude.

<div class="task-box" markdown="1">

⭐ <u>Task 2-2</u>

**Get a set of columns from data frame**

Select all the columns from your purchase data that are *not* the
“Postal_Code” column.

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
purchaseData %>%
  select(-Postal_Code) %>%
  head(5)
```

    ##   Row_ID                 Order_ID Order_Date  Ship_Date    Ship_Mode
    ## 1  40098 CA-2014-AB10015140-41954 2014-11-11 2014-11-13  First Class
    ## 2  26341   IN-2014-JR162107-41675 2014-02-05 2014-02-07 Second Class
    ## 3  25330   IN-2014-CR127307-41929 2014-10-17 2014-10-18  First Class
    ## 4  13524  ES-2014-KM1637548-41667 2014-01-28 2014-01-30  First Class
    ## 5  47221  SG-2014-RH9495111-41948 2014-11-05 2014-11-06     Same Day
    ##    Customer_ID    Customer_Name     Segment          City           State
    ## 1 AB-100151402    Aaron Bergman    Consumer Oklahoma City        Oklahoma
    ## 2    JR-162107    Justin Ritter   Corporate    Wollongong New South Wales
    ## 3    CR-127307     Craig Reiter    Consumer      Brisbane      Queensland
    ## 4   KM-1637548 Katherine Murray Home Office        Berlin          Berlin
    ## 5   RH-9495111      Rick Hansen    Consumer         Dakar           Dakar
    ##         Country         Region       Market  Product_ID   Category Sub_Category
    ## 1 United States     Central US         USCA TEC-PH-5816 Technology       Phones
    ## 2     Australia        Oceania Asia Pacific FUR-CH-5379  Furniture       Chairs
    ## 3     Australia        Oceania Asia Pacific TEC-PH-5356 Technology       Phones
    ## 4       Germany Western Europe       Europe TEC-PH-5267 Technology       Phones
    ## 5       Senegal Western Africa       Africa TEC-CO-6011 Technology      Copiers
    ##                                Product_Name   Sales Quantity Discount  Profit
    ## 1                          Samsung Convoy 3  221.98        2      0.0   62.15
    ## 2 Novimex Executive Leather Armchair, Black 3709.40        9      0.1 -288.77
    ## 3         Nokia Smart Phone, with Caller ID 5175.17        9      0.1  919.97
    ## 4            Motorola Smart Phone, Cordless 2892.51        5      0.1  -96.54
    ## 5            Sharp Wireless Fax, High-Speed 2832.96        8      0.0  311.52
    ##   Shipping_Cost Order_Priority
    ## 1         40.77           High
    ## 2        923.63       Critical
    ## 3        915.49         Medium
    ## 4        910.16         Medium
    ## 5        903.04       Critical

</details>

{::options parse_block_html='false'/}

</div>

❗ We can also select a *subset* of columns

- e.g., columns whose names begin with a common string of characters.

In our data set, multiple column names begin with “Product”. We want to
see only the data of columns whose names begin with “Product.” The
following explains the process.

- Use piping on your purchaseData

  - `purchaseData %>%`

- Use the `select()` function to select the columns

- Use the `starts_with()` function as the parameter for `select()`. In
  this case, you won’t pipe the results of `select()` into
  `starts_with()`, as you want the select to work on the `start_with()`
  parameter. That is, they are working simulteanously, and therefore, a
  pipe won’t work.

  - note that you’re selecting all columns that start with a specific
    name, rather than one column that is exactly equal to or not equal
    to a specific value

- The parameter for `starts_with()` is the value of the beginning of all
  columns you want to select.

<div class="task-box" markdown="1">

⭐ <u>Task 2-3</u>

**Get a set of columns from a data frame**

Select all the columns from our cleaned purchase data that start with
“Product”.

Write the one-line command to achieve this with piping and the
`starts_with()` function

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
# Selects all columns (and their values) from purchaseData whose names begin with "Product"
purchaseData %>%
  select(starts_with("Product")) %>%
  head(5) # again, remember that this is just for easier visibility, you don't need to have the head() function to select columns
```

    ##    Product_ID                              Product_Name
    ## 1 TEC-PH-5816                          Samsung Convoy 3
    ## 2 FUR-CH-5379 Novimex Executive Leather Armchair, Black
    ## 3 TEC-PH-5356         Nokia Smart Phone, with Caller ID
    ## 4 TEC-PH-5267            Motorola Smart Phone, Cordless
    ## 5 TEC-CO-6011            Sharp Wireless Fax, High-Speed

</details>

{::options parse_block_html='false'/}

</div>

## 2. Select specific rows based on a condition

We may also want to handle certain items (rows) in our data set based on
certain criteria.

- This is called “filtering”

- We can filter for different purposes, like processing statistical
  operations, making charts and so on.

- We can even create new data objects, which can make future analyses
  easier.

To select items (rows, *not* columns), we use the `filter()` function.

- The parameter for `filter()` is logical expression based on a column
  of the dataset. This expression will return TRUE or FALSE for each
  row, and the function will return the rows that were TRUE.

<div class="task-box" markdown="1">

⭐ <u>Task 2-4</u>

**Filter conditionally**

Filter all the rows from your purchase data where `Quantity` is greater
than 10.

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
purchaseData %>%
  filter(Quantity > 10) %>%
  head(5) # again, this is just for easier viewing now, but without this, you would see ALL rows that have Quantity greater than 10
```

    ##   Row_ID                 Order_ID Order_Date  Ship_Date      Ship_Mode
    ## 1  27704  IN-2014-PF1912027-41796 2014-06-06 2014-06-08   Second Class
    ## 2  12069  ES-2015-PJ1883564-42255 2015-09-08 2015-09-14 Standard Class
    ## 3  15380 ES-2015-PO18865139-42018 2015-01-14 2015-01-18 Standard Class
    ## 4  25795  IN-2015-VG2180558-42273 2015-09-26 2015-09-28   Second Class
    ## 5   6550 MX-2015-JH15820141-42356 2015-12-18 2015-12-20   Second Class
    ##   Customer_ID     Customer_Name   Segment Postal_Code               City
    ## 1  PF-1912027      Peter Fuller  Consumer          NA         Mudanjiang
    ## 2  PJ-1883564     Patrick Jones Corporate          NA              Prato
    ## 3 PO-18865139 Patrick O'Donnell  Consumer          NA   Stockton-on-Tees
    ## 4  VG-2180558       Vivek Grady Corporate          NA Thiruvananthapuram
    ## 5 JH-15820141       John Huston  Consumer          NA           Paysandú
    ##          State        Country          Region       Market  Product_ID
    ## 1 Heilongjiang          China    Eastern Asia Asia Pacific OFF-AP-4959
    ## 2      Tuscany          Italy Southern Europe       Europe OFF-AP-4743
    ## 3      England United Kingdom Northern Europe       Europe TEC-CO-3598
    ## 4       Kerala          India   Southern Asia Asia Pacific FUR-BO-5951
    ## 5     Paysandú        Uruguay   South America        LATAM FUR-CH-4531
    ##          Category Sub_Category
    ## 1 Office Supplies   Appliances
    ## 2 Office Supplies   Appliances
    ## 3      Technology      Copiers
    ## 4       Furniture    Bookcases
    ## 5       Furniture       Chairs
    ##                                          Product_Name   Sales Quantity Discount
    ## 1                         KitchenAid Microwave, White 3701.52       12        0
    ## 2                                   Hoover Stove, Red 7958.58       14        0
    ## 3                          Brother Fax Machine, Laser 4141.02       13        0
    ## 4                Sauder Classic Bookcase, Traditional 5667.87       13        0
    ## 5 Harbour Creations Executive Leather Armchair, Black 3473.14       11        0
    ##    Profit Shipping_Cost Order_Priority
    ## 1 1036.08        804.54       Critical
    ## 2 3979.08        778.32            Low
    ## 3 1697.67        668.96           High
    ## 4 2097.03        658.35         Medium
    ## 5  868.12        634.53           High

</details>

{::options parse_block_html='false'/}

*Hint:* `>` is the ‘greater than’ operator.

</div>

<div class="task-box" markdown="1">

⭐ <u>Task 2-5</u>

**Filter conditionally**

Filter all the rows from your purchase data where `City` is “Sydney”.

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
purchaseData %>%
  filter(City == "Sydney") %>%
  head(5) # again, this is just for easier viewing now, but without this, you would see ALL rows that have Sidney listed as City
```

    ##   Row_ID               Order_ID Order_Date  Ship_Date      Ship_Mode
    ## 1  22732 IN-2014-JM156557-41818 2014-06-28 2014-07-01   Second Class
    ## 2  25026 IN-2013-RP192707-41438 2013-06-13 2013-06-13       Same Day
    ## 3  29629 IN-2014-LC168857-41747 2014-04-18 2014-04-19    First Class
    ## 4  21263 IN-2015-MB173057-42179 2015-06-24 2015-06-28 Standard Class
    ## 5  20521 IN-2015-BE114557-42080 2015-03-17 2015-03-22   Second Class
    ##   Customer_ID   Customer_Name     Segment Postal_Code   City           State
    ## 1   JM-156557     Jim Mitchum   Corporate          NA Sydney New South Wales
    ## 2   RP-192707    Rachel Payne   Corporate          NA Sydney New South Wales
    ## 3   LC-168857  Lena Creighton    Consumer          NA Sydney New South Wales
    ## 4   MB-173057 Maria Bertelson    Consumer          NA Sydney New South Wales
    ## 5   BE-114557      Brad Eason Home Office          NA Sydney New South Wales
    ##     Country  Region       Market  Product_ID   Category Sub_Category
    ## 1 Australia Oceania Asia Pacific TEC-PH-5842 Technology       Phones
    ## 2 Australia Oceania Asia Pacific TEC-CO-3611 Technology      Copiers
    ## 3 Australia Oceania Asia Pacific TEC-CO-6012 Technology      Copiers
    ## 4 Australia Oceania Asia Pacific FUR-BO-5948  Furniture    Bookcases
    ## 5 Australia Oceania Asia Pacific TEC-CO-4568 Technology      Copiers
    ##                          Product_Name   Sales Quantity Discount  Profit
    ## 1 Samsung Smart Phone, with Caller ID 2862.68        5      0.1  763.28
    ## 2         Brother Wireless Fax, Laser 3068.36        9      0.1 1124.90
    ## 3           Sharp Wireless Fax, Laser 1601.64        5      0.1  587.19
    ## 4      Sauder Classic Bookcase, Metal 5486.67       14      0.1 2316.51
    ## 5         Hewlett Copy Machine, Color 3299.56       14      0.1  366.28
    ##   Shipping_Cost Order_Priority
    ## 1        897.35       Critical
    ## 2        555.77           High
    ## 3        511.47       Critical
    ## 4        346.60         Medium
    ## 5        336.02         Medium

</details>

{::options parse_block_html='false'/}

*Hint:* `==` is used for “equal to”

</div>

<div class="task-box" markdown="1">

⭐ <u>Task 2-6</u>

**Create a data frame**

Create a new data frame with all the rows from purchaseData where
`Country` is “United States” and `Discount` is greater than 0.

- Name this data frame: `discountedUSPurchases`

- Do not add ” %\>% head(5)” to the command when <u>creating</u> a new
  data frame. We were just using this to <u>view</u> subsets of the
  data.

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
# Create the dataframe
discountedUSPurchases <- purchaseData %>%
                          filter(Country == "United States" & Discount > 0)

# View your data frame
discountedUSPurchases %>%
  head(5)
```

    ##   Row_ID                 Order_ID Order_Date  Ship_Date      Ship_Mode
    ## 1  36258 CA-2012-AB10015140-40974 2012-03-06 2012-03-07    First Class
    ## 2  39519 CA-2012-AB10015140-40958 2012-02-19 2012-02-25 Standard Class
    ## 3  40977 CA-2013-AH10030140-41635 2013-12-27 2013-12-31 Standard Class
    ## 4  36651 CA-2012-AH10030140-41041 2012-05-12 2012-05-18 Standard Class
    ## 5  37425 US-2012-AH10030140-41206 2012-10-24 2012-10-27    First Class
    ##    Customer_ID Customer_Name   Segment Postal_Code          City      State
    ## 1 AB-100151404 Aaron Bergman  Consumer       98103       Seattle Washington
    ## 2 AB-100151402 Aaron Bergman  Consumer       76017     Arlington      Texas
    ## 3 AH-100301404 Aaron Hawkins Corporate       94122 San Francisco California
    ## 4 AH-100301404 Aaron Hawkins Corporate       90004   Los Angeles California
    ## 5 AH-100301404 Aaron Hawkins Corporate       94109 San Francisco California
    ##         Country     Region Market  Product_ID        Category Sub_Category
    ## 1 United States Western US   USCA FUR-CH-4421       Furniture       Chairs
    ## 2 United States Central US   USCA OFF-ST-3078 Office Supplies      Storage
    ## 3 United States Western US   USCA TEC-PH-4389      Technology       Phones
    ## 4 United States Western US   USCA FUR-CH-4840       Furniture       Chairs
    ## 5 United States Western US   USCA OFF-BI-4372 Office Supplies      Binders
    ##                                    Product_Name  Sales Quantity Discount Profit
    ## 1    Global Push Button Manager's Chair, Indigo  48.71        1      0.2   5.48
    ## 2                            Akro Stacking Bins  12.62        2      0.2  -2.52
    ## 3                          Geemarc AmpliPOWER60 668.16        9      0.2  75.17
    ## 4 Iceberg Nesting Folding Chair, 19w x 6d x 43h 279.46        6      0.2  20.96
    ## 5                       GBC VeloBind Cover Sets  49.41        4      0.2  18.53
    ##   Shipping_Cost Order_Priority
    ## 1         11.13           High
    ## 2          1.97            Low
    ## 3         45.74         Medium
    ## 4         11.69         Medium
    ## 5          2.84           High

</details>

{::options parse_block_html='false'/}

*Hint:* `&` is used for “and”, in cases where you want to manage
multiple cases like filtering my two variables<br> - e.g., values of the
`Country` and `Discount` columns.

</div>

------------------------------------------------------------------------

📍 Reminder! Save your work

------------------------------------------------------------------------

## 3. Modify a data frame with `mutate()`

“Mutation” involves creating or altering columns in a data frame,

- using the `mutate()` function
  - e.g., If you have a column with a range of numbers, but you want to
    be able to quickly work with the data only over or under a specific
    value, like any orders under \$10, you can create a “Cheap” column
    and the values would be TRUE or FALSE.
- adds new variables or modifies existing ones.

<br> <u>Here’s how we’ll do it:</u>

- Assign the mutation (modification) to the existing dataframe (or new
  object if you want to create a new one rather than adding a new column
  to an existing one)
  - `existing_dataframe_name <-`
- Identify the existing name of the object you want to mutate
  - `existing_dataframe_name`
- Use a pipe to identify the action being performed on our existing
  object
  - `%>%`
- Identify that the action being performed on the existing object is the
  mutation
  - using the `mutate()` function
- Pass the condition in as the parameter for the `mutate` function
  - If we want to add a variable (column), begin the parameter of
    `mutate()` with `new_column_name =`
    - `=` means the values assigned to each item in that column is
      generated by the following condition
    - The new column name is and is followed by the expression to create
      the new variable
  - Say we want to add a column that has a TRUE/FALSE variable (aka
    boolean) for whether the order priority is low.
    - The expression will be `Order_Priority == "Low"`
      - `==` means “the left value is equal to the right value”
        - The result will be a vector with TRUE or FALSE, with TRUE for
          every row in the data is “Low” in the “Order_Priority” column,
          and FALSE otherwise

``` r
purchaseData <- purchaseData %>%
      mutate(Low_Priority = (Order_Priority == "Low"))

#view first 3 rows of your data frame
purchaseData %>%
    head(3)
```

    ##   Row_ID                 Order_ID Order_Date  Ship_Date    Ship_Mode
    ## 1  40098 CA-2014-AB10015140-41954 2014-11-11 2014-11-13  First Class
    ## 2  26341   IN-2014-JR162107-41675 2014-02-05 2014-02-07 Second Class
    ## 3  25330   IN-2014-CR127307-41929 2014-10-17 2014-10-18  First Class
    ##    Customer_ID Customer_Name   Segment Postal_Code          City
    ## 1 AB-100151402 Aaron Bergman  Consumer       73120 Oklahoma City
    ## 2    JR-162107 Justin Ritter Corporate          NA    Wollongong
    ## 3    CR-127307  Craig Reiter  Consumer          NA      Brisbane
    ##             State       Country     Region       Market  Product_ID   Category
    ## 1        Oklahoma United States Central US         USCA TEC-PH-5816 Technology
    ## 2 New South Wales     Australia    Oceania Asia Pacific FUR-CH-5379  Furniture
    ## 3      Queensland     Australia    Oceania Asia Pacific TEC-PH-5356 Technology
    ##   Sub_Category                              Product_Name   Sales Quantity
    ## 1       Phones                          Samsung Convoy 3  221.98        2
    ## 2       Chairs Novimex Executive Leather Armchair, Black 3709.40        9
    ## 3       Phones         Nokia Smart Phone, with Caller ID 5175.17        9
    ##   Discount  Profit Shipping_Cost Order_Priority Low_Priority
    ## 1      0.0   62.15         40.77           High        FALSE
    ## 2      0.1 -288.77        923.63       Critical        FALSE
    ## 3      0.1  919.97        915.49         Medium        FALSE

``` r
# Note how there is a new column at the end called "Low_Priority". It starts with
# 3 values of FALSE as the first 3 rows were not low priority (see the Order_Priority column)
```

<div class="task-box" markdown="1">

⭐ <u>Task 2-7</u>

**Add a column to your dataframe**

Now try it yourself. Add a new boolean (TRUE/FALSE) variable (column) to
the purchase data that identifies whether a purchase’s shipping cost is
greater than 100 dollars.

- Name the new column: `High_Shipping`
- The value will be TRUE if the `Shipping_Cost` value is over (`>`) 100.

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
purchaseData <- purchaseData %>%
  mutate(High_Shipping = (Shipping_Cost > 100))

#view your data frame
purchaseData %>%
  head(5)
```

    ##   Row_ID                 Order_ID Order_Date  Ship_Date    Ship_Mode
    ## 1  40098 CA-2014-AB10015140-41954 2014-11-11 2014-11-13  First Class
    ## 2  26341   IN-2014-JR162107-41675 2014-02-05 2014-02-07 Second Class
    ## 3  25330   IN-2014-CR127307-41929 2014-10-17 2014-10-18  First Class
    ## 4  13524  ES-2014-KM1637548-41667 2014-01-28 2014-01-30  First Class
    ## 5  47221  SG-2014-RH9495111-41948 2014-11-05 2014-11-06     Same Day
    ##    Customer_ID    Customer_Name     Segment Postal_Code          City
    ## 1 AB-100151402    Aaron Bergman    Consumer       73120 Oklahoma City
    ## 2    JR-162107    Justin Ritter   Corporate          NA    Wollongong
    ## 3    CR-127307     Craig Reiter    Consumer          NA      Brisbane
    ## 4   KM-1637548 Katherine Murray Home Office          NA        Berlin
    ## 5   RH-9495111      Rick Hansen    Consumer          NA         Dakar
    ##             State       Country         Region       Market  Product_ID
    ## 1        Oklahoma United States     Central US         USCA TEC-PH-5816
    ## 2 New South Wales     Australia        Oceania Asia Pacific FUR-CH-5379
    ## 3      Queensland     Australia        Oceania Asia Pacific TEC-PH-5356
    ## 4          Berlin       Germany Western Europe       Europe TEC-PH-5267
    ## 5           Dakar       Senegal Western Africa       Africa TEC-CO-6011
    ##     Category Sub_Category                              Product_Name   Sales
    ## 1 Technology       Phones                          Samsung Convoy 3  221.98
    ## 2  Furniture       Chairs Novimex Executive Leather Armchair, Black 3709.40
    ## 3 Technology       Phones         Nokia Smart Phone, with Caller ID 5175.17
    ## 4 Technology       Phones            Motorola Smart Phone, Cordless 2892.51
    ## 5 Technology      Copiers            Sharp Wireless Fax, High-Speed 2832.96
    ##   Quantity Discount  Profit Shipping_Cost Order_Priority Low_Priority
    ## 1        2      0.0   62.15         40.77           High        FALSE
    ## 2        9      0.1 -288.77        923.63       Critical        FALSE
    ## 3        9      0.1  919.97        915.49         Medium        FALSE
    ## 4        5      0.1  -96.54        910.16         Medium        FALSE
    ## 5        8      0.0  311.52        903.04       Critical        FALSE
    ##   High_Shipping
    ## 1         FALSE
    ## 2          TRUE
    ## 3          TRUE
    ## 4          TRUE
    ## 5          TRUE

</details>

{::options parse_block_html='false'/}

*Hint:* `Shipping_Cost > 100`

</div>

The `mutate()` function also allows you to change the data type for each
variable by substituting with a new variable or to create a new variable
based on calculations between two variables. For example, let’s imagine
you wanted to know how long it takes to ship a product after it is
ordered to see if the process can be streamlined or improved. To do
that, you would need to: - Transform your data variables into a date
data type (a specific type of data that allows for calculating
differences between dates); - Calculate the difference in days between
the two date variables (`Order_date` and `Ship_date`) and save it in a
new variable.

Here’s how you can do that using the `mutate()` function

``` r
purchaseData <- purchaseData %>% # Get the purchaseData object
  mutate( # identify that you want to mutate variables
    # Different variables that are being mutated inside the mutate function
    # should be separated by comma
    Order_Date = as.Date(Order_Date), # create a variable that is the same as the Order_Date
    # variable but transformed to date type, and overwrite Order_Date with that new variable
    Ship_Date = as.Date(Ship_Date), # repeat for the Ship_Date variable
    days_to_ship = Ship_Date - Order_Date # calculate the difference in days between both dates variables
  )

# View the dataset
purchaseData %>%
  head(5)
```

    ##   Row_ID                 Order_ID Order_Date  Ship_Date    Ship_Mode
    ## 1  40098 CA-2014-AB10015140-41954 2014-11-11 2014-11-13  First Class
    ## 2  26341   IN-2014-JR162107-41675 2014-02-05 2014-02-07 Second Class
    ## 3  25330   IN-2014-CR127307-41929 2014-10-17 2014-10-18  First Class
    ## 4  13524  ES-2014-KM1637548-41667 2014-01-28 2014-01-30  First Class
    ## 5  47221  SG-2014-RH9495111-41948 2014-11-05 2014-11-06     Same Day
    ##    Customer_ID    Customer_Name     Segment Postal_Code          City
    ## 1 AB-100151402    Aaron Bergman    Consumer       73120 Oklahoma City
    ## 2    JR-162107    Justin Ritter   Corporate          NA    Wollongong
    ## 3    CR-127307     Craig Reiter    Consumer          NA      Brisbane
    ## 4   KM-1637548 Katherine Murray Home Office          NA        Berlin
    ## 5   RH-9495111      Rick Hansen    Consumer          NA         Dakar
    ##             State       Country         Region       Market  Product_ID
    ## 1        Oklahoma United States     Central US         USCA TEC-PH-5816
    ## 2 New South Wales     Australia        Oceania Asia Pacific FUR-CH-5379
    ## 3      Queensland     Australia        Oceania Asia Pacific TEC-PH-5356
    ## 4          Berlin       Germany Western Europe       Europe TEC-PH-5267
    ## 5           Dakar       Senegal Western Africa       Africa TEC-CO-6011
    ##     Category Sub_Category                              Product_Name   Sales
    ## 1 Technology       Phones                          Samsung Convoy 3  221.98
    ## 2  Furniture       Chairs Novimex Executive Leather Armchair, Black 3709.40
    ## 3 Technology       Phones         Nokia Smart Phone, with Caller ID 5175.17
    ## 4 Technology       Phones            Motorola Smart Phone, Cordless 2892.51
    ## 5 Technology      Copiers            Sharp Wireless Fax, High-Speed 2832.96
    ##   Quantity Discount  Profit Shipping_Cost Order_Priority Low_Priority
    ## 1        2      0.0   62.15         40.77           High        FALSE
    ## 2        9      0.1 -288.77        923.63       Critical        FALSE
    ## 3        9      0.1  919.97        915.49         Medium        FALSE
    ## 4        5      0.1  -96.54        910.16         Medium        FALSE
    ## 5        8      0.0  311.52        903.04       Critical        FALSE
    ##   High_Shipping days_to_ship
    ## 1         FALSE       2 days
    ## 2          TRUE       2 days
    ## 3          TRUE       1 days
    ## 4          TRUE       2 days
    ## 5          TRUE       1 days

## 4. Sorting data with `arrange()`

Being able to arrange data by ordering values numerically or
alphabetically is particularly handy for swiftly identifying which
measurements recorded the highest or lowest values.

The `arrange()` function enables you to order your data frame according
to the values of a specific variable.

- Parameter of `arrange()` is the column you want to use to sort your
  data frame

- This is particularly handy for swiftly identifying which measurements
  recorded the highest or lowest values.

When using `arrange()`, you specify the column name that you wish to
organize by.

*hint*: You can also use the `sort()` function but it only takes a
vector parameter, not a data frame.

<div class="task-box" markdown="1">

⭐ <u>Task 2-8</u>

**Sort data frame**

Update `purchaseData` to sort objects by price (column `Sales`).

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
purchaseData <- purchaseData %>%
                    arrange(Sales)

# View your data frame
purchaseData %>%
  head(5)
```

    ##   Row_ID                 Order_ID Order_Date  Ship_Date      Ship_Mode
    ## 1  35398 US-2015-ZC21910140-42175 2015-06-20 2015-06-24 Standard Class
    ## 2  40589 CA-2015-RS19765140-42066 2015-03-03 2015-03-03       Same Day
    ## 3  39955 CA-2014-KB16600140-41812 2014-06-22 2014-06-26 Standard Class
    ## 4  36008 CA-2012-JO15280140-40998 2012-03-30 2012-03-30       Same Day
    ## 5  33403 US-2012-HG14965140-41177 2012-09-25 2012-09-25       Same Day
    ##    Customer_ID    Customer_Name   Segment Postal_Code         City        State
    ## 1 ZC-219101402 Zuschuss Carroll  Consumer       77095      Houston        Texas
    ## 2 RS-197651402   Roland Schwarz Corporate       76706         Waco        Texas
    ## 3 KB-166001402      Ken Brennan Corporate       60623      Chicago     Illinois
    ## 4 JO-152801406    Jas O'Carroll  Consumer       19120 Philadelphia Pennsylvania
    ## 5 HG-149651402    Henry Goldwyn Corporate       75150     Mesquite        Texas
    ##         Country     Region Market  Product_ID        Category Sub_Category
    ## 1 United States Central US   USCA OFF-AP-4739 Office Supplies   Appliances
    ## 2 United States Central US   USCA OFF-BI-2935 Office Supplies      Binders
    ## 3 United States Central US   USCA OFF-BI-3268 Office Supplies      Binders
    ## 4 United States Eastern US   USCA OFF-BI-3318 Office Supplies      Binders
    ## 5 United States Central US   USCA OFF-BI-2880 Office Supplies      Binders
    ##                                                                 Product_Name
    ## 1 Hoover Replacement Belt for Commercial Guardsman Heavy-Duty Upright Vacuum
    ## 2                                   Acco Suede Grain Vinyl Round Ring Binder
    ## 3                         Avery Durable Slant Ring Binders With Label Holder
    ## 4                                              Avery Round Ring Poly Binders
    ## 5                                                          Acco 3-Hole Punch
    ##   Sales Quantity Discount Profit Shipping_Cost Order_Priority Low_Priority
    ## 1  0.44        1      0.8  -1.11          1.01         Medium        FALSE
    ## 2  0.56        1      0.8  -0.95          1.08         Medium        FALSE
    ## 3  0.84        1      0.8  -1.34          1.06         Medium        FALSE
    ## 4  0.85        1      0.7  -0.60          1.10           High        FALSE
    ## 5  0.88        1      0.8  -1.40          1.09           High        FALSE
    ##   High_Shipping days_to_ship
    ## 1         FALSE       4 days
    ## 2         FALSE       0 days
    ## 3         FALSE       4 days
    ## 4         FALSE       0 days
    ## 5         FALSE       0 days

</details>

{::options parse_block_html='false'/}

*Hint:* Do not wrap the column name in quotations.

</div>

## 5. Summarizing variables with `summarise()`

The `summarise()` function synthesizes information into a data frame
with information like totals, averages, medians, and so on. It reduces
the number of rows in the dataframe by summarizing information from
multiple rows into one value (e.g., mean value)

- `summarise()` takes an unlimited number of parameters, where each
  parameter will appear as a column. <br>

<div class="task-box" markdown="1">

⭐ <u>Task 2-9</u>

**Preview statistics**

Preview the mean sales values and mean discount values in the discounted
US purchases data using the `summarise()` function.

- parameter 1: `columnName = mean(column)`
- another parameter: `columnName2 = mean(another column)`

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
# Only purchases made in the US with discounts (we created this object before)
discountedUSPurchases %>% 
   summarise( # call the summarise() function. It is helpful to break the parameters into different lines to see the new columns you are creating
        meanSales = mean(Sales),    #average sale price 
        meanDiscount = mean(Discount) # and average discount
              )
```

    ##   meanSales meanDiscount
    ## 1  232.7353    0.3004407

``` r
#To retrieve this data later, assign this command to a new variable.
```

</details>

{::options parse_block_html='false'/}

*Hint:* Both spellings, `summarize` or `summarise`, will work.

</div>

## 6. Analyzing groups with `group_by()`

Let’s say we wanted to know how profitable each US city is.

That means we want to get the average profit, but grouped by US city.

As you recall, we can use summarise to get an average, but in this case,
we want to calculate the average within groups. That’s where the
`group_by` function comes in handy.

- The parameter of the `group_by()` function is the name of the column
  you want to group by. No need to use quotation marks.

<div class="task-box" markdown="1">

⭐ <u>Task 2-10</u>

**Create a data frame**

Create a data frame of US Cities and the profit on discounted values for
each.

- You will use the `discountedUSPurchases` data frame to create this
  *new* data frame.
- Name the new data frame `USCityProfits`.
- You will use `group_by()` with `City`, where each row will be a city.
- You will use `summarise()` function to get the summary statistics for
  each city.
- The statistic you will be summarizing is the total `Profit` values on
  purchases made in the US where the items have been discounted.

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
# Only purchases made in the US with discounts
USCityProfits <- discountedUSPurchases %>% 
      group_by(City) %>% # group by city purchase was made in
      summarise(totalProfit = sum(Profit)) # total profit for each city

# Now view your results
USCityProfits
```

    ## # A tibble: 348 × 2
    ##    City        totalProfit
    ##    <chr>             <dbl>
    ##  1 Abilene           -3.76
    ##  2 Akron           -187.  
    ##  3 Albuquerque       96.2 
    ##  4 Allen            -39.9 
    ##  5 Allentown       -226.  
    ##  6 Altoona           -1.19
    ##  7 Amarillo        -388.  
    ##  8 Anaheim          448.  
    ##  9 Ann Arbor          5.23
    ## 10 Apopka            54.4 
    ## # ℹ 338 more rows

*Note:* You might have noticed that the resulting data frame is
displayed a bit differently than before. This is because in the
tidyverse package, data frames take this new format called `tibbles`,
that are previewed in this way you are seeing above. Using the
`group_by()` and `summarise()` functions together has automatically
transformed your results into a `tibble`. They are the same as data
frames, but are just printed differently in the console as an ouput.

</details>

{::options parse_block_html='false'/}

</div>

The table will be sorted by city, alphabetically

Optional activities: You can now use the functions you just learned to
review this new data frame.

⭐ **Sort the table by `Profit` using the `arrange()` function to order
it by the lowest profitable city to the highest profitable city.**

<br>

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
USCityProfits <- USCityProfits %>%
                        arrange(totalProfit)
```

</details>

{::options parse_block_html='false'/}

<br>

⭐ **View the 5 least profitable cities.** <br>

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
# Least profitable are at the top of our data frame, since `arrange()` puts data in ascending order by default. 
USCityProfits %>%
    head(5)
```

    ## # A tibble: 5 × 2
    ##   City         totalProfit
    ##   <chr>              <dbl>
    ## 1 Philadelphia     -13838.
    ## 2 Houston          -10153.
    ## 3 San Antonio       -7299.
    ## 4 Lancaster         -7243.
    ## 5 Chicago           -6655.

</details>

{::options parse_block_html='false'/}

⭐ **View the 5 most profitable cities.**

Use `tail()` to get the last 5 rows of a data frame.

{::options parse_block_html='true' /}
<details>

<summary>

Check your code
</summary>

``` r
# Most profitable are at the end of our data frame
USCityProfits %>%
        tail(5)
```

    ## # A tibble: 5 × 2
    ##   City          totalProfit
    ##   <chr>               <dbl>
    ## 1 San Diego           2895.
    ## 2 San Francisco       7067.
    ## 3 Seattle             7452.
    ## 4 Los Angeles        12697.
    ## 5 New York City      16994.

</details>

{::options parse_block_html='false'/}

------------------------------------------------------------------------

📍 Reminder! Save your work.

------------------------------------------------------------------------

<script>  
function toggle(input) {
    var x = document.getElementById(input);
    if (x.style.display === "none") {
        x.style.display = "block";
    } else {
        x.style.display = "none";
    }
}
</script>

<style>
details {
    background-color: lightgray; 
    padding: 10px;
    margin: 5px;
    border-radius: 5px;
}
.task-box {
      border: 1.5px solid #ccc;
      padding: 10px;
      margin: 10px 0;
      border-radius: 5px;
      background-color: #f5f2f6;
  }
  &#10;</style>

<!--https://gist.github.com/rxaviers/7360908-->

[NEXT STEP: Data Visualization](data-vizualization.html){: .btn
.btn-blue }
