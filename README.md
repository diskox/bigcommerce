# Bigcommerce Stencil Cornerstone Shortocdes

## Getting all the products from the cart

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

        });

    },
    async: false

});
```

## Custom gallery for product variations based on SKU

Assumes you have list(as list can have defeault option selected upon page load) instead of swatch for SKU's (variants). In this example we are replicating look of swatch option through radio list.

**set-radio.html**
```handlebars
<label data-product-attribute-value="{{id}}" class="form-option form-option-swatch" for="attribute_{{id}}">
    <span class='form-option-variant form-option-variant--color' title="{{this.label}}" style="background-color: #{{#if id '==' 120}}1B395C{{else if id '==' 121}}232323{{else if id '==' 122}}AAC3C6{{else if id '==' 123}}D1D1D0{{else if id '==' 124}}D9B8A7{{/if}}
    "></span>
</label>
```

You'd upload all images in main products gallery and populate their description field with product variation name.

**product.js**
```javascript
function imgSwitcher() {

    $(function () {

        var swatches = $('.productView-options [data-product-option-change]'),
            colorName = $('label.form-label.form-label--alternate.form-label--inlineSmall > small'),
            selected = $('.productView-options .form-field input[type="radio"]:checked'),
            selectedVal = '';

        setTimeout(function () { selected.trigger('change'); }, 800);

        if (swatches.length > 0) {

            selectedVal = selected.val();

            $(colorName).html($('label[for="attribute_' + selectedVal + '"]').text());

            $('.form-radio', swatches).on('change', function (eventObject) {

                var colorSwatch = $('label[for="' + $(eventObject.target).attr('id') + '"]').children('span').attr('title');
                if ( colorSwatch.length > 0 ) {
                    $(colorName).html(colorSwatch);


                    // GA Enhanced Ecommerce
                    if ( item.Color !== undefined ) {
                        item.Color = colorSwatch;
                    }
                    if ( item.SKU !== undefined ) {
                        item.SKU = $('label[for="' + $(eventObject.target).attr('id') + '"]').attr('data-product-attribute-value'); 
                    }
                }

                // GA Enhanced Ecommerce
                if ( item.SKU === '121' ) {
                    item.SKU = 'BDOB050A';
                } else if ( item.SKU === '122' ) {
                    item.SKU = 'BDSM050A';
                } else if ( item.SKU === '123' ) {
                    item.SKU = 'BDGW050A';
                } else if ( item.SKU === '124' ) {
                    item.SKU = 'BDHP050A';
                } else {
                    item.SKU = 'BDMB050A';
                }

                $('.productView-thumbnail').hide();

                $('[title*="' + colorSwatch + '"]').parent().parent().show().css('visibility', 'visible');

                $('.productView-thumbnail:visible').eq(0).find('img').trigger('click');

            });
        }
    });
}

imgSwitcher();
```

## Access SKU parameteres throughout the web ##

To make it possible we have to use featured option. First, add below code to page.html, blog-post.html and blog.html if you use blog, cart.html if you need it there, home.html etc.
```
---
products:
    featured:
        limit: 2
---
```
then access like 
```
{{products.featured.[0].name}}
{{products.featured.[0].price.without_tax.value}}
{{products.featured.[0].url}}
```
and so on.


## Stencil handlebars version of var_dump
Objects
```
{{#each something}}
{{@key}} - {{this}}
{{/each}}
```

Arrays
```
{{#each something}}
{{@index}} - {{this}}
{{/each}}
```

## order-list.html getting image of correct product variant instead of having main SKU's image
```
<img class="account-product-image lazyload"
    data-sizes="auto"
    src="{{cdn 'img/loading.svg'}}"
    data-src="{{#if this.items.[0].productAttributesList.[0].value}}/content/assets/{{this.items.[0].productAttributesList.[0].value}}.jpg{{else}}{{getImage image 'productthumb_size' (cdn ../theme_settings.default_image_product)}}{{/if}}"
    alt="{{#if this.items.[0].productAttributesList.[0].value}}{{this.items.[0].productAttributesList.[0].value}}{{else}}{{image.alt}}{{/if}}"
    title="{{#if this.items.[0].productAttributesList.[0].value}}{{this.items.[0].productAttributesList.[0].value}}{{else}}{{image.alt}}{{/if}}">
```

## Bigcommerce GA Enhanced Ecommerce
- It's possible to fully implement everything from https://developers.google.com/tag-manager/enhanced-ecommerce via some custom code. Same goes for Facebook tracking too. As there's a lot of different files and code to support it, reach out to atonik@kaleb.co if you need assistance.
