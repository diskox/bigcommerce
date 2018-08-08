#Bigcommerce Cornerstone shortocdes

##Getting all the products from the cart

```javascript
$.ajax({

    url: '{shopPath}/cart.php',
    success: function (result) {

        //Getting all the products from the cart

        cart_htmlPage = $(result);
        cart_items_found = $('.cart-item .cart-item-title', cart_htmlPage);

        $(cart_items_found).each(function (i, el) {

            //Retrieving the product’s details page URL
            cart_product_link = '{shopPath}/' + $(this).find('.cart-item-name a').attr('href');
            cart_variation = $(this).find('.definitionList-value').text().trim();
            console.log(cart_variation);

            // Now comparing with the product's URL found from category page
            // if (product_link == cart_product_link) {

            // // OK, we got it, so do what you want to do here 🙂
            // console.log('Found. Item number: ' + i);

            // }

        });

    },
    async: false

});
```
