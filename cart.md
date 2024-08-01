# Nasher Miles Cart page and cart drawer

- ### Cart page/drawer contains the items which has been added to cart , we render the items into cart page/drawer usinjg cart storefront api/cart object given by shopify.

- ### Cart page contains empty cart  collections .
  - #### empty cart collection is showed only when cart is empty , this can be given in global customization settings to merchant , to select the collection , which merchant wants to display in cart page/drawer by using collection pickers schema inout settings .

- ### Cart page contains , upsell section , where related products to the cart items will be displayed , only when cart has atleast one item .
  - #### To get related products to display , we use product metafield , where merchant adds related product in that metafield.
  - #### After accessing the metafields of all products in cart , we display the products in flickity carousel .

    The reference liquid code to access the metafields to retrieve products 
    ```
    {% assign cross_sellHandles = '' %}
    {% assign item_in_cart_count = 0 %}
    {% for item in cart.items %}
      {% for product in item.product.metafields.custom.you_may_also_like.value.product.value %}
        {% unless product.available %}
          {% assign item_in_cart_count = item_in_cart_count |  plus: 1 %}
        {% endunless %}
        {% assign handle = product.handle %}
        {% assign cross_sellHandles = cross_sellHandles | append: handle | append: ',' %}
      {% endfor %}
    {% endfor %}
    {% assign cross_sellHandles = cross_sellHandles | split: ',' | uniq %}
    ```
    Also , we will not display the product which is already in cart , for this we check ,if cart item is present is retrieved products by metafield and skip that loop iteration .
