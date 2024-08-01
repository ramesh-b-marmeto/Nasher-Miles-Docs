# Nasher Miles Collection Page

> The Collection page , product list filters can be added or deleted in search and discovery app which is given by shopify for free .

  ## **Collection Page Features**

  - ### The Usp blocks in collection page is added through blocks in Collection Pages section , for this inside collection pages section , we create usp block and display those blocks by looping through it , inside any container div .

  - ### The product list , display have 2 options , 
    > on click of the options , related class will be replaced in collection list container class, on which , styles has been coded in css files for respective display options .
    - 3 columns per row in desktop , and 2 per row in small and medium 
    - one product taking full parent width , shoqing all details along the row .

    #### Below code for reference logic
    ```
     this.gridViewButtons = this.querySelectorAll('.grid-view__option');
      this.gridViewButtons.forEach((eachButton) => {
      eachButton?.addEventListener('click', () => {
        this.changeGridView(eachButton);
      })
    })
  
 
    changeGridView(eachButton) {
      const collectionProductsElement = document.querySelector('.collection__products');
      const gridViewValue = eachButton.getAttribute('data-grid-option')
      this.gridViewButtons.forEach((each)=> {
        each.classList.remove('active-grid')
      })
      eachButton.classList.add('active-grid');
      if (collectionProductsElement.classList.contains('three-grid-view')) {
        collectionProductsElement.classList.remove('three-grid-view')
      } else if (collectionProductsElement.classList.contains('two-grid-view')) {
        collectionProductsElement.classList.remove('two-grid-view')
      }else if (collectionProductsElement.classList.contains('one-grid-view')) {
        collectionProductsElement.classList.remove('one-grid-view')
      }
      collectionProductsElement.classList.add(`${gridViewValue}-grid-view`)
    }
    ```
    #### on click of respective grid option button , first we remove , `active-grid` button from all option buttons ,  we will add class `active grid` to that button which is clicked , and extract the value by dat attributes , now , in the collection container , first we remove existing display grid class , now we add the extracted , grid value from button to the conatiner classes .

  - ### Product card in collection list , have swatches of size and color 
      #### Working of Variant change in product card on clicking of swatches :
  
       - On selecting the specific variant swatch , using layout rendering [template rendering] we fetch that alternate product template , which we defined in liquid , where the product card is returned for specific product

       - Then We replace the the card with new fetched card .
       
      

  - ### Filters and sort functionality is supported by liquid objects , we just have to update the url with the right paremeters and , shopify cli will filter and sort and give the html , we have replace the updates section with existing with section rendering .
    - #### We have to fetch the collection page with parameters for sorting and filtering .
    - #### Before that we have to prepare the parameters to send , by getting all the option input for filters and sort , we have to extract the values and prepare the parameters  .

      **Example url for filter and sort parameter for reference**
      >https://nashermiles.com/collections/boston?sort_by=price-descending&filter.v.availability=1&filter.v.availability=0&filter.v.price.gte=1283&filter.v.price.lte=&filter.p.m.custom.color=Black&filter.p.m.custom.color=Orange&filter.p.m.custom.material=Polypropylene

      we have to add & between all parameters 

  - ### Pagination of products list acheieved through , infinte loading , where we fetch next page and append that to  collection list div .

    #### Below reference code for updating product list in collection page.
    ```
    this.request = new XMLHttpRequest();

        this.request.onreadystatechange = function success() {
          if (!this.request.responseXML) {
            return;
          }
          if (!this.request.readyState === 4 || !this.request.status === 200) {
            return;
          }

          const newContainer = this.request.responseXML.querySelector(this.settings.container);
          const newPagination = this.request.responseXML.querySelector(this.settings.pagination);

          this.containerElement.insertAdjacentHTML('beforeend', newContainer.innerHTML);
          if (typeof newPagination === 'undefined' || newPagination === null) {
            this.removePaginationElement();
          } else {
            this.paginationElement.innerHTML = newPagination.innerHTML;

            if (this.settings.callback && typeof this.settings.callback === 'function') {
              this.settings.callback(this.request.responseXML);
            }
        }
        }
    ```
  - ### Recently viewed products section ,where everytime a product page is opened/viewed , we will store that product id in local storage . then , when section is rendered in other page , it will fetch recently viewed product by getting local stoarge id's .
    - We use search page with different template to get the results using id .

      > for eg: search.recently-viewed.liquid template file
    - We use search object inside this template , to get the result , where this search object will get it by query paramter passed to url . here we define produt list slider to display products .
      ```
      //search.recently-viewed.liquid template file
      {%- layout none -%}
      {%- section 'marmeto-recently-viewed-products' -%}
      ```
    - when we fetch search page with this alternate template , we will get the product slider html with recently viewed products query passed .
    - Then we replace the inner html with resultant html we got by template rendering .
      Below code for refernce :
      ```
      fetch(("/search?view=marmeto-recently-viewed-products&type=product&q=").concat(queryString), {
      credentials: 'same-origin',
      method: 'GET'
      })
      .then(function (response) {
        response.text().then(function (content) {
          var blankDivElement = document.createElement('div');
          blankDivElement.innerHTML = content;
          
          _this.container.querySelector('.mm-recentlyviewed__container').innerHTML = blankDivElement.querySelector('[data-section-type="mm-recently-viewed-products"] .mm-recentlyviewed__container').innerHTML;
          _this.container.parentNode.style.display = 'block';
          
          _this.initSlider();
        })
      })
      ```
