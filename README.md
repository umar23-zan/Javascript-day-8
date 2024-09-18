# Javascript-day-8
## Modules
14a. In checkout.html, find the element that displays "3 items" and delete the text inside. It should display "Checkout ()" when the page loads.
```
 href="amazon.html"></a>)
```
14b. In checkout.js, when the page loads, calculate the actual quantity of products in the cart, and display it in the header: `$(quantity) items Hints: In amazon.js, inside the function updateCartQuantity(), we wrote

some code to calculate the cart quantity. Reuse this code. In checkout.html, you'll need to add a class to the element so you can select it with the DOM and change the innerHTML
```
 Checkout (<a class="return-to-home-link js-return-to-home-link"

let cartQuantity = 0;

cart.forEach((cartItem) => {
  cartQuantity += cartItem.quantity;
});

document.querySelector('.js-return-to-home-link')
  .innerHTML = `${cartQuantity} items`;
```
14c. Continuing from 14b, also calculate and display the quantity in the header when clicking "delete". First, create a function updateCartQuantity() and put the code

from 14b inside

• Run this function when loading the page and when clicking delete (notice that this function doesn't conflict with updateCartQuantity in amazon.js because we're using modules)
```
 updateCartQuantity();

function updateCartQuantity() {
  let cartQuantity = 0;

  cart.forEach((cartItem) => {
    cartQuantity += cartItem.quantity;
  });

  document.querySelector('.js-return-to-home-link')
    .innerHTML = `${cartQuantity} items`;
}

updateCartQuantity();
```
14d. If we open the home page (amazon.html) notice the cart quantity in the top-right always starts at 0. Cart

Remove the text "0" (so the cart quantity starts as blank) When the page loads, calculate the cart quantity and display it in the top right (reuse the updateCartQuantity function)
```
<div class="cart-quantity js-cart-quantity"></div>

updateCartQuantity();
```
14e. Inside the function updateCartQuantity, we have some code that calculates the cart quantity (creates a variable, loops through the cart, and adds up all the quantities). Notice this code is repeated in checkout.js and amazon.js.

Create a function calculateCartQuantity() and move this code into the function so we can reuse it

Put calculateCartQuantity() inside cart.js (because this code relates to the cart) and use export/import to share it between the 2 files
```
export function calculateCartQuantity() {
  let cartQuantity = 0;

  cart.forEach((cartItem) => {
    cartQuantity += cartItem.quantity;
  });

  return cartQuantity;
}

import {cart, addToCart,
  calculateCartQuantity} from '../data/cart.js';
const cartQuantity = calculateCartQuantity();

import {cart, removeFromCart,
  calculateCartQuantity} from '../data/cart.js';
const cartQuantity = calculateCartQuantity();

```
Challenge Exercises

We'll make "Update" interactive step-by-step.

14f. In checkout.js, get all the "Update" links from the page and add a "click" event listener to each link. Also, attach the productid to each link. When clicking the link, get the productid and console.log() it.
```
<span class="update-quantity-link link-primary js-update-link"
              data-product-id="${matchingProduct.id}">

document.querySelectorAll('.js-update-link')
  .forEach((link) => {
    link.addEventListener('click', () => {
      const productId = link.dataset.productId;
      console.log(productId);
    });
  });
```
14g. Add 2 HTML elements after the "Update" link: An <input class="quantity-input"> (for entering a new quantity) A <span class="save-quantity-link">Save</span> (to save the quantity) Style the <input> and set its width to 30px (put the styles in the file: styles/pages/checkout/checkout.css) Quanity 2
```
<input class="quantity-input">
            <span class="save-quantity-link link-primary">Save</span>
```

14h. Make "Save" appear when clicking "Update"

When clicking "Update", get the cart-item-container for the product, and add the class "is-editing-quantity" to the container (use.classList) In checkout.css, style the <input> & "Save" link and add display: none; (they will be invisible at the start) The CSS ".is-editing-quantity.quantity-input {...}" styles elements with class "quantity-input" inside an element with class "is-editing-quantity" Use this, and "display: initial;" (resets the display property) to make the <input> appear when editing the quantity. Same for the "Save" link
```
 const container = document.querySelector(
        `.js-cart-item-container-${productId}`
      );
      container.classList.add('is-editing-quantity');
    });
```
141. Using similar CSS selectors as 14h, make the quantity and "Update" link disappear when editing the quantity.
```
.is-editing-quantity .quantity-label {
  display: none;
}

.is-editing-quantity .update-quantity-link {
  display: none;
}
```
14j. Now we'll implement switching between "Update" and "Save"

Add "click" event listeners to all "Save" links. When clicking "Save", do the opposite of "Update": get the cart-item-container for the product, and remove the class "is-editing-quantity". This should reverse all the styling that's applied when editing the quantity.
```
 <span class="save-quantity-link link-primary js-save-link"
              data-product-id="${matchingProduct.id}">
              Save
            </span>
```
14k. When clicking "Save", use the DOM to get the quantity <input> for the product, and get the value inside (remember to convert this value to a number). This will be the new quantity of the product in the cart.
```
 <input class="quantity-input js-quantity-input-${matchingProduct.id}">
```
141. In cart.js, create a function updateQuantity(productid, newQuantity) which will find a matching productid in the cart, and update its quantity to the new quantity (remember to save to storage after). Then, import and use this function when clicking a "Save" link
```
export function updateQuantity(productId, newQuantity) {
  let matchingItem;

  cart.forEach((cartItem) => {
    if (productId === cartItem.productId) {
      matchingItem = cartItem;
    }
  });

  matchingItem.quantity = newQuantity;

  saveToStorage();
}

import {
  cart,
  removeFromCart,
  calculateCartQuantity,
  updateQuantity
} from '../data/cart.js';
```
14m. Now that we've updated the quantity in the cart, the last step is to update the quantity in the HTML. Update these 2 places: Inside the product Quantity 2

• In the header at the top Checkout (3 items)
```
 Quantity: <span class="quantity-label js-quantity-label-${matchingProduct.id}">${cartItem.quantity}</span>
```
14n. Try to come up with more features to add to the "Update" link like: Add validation (check the new quantity is >= 0 and < 1000) Add keyboard support (allow updating by pressing 'Enter')
```
if (newQuantity < 0 || newQuantity >= 1000) {
        alert('Quantity must be at least 0 and less than 1000');
        return;
      }
      updateQuantity(productId, newQuantity);

      const container = document.querySelector(
        `.js-cart-item-container-${productId}`
      );
      container.classList.remove('is-editing-quantity');

```
