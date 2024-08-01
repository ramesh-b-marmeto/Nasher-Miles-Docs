
# Nasher Miles Product page

> Note : swatches are implemented using product as a variant , so every product have only one   variant of one color and one size , all variant products are grouped under a collection with same product name .



## Page Features

- ### Along with product title , the breadcrumb naviagtion or vendor or collection name of a product is rendered above the product title based on custmizer option . logic can be found in product snippet .
  ```
    {%- case block.settings.subheading -%}
      {%- when 'breadcrumbs' -%}
        //adding breadcrumb logic , adding Home > collection name > product name
      {%- when 'collection' -%}
        //adding collection name logic
      {%- when 'vendor' -%}
        //adding vendor name 
  ``` 
  > #### this resultant text will be placed before title

- ### Buy now button and EMI payment features is offered and maintained by app .

- ### Pincode checker block is also offered by app .

- ### Product media gallery is offered in 3 opitions namely
  >Note : Media gallery type is caraousel for medium and small screens regardless of media gallery type option .

  -  **Grid**- where images are displyed in grid fashion 
  -  **Stacked** - where images are stacked one below another taking full width of parent div .
  - **Carousel with thubmnail slider** - where images are displayed in slider using flickity

- ### Variant as a product swatches

    >In this store, each product is configured to have multiple variants based on two options: **color** and **size**. This means that each unique combination of these options (e.g., red in small, blue in medium) is created as a separate product .

    ### Functionality Description
    To implement variant swatches on our product pages, we retrieve a collection of all variant products by accessing tags associated with each product. The tags follow a specific format where the collection name is the same as the product's name. This allows us to categorize variants of the same product name into distinct collections. We then loop through these collections to display swatches corresponding to each variant, linking directly to their respective product pages.
	 
    ### Implementation Steps
    1. **Access Product Tags**: 
      - Each product in our store has tags formatted as `collection-collection-name-goes-here`.
      - These tags categorize products into collections based on their variant options.

    2. **Retrieve Variant Collections**: 
      - Extract the collection name from each product's tags.
      - Use this collection name to gather all variants associated with that product.

    3. **Display Swatches**:
      - Loop through each variant collection.
      - Display visual swatches representing the color options available for each variant.
      - Ensure each swatch links to the specific product variant's page.

    #### For color swatch , to fetch the products and add as swatch :

    ```
      {% for c_product in collections[handlized_collection].products %}
        {% if c_product.tags contains product_type_by_tag %} 
           //to check if , product is any one of type , like single , set or bag
          {% if product_option_by_size == c_product_option_by_size %}
            // to check , if product type is same
            here place the swatch variant
          {% endif%}
        {% endif%}
      {% endfor %}

    ```
    only the products which  has same type of product [single , set , bag ], and same size variant [small , large , medium , small large , medium large , small medium large] , we will add that as a product variant swatch , by adding product href to that swatch anchor tag .
- ### Sticky form for add to cart is enabled only in medium to small screens, which has price ,discount and add to cart button .

- ### To redeem gift card , the buy button which was coming from an app is replaced with shopify checkout button to reddem gifts ijn shopify checkout page .
  #### Here is the code logic responsible for swapping checkout buttons
  ```
  const cartPaymentContainer = giftCardCheckboxInput.closest('.cart__foot-inner') || giftCardCheckboxInput.closest('.product__submit__holder')
    if (giftCardCheckboxInput.checked) {
      cartPaymentContainer?.classList.add('native-checkout');
    } else {
      cartPaymentContainer?.classList.remove('native-checkout');
    }
  ```
  #### on toggling class `native-checkout` on checkout container , styles have been defined to make native checkout to display block , and app checkout to display none and vice versa on input is checked , which is added on eventlistener for that input check element . 


