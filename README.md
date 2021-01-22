# Javascript Complete

## Performance & Optimizations

### What is "Performance Optimization" About?

* Refer : what-is-performance.pdf

* Refer : performance-optimization-layers.pdf

* Refer : measuring-performance.pdf

### Diving Into The Browser Devtools (for Performance Measuring)

Refer : https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference
Refer : https://developers.google.com/web/fundamentals/performance/rendering/optimize-javascript-execution

### Lazy loading

* Optimize unused code with the help of lazy loading.

```js
import { products } from './products';
import { renderProducts } from './rendering';

function addProduct(event) {
  event.preventDefault();
  import('./product-management.js').then(mod => {
    mod.addProduct(event);
  })
}

function deleteProduct(productId) {
  import('./product-management.js').then(mod => {
    mod.deleteProduct(productId); // here we are passing productId with lazy loading product-management file
  })
}

function initProducts() {
  renderProducts(products, deleteProduct); //we are passing  deleteProduct to renderProducts
}

const addProductForm = document.getElementById('new-product');

initProducts();

addProductForm.addEventListener('submit', addProduct); // addProduct is lazy loaded
```

* if add product or delete product in product management were more complex functions, then you could save a lot by only importing them dynamically when they're needed instead of importing them upfront which we're doing at the moment.

### Updating The DOM Correctly

* we should not recreate dom every time when we delete a item .. if we delete a last item for that if we recreate the entire dom that won't make sense, try to use insertAdjacentElement instead of loading after unshift data and reload entire data.

```js
var s = document.getElementsByTagName("span")[0];
var h = document.getElementById("myH2");
h.insertAdjacentElement("afterend", s);
```

* While delete instead don't recreate array again gracefully remove element

```js
export function deleteProduct(prodId) {
  const deletedProductIndex = products.findIndex(prod => prod.id === prodId);
  const deletedProduct = products[deletedProductIndex];
  products.splice(deletedProductIndex, 1);
  updateProducts(deletedProduct, prodId, deleteProduct, false);
}
```

```js
export function updateProducts(product, prodId, deleteProductFn, isAdding) {
  if (isAdding) {
    const newProductEl = createElement(product, prodId, deleteProductFn);
    productListEl.insertAdjacentElement('afterbegin', newProductEl);
  } else {
    const productEl = document.getElementById(prodId);
    productEl.remove();
    // productEl.parentElement.removeChild(productEl);
  }
}
```
### Finding & Fixing Memory Leaks

* Refer : https://developers.google.com/web/tools/chrome-devtools/memory-problems
* Refer : improvement-ideas.pdf
