# Nasher Miles Homepage

>  In this whole web application , [Flickity Slider](https://flickity.metafizzy.co/) is being used , refer docs for slider customization

  
# Header

- ### Customized Predictive search , with placeholder animation and adding Most Searched And Bought collection  model below the search .


- ### Nav items contains link to collection , which also contains further menu with list of collection (mega-menu) - can be cusomized in admin panel

- ### Account login and handling is supported by GoKwik

- ### Nav item contains store locator which is also supported by Stockist store locator

  
# Home page sections

- ### To decrease the load time and increase performance , the sections are lazy loaded using intersection observer and custom component .
	 we need to wrap the  section with lazy-section custom component created , to which we need to lazy load .
 
	Below code for reference .
	```
	 fetchSections() {
    LazySection.sectionsToFetch.push(this.section);
    const sectionsToFetchBatch = LazySection.sectionsToFetch.length == 5 ? LazySection.sectionsToFetch.splice(0, 5) : LazySection.sectionsToFetch;

    // Check if there is an ongoing network request
    if (LazySection.abortController) {
      LazySection.abortController.abort();
    }

    LazySection.abortController = sectionsToFetchBatch.length < 5 && new AbortController();

    fetch(window.location.pathname + '?sections=' + sectionsToFetchBatch.join(','), LazySection.abortController.signal ? { signal: LazySection.abortController.signal } : {})
      .then((response) => response.json())
      .then((json) => {
        for (const [key, value] of Object.entries(json)) {
          const sectionContent = new DOMParser().parseFromString(value, 'text/html').getElementById('shopify-section-' + key);
 
          if (sectionContent && sectionContent.innerHTML.trim().length) {
            const section = document.getElementById('shopify-section-' + key);
            section.innerHTML = sectionContent.innerHTML;

            // Reinjects the script tags to allow execution. By default, scripts are disabled when using element.innerHTML.
            section.querySelectorAll('script').forEach((oldScriptTag) => {
              const newScriptTag = document.createElement('script');
              Array.from(oldScriptTag.attributes).forEach((attribute) => {
                newScriptTag.setAttribute(attribute.name, attribute.value);
              });
              newScriptTag.appendChild(document.createTextNode(oldScriptTag.innerHTML));
              oldScriptTag.parentNode.replaceChild(newScriptTag, oldScriptTag);
            });

            //Re-initialize flickity and rest of the functionality
            window.ft("*")
          }
        }

        if (sectionsToFetchBatch.length < 5)
          LazySection.sectionsToFetch = [];
      })
      .catch((e) => {
        console.warn(e);
      });
    setTimeout(function() { HulkappWishlist?.init(); }, 3000);
  }
	```

- ### Testimonial section is supported by app

- ### Featured Collection section , contains product cards with color swatches and available size variants , on click it changes , the product card information to clicked variant's information using template rendering [layout rendering].
	 - On selecting the specific variant swatch , using layout rendering [template rendering] we fetch that alternate product template , which we defined in liquid , where the product card is returned for specific product

	- Then We replace the the card with new fetched card .
	
	#### here's the resposible code for swatch change using template rendering .

	```
	function fetchProductData(target, sizeOption) {
	const productHandle = target.dataset.productHandle;
	const productUrl = `/products/${productHandle}?view=card`;
	const container = target?.closest('.product-grid-item');
	if (target.getAttribute('aria-selected') === 'true') return;
	if (!productHandle || !container) return;
	fetch(productUrl)
		.then((response) => response.text())
		.then((data) => { 
			// this will update product card except color swatches
			const html = new DOMParser().parseFromString(data, "text/html");
			//using this html from fetch , we will update the card details 
		})
		.catch((error) => {
			console.error('Error fetching product data:', error);
		}).finally(() => {
			if (!sizeOption) {
				container.querySelector('[aria-selected="true"]')?.setAttribute('aria-selected', 'false');
				target.setAttribute('aria-selected', 'true');
			}
		});
	}
	```

- ### Also , product card contains , add to cart button , which adds specific product variant , based on swatches , to the cart .

- ### Home page overlay modals

	 - #### Customer login overlay modal , which comes from app .
	 - #### Cart drawer overlay modal , triggered when clicking on cart icon , or when a product is added .
	 - #### Notification overlay , to allow notification from web application in browser
	 - #### Whatsapp icon , which is fixed to bottom left , takes you to web whatsapp page , to start chatting with nasher miles channel .

  
