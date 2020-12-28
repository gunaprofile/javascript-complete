# Javascript Complete

## Classes & Object-oriented Programming (OOP)

### What is "Object-oriented Programming" (OOP)?

* It's an approach, it's a way of writing and of structuring your code in the end, it's a way of thinking or reasoning about your code and planning your code and the idea behind object oriented programming is that you work with kind of real life entities in your code.

* Refer : whats-oop.pdf

### Defining & Using a First Class

* Classes allow us to build objects in an easier way or to build objects based on some blueprint to be precise.So we have objects and we have classes.

* Now classes are something we can create in Javascript too, which do not replace objects but instead which allow us to define blueprints for objects so that we can easily recreate objects based on these classes because indeed objects then are also called instances of classes.

* So we can create an object based on some class thereafter and therefore a class is just a definition of how the object looks like, which properties and methods it has, the place where we store our logic and then the object is the concrete thing we build based on that class with which we work in our code. So this class based creation which I'll show you in this module is an alternative to using object literals.

* Refer : classes-and-instances.pdf

```js
// we could create a function that builds us such an object, well a class in the end is such a function,

class Product { // Make sure to use capital character for every sub words 

// what's inside of that class is basically your blueprint of how an object created based on that class should look like.

  // title = 'DEFAULT';
  // imageUrl;
  // description;
  // price;

  constructor(title, image, desc, price) {
    //  When we create a product based on such a class, it would be nice if we could create it with some initial values
    this.title = title;
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}

const productList = {
  products: [
    new Product( // New in the end is a keyword Javascript understands that together with such a function execution which is based on a class,
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ],
  render() {
    const renderHook = document.getElementById('app');
    const prodList = document.createElement('ul');
    prodList.className = 'product-list';
    for (const prod of this.products) {
      const prodEl = document.createElement('li');
      prodEl.className = 'product-item';
      prodEl.innerHTML = `
        <div>
          <img src="${prod.imageUrl}" alt="${prod.title}" >
          <div class="product-item__content">
            <h2>${prod.title}</h2>
            <h3>\$${prod.price}</h3>
            <p>${prod.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
      prodList.append(prodEl);
    }
    renderHook.append(prodList);
  }
};

productList.render();

```
* what's inside of that class is basically your blueprint of how an object created based on that class should look like.

* So you define which properties such an object should have and which methods it should have.

* New in the end is a keyword Javascript understands that together with such a function execution which is based on a class,

*  When we create a product based on such a class, it would be nice if we could create it with some initial values.

* I want to make sure that when we call new product, we can kind of pass our initial values here to new product so that we can create a new object with some initial values. Of course by the way, we could of course create a new product like this and then just assign values with the dot notation but that's not what we want to do, we want to create it in one go,

### Working with Constructor Methods


* It would be nice if we could call new product and just pass information to that product function because we call it like a function, wouldn't it be nice if you could pass some arguments like a title, a price and so on here?

* Note!!!! while adding method shorthand syntax don't add semi-colan at the end even if you add multiple methods add at the new line don't add semi-colan.

* special method which Javascript executes for us is called the constructor method or the constructor function. We add constructor here as a method name and that's a reserved name

* The idea behind a constructor is that it can accept arguments like any normal method.

* in the curly braces and that's the interesting thing now, you can assign the values you're getting here for these parameters, so you can assign the arguments you're getting, to your class field, so to the properties of the object when it is instantiated then and you do this with the good old this keyword.

```js
class Product { 
  constructor(title, image, desc, price) {
    this.title = title; // This in here refers to your class or to be precise, since this class will be used to create an object, to the object that is created.
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}
```
* So now we can pass in a value

```js
products: [
    new Product(
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ],
```
* So now we're using this class as a blueprint and the huge advantage here is that we now have an easy, reusable way of creating objects which are guaranteed to always look the same, it's impossible for us to omit properties or to mistype properties because it's all defined in here in this class definition.

### Fields vs Properties

* let me come back to that class field versus property thing because it is important to get that right.

* Refer : class-properties-fields-methods.pdf

* so title is a class property, category is a class field but it's important to know that in the end, this is just a theoretical separation, a field becomes a property, when we create an object based on the class, we just call it property in the constructor function right away because the constructor gets called during that object creation process so there we get a property right away.

* You don't really have to memorize this difference though, in the end fields are like properties, we define them in a class so that we have a property when we create that object based on the class.

* In constructor property we overwrite that default logic where every field would be translated to a property because you manually assign a value to that property and therefore add that property anyways in the constructor.

### Using & "Connecting" Multiple Classes

*  Let's add another class because we can not just create classes which predefine objects which are basically data containers, we can also create classes for objects which hold more logic so that in the end our entire application logic is split up across multiple classes which we then just connect in some clever way.

```js
class Product {
  // title = 'DEFAULT';
  // imageUrl;
  // description;
  // price;

  constructor(title, image, desc, price) {
    this.title = title;
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}

// now I want to outsource the logic for a single product, so what we render for a single product into another class.

// so if I refer to a class inside of product list, I don't have to define it in front of product list, I could define it thereafter, Javascript will be aware of all classes,
class ProductItem {
  constructor(product) {
    this.product = product; // adds product poperty to the productItem's product
  }

  render() {
    const prodEl = document.createElement('li');
    prodEl.className = 'product-item';
    prodEl.innerHTML = `
        <div>
          <img src="${this.product.imageUrl}" alt="${this.product.title}" >
          <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>\$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
    return prodEl;
  }
}

class ProductList {
  products = [
    new Product(
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ];

  constructor() {}

  render() {
    const renderHook = document.getElementById('app');
    const prodList = document.createElement('ul');
    prodList.className = 'product-list';
    for (const prod of this.products) {
      const productItem = new ProductItem(prod);
      const prodEl = productItem.render();
      prodList.append(prodEl);
    }
    renderHook.append(prodList);
  }
}

const productList = new ProductList();
productList.render();
```
###  Binding Class Methods & Working with "this"

```js
class Product {
  // title = 'DEFAULT';
  // imageUrl;
  // description;
  // price;

  constructor(title, image, desc, price) {
    this.title = title;
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}

class ProductItem {
  constructor(product) {
    this.product = product; // bind method calling this class on click button
  }

  addToCart() {
    console.log('Adding product to cart...');
    console.log(this.product); 
  }

  render() {
    const prodEl = document.createElement('li');
    prodEl.className = 'product-item';
    prodEl.innerHTML = `
        <div>
          <img src="${this.product.imageUrl}" alt="${this.product.title}" >
          <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>\$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
    const addCartButton = prodEl.querySelector('button');

    // Now as you learned in that object module, Javascript then binds this to the source of that event,
    // so to that button and not to your your class or the object where this effectively runs on later.

    // addCartButton.addEventListener('click', this.addToCart);

    // The solution or one possible solution is to use bind here and bind this, so that means that we bind this inside of add to cart,
    // so what this refers to instead of addToCart method to the same thing this refers to in this place here
    addCartButton.addEventListener('click', this.addToCart.bind(this)); // bind method
   
    return prodEl;
  }
}

class ProductList {
  products = [
    new Product(
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ];

  constructor() {}

  render() {
    const renderHook = document.getElementById('app');
    const prodList = document.createElement('ul');
    prodList.className = 'product-list';
    for (const prod of this.products) {
      const productItem = new ProductItem(prod);
      const prodEl = productItem.render();
      prodList.append(prodEl);
    }
    renderHook.append(prodList);
  }
}

const productList = new ProductList();
productList.render();
```

### Static Methods & Properties

* Refer : static-fields-methods.pdf

* thus far, we only work with instances, we always use all these classes by using new because we need different product items which have the same structure but hold different data, with static properties and static methods, we have a class which is not instantiated and which therefore always works on the same data for example but that's exactly what we can utilize here.

```js
class Product {
  // title = 'DEFAULT';
  // imageUrl;
  // description;
  // price;

  constructor(title, image, desc, price) {
    this.title = title;
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}

class ShoppingCart {
  items = [];

  addProduct(product) {
    this.items.push(product);
    this.totalOutput.innerHTML = `<h2>Total: \$${1}</h2>`;
  }

  render() {
    const cartEl = document.createElement('section');
    cartEl.innerHTML = `
      <h2>Total: \$${0}</h2>
      <button>Order Now!</button>
    `;
    cartEl.className = 'cart';
    this.totalOutput = cartEl.querySelector('h2');
    return cartEl;
  }
}

class ProductItem {
  constructor(product) {
    this.product = product;
  }

  addToCart() {
    App.addProductToCart(this.product); // Now important here I'm calling add product to cart on app and I'm forwarding this product, referring to my product in product item. 
  }

  render() {
    const prodEl = document.createElement('li');
    prodEl.className = 'product-item';
    prodEl.innerHTML = `
        <div>
          <img src="${this.product.imageUrl}" alt="${this.product.title}" >
          <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>\$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
    const addCartButton = prodEl.querySelector('button');
    addCartButton.addEventListener('click', this.addToCart.bind(this));
    return prodEl;
  }
}

class ProductList {
  products = [
    new Product(
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ];

  constructor() {}

  render() {
    const prodList = document.createElement('ul');
    prodList.className = 'product-list';
    for (const prod of this.products) {
      const productItem = new ProductItem(prod);
      const prodEl = productItem.render();
      prodList.append(prodEl);
    }
    return prodList;
  }
}

class Shop {
  
  render() {
    const renderHook = document.getElementById('app');

    this.cart = new ShoppingCart();   //  we can store this reference to the cart object in a property, so this cart is equal to shopping cart.
    const cartEl = this.cart.render(); // here also we used  this.cart as ShoppingCart instance
    const productList = new ProductList();
    const prodListEl = productList.render();

    renderHook.append(cartEl);
    renderHook.append(prodListEl);
  }
}

class App {
  static cart; // it's optional but still I would say it's a good practice, you can also add a static field here to the app class which is named cart so that we make it clear that we have these static cart property.

  static init() { 
    const shop = new Shop();
    shop.render(); // first render this shop and then use shop.cart below
    this.cart = shop.cart; // just be aware that if you would use this in here, you would always refer to the class itself 
  }

  static addProductToCart(product) {
    this.cart.addProduct(product);
  }
}

 // now you therefore also don't create a new app like that or at least you could do that
// but to call init, you don't do it, instead you can just call app referring to the class itself like this, .init like this
App.init(); // and this will execute this init method directly on the class itself.

```
* Now again, we therefore have no app object we work with, instead we directly operate on that class and hence this will work but it's a different approach.

###  First Summary & Classes vs Object Literals

* Refer : classes-vs-object-literals.pdf

* So now we had a first look at classes, we're using some classes to split our logic and we're also using classes in different ways.

* We have our class with the static properties and the static methods to glue together some of the other classes and that's just one possible use case, static methods and static properties are always a good idea if you want to share some functionality across different parts of your application

* let me take a step back and go back to the relation between classes and the objects you work with in Javascript. Refer : classes-vs-object-literals.pdf

* For one of course it's important to realize that if you're not using a class in a static way like this here but instead by instantiating with new, what new shop in this case here returns, so what's stored in the shop constants here is a regular Javascript object or a reference to that object to be precise.

```js
 const shop = new Shop();
```
* So it's in the end the same as what you get with that object literal notation, I really want to emphasize this

* As a result, you can of course use all the things on these objects as you can do on normal objects because these are just normal objects. You can for example use destructuring to get the cart out of shop if you wanted to do that, something like that is possible, it's really important to understand this relation.

```js
 const shop = new Shop();
 const { cart } = shop;
```

* It's also important to understand when to use classes. You might think that with classes, the old way of creating objects with the object literal notation is obsolete (ie no longer useful because something better has been invented) and that would be wrong, It's not obsolete,

* it's great to use that object literal notation if you have some data collection, a couple of variables which you in the end want to group together and where you only want to do this once or in one place of your app, of your code and you don't plan on reusing that. So not as we're doing it with the product where we need that in different places, where we want to ensure that they always have the same structure but where you quickly want to create such an object on the fly, that's a perfect use case for using object literals.

* that's a perfect use case for using object literals. They're quick and easy to use, you have no extra overhead and you also have a small performance benefit versus this class based instantiation which is quickly made up for though if you would unnecessarily create multiple objects with object literals instead of using classes and therefore sharing code, just to also say that.

* So classes in general are a good idea if you have some logic which you want to reuse, if you want to recreate the same type of object with the same structure and the same attached logic over and over again

* You have a little bit more overhead initially because you need to write that class definition but thereafter, you have that easy object duplication, of course not really a duplication instead new objects are created with that same internal structure but with different data, depending on how your class works of course.

* So both is important and both has its place in Javascript and it's simply also something that comes with experience and with you working with Javascript, that you get a better feeling for when to use which.

### Getters & Setters

```js
class ShoppingCart {
  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: \$${this.totalAmount.toFixed(2)}</h2>`; // getter name ad totalAmount
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, curItem) => prevValue + curItem.price,
      0
    );
    return sum;
  }

  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems; // setter name  this.cartItems , updatedItems is the value to the  cartItems
  }

  render() {
    const cartEl = document.createElement('section');
    cartEl.innerHTML = `
      <h2>Total: \$${0}</h2>
      <button>Order Now!</button>
    `;
    cartEl.className = 'cart';
    this.totalOutput = cartEl.querySelector('h2');
    return cartEl;
  }
}
```
###  Introducing Inheritance

* Refer : inheritance.pdf

* In this application, we get a couple of classes - product item, product list and also shopping cart and what they all have in common is that they have a render method and in that render method, we do different things but we always create a new element, we then add stuff to that element, for example with innerHTML, we then return that element.

* Now we duplicate that logic, of course the exact configuration, for example the class name and the tag, that differs but the general logic always is the same. So whilst we do have different logic for what we then add to this element, the creation and configuration basically as I just said multiple times is the same and in such cases, we can use a concept called inheritance,

* The idea behind inheritance is that we have some base class, let's say a post if we're building a social network, which holds a couple of properties and/or methods, in this example three properties, which we also need in other classes. Let's say we have a specialized version of that post which is an image post which also has all these properties but also in addition has an imageUrl and an image description and then we also have another specialized version of a post you can make, a video post which also has title, text and creatorId but which then also needs a video URL and let's say a rating regarding the age you've got to have to watch the video.

* So we can of course build multiple classes but that's suboptimal because we duplicate a lot of code, all that purple code - title, text, creatorId, these properties we duplicate them all the time. So instead it would be nice if you could extend that base class and of course you can do that in Javascript and therefore inherit all that purple stuff.

* So you can extend that class and then you get this shared purple content automatically in the specialized classes and you can still add your old extra properties or logic in these subclasses here, you can also override things shared in the base class if you would want to do that.

* Refer : inheritance.pdf

###  Implementing Inheritance

```js

class ElementAttribute {
  constructor(attrName, attrValue) {
    this.name = attrName;
    this.value = attrValue;
  }
}


class Component {
  constructor(renderHookId) {
    this.hookId = renderHookId;
  }

  createRootElement(tag, cssClasses, attributes) { // createRootElement method 
    const rootElement = document.createElement(tag);
    if (cssClasses) {
      rootElement.className = cssClasses;
    }
    if (attributes && attributes.length > 0) {
      for (const attr of attributes) {
        rootElement.setAttribute(attr.name, attr.value);
      }
    }
    document.getElementById(this.hookId).append(rootElement);
    return rootElement;
  }
}
```
* we need to extends this component class in our child class

```js
class ShoppingCart extends Component { // here we extends our base class - Component class

  constructor(renderHookId) { 
    super(renderHookId); // we need to pass renderHookId to the base component class 
  }

  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: \$${this.totalAmount.toFixed(2)}</h2>`;
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, curItem) => prevValue + curItem.price,
      0
    );
    return sum;
  }

  
  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems;
  }

  render() {
    // since we extended  this will refer to this object and as I said, this object will hold everything or will have access to everything that's part of the parent class as well,
    const cartEl = this.createRootElement('section', 'cart'); // here we called our base Component class method
    cartEl.innerHTML = `
      <h2>Total: \$${0}</h2>
      <button>Order Now!</button>
    `;
    this.totalOutput = cartEl.querySelector('h2');
  }
}
```
* How we will get hookId ??

```js
class Shop {
  render() {
    const renderHook = document.getElementById('app');

    this.cart = new ShoppingCart('app'); // 'app' --> renderHookId
    this.cart.render();
    const productList = new ProductList();
    const prodListEl = productList.render();

    renderHook.append(prodListEl);
  }
}
```
###  Using Inheritance Everywhere

```js
class Product {
  // title = 'DEFAULT';
  // imageUrl;
  // description;
  // price;

  constructor(title, image, desc, price) {
    this.title = title;
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}

class ElementAttribute {
  constructor(attrName, attrValue) {
    this.name = attrName;
    this.value = attrValue;
  }
}

class Component {
  constructor(renderHookId) {
    this.hookId = renderHookId;
  }

  createRootElement(tag, cssClasses, attributes) {
    const rootElement = document.createElement(tag);
    if (cssClasses) {
      rootElement.className = cssClasses;
    }
    if (attributes && attributes.length > 0) {
      for (const attr of attributes) {
        rootElement.setAttribute(attr.name, attr.value);
      }
    }
    document.getElementById(this.hookId).append(rootElement);
    return rootElement;
  }
}

class ShoppingCart extends Component {
  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: \$${this.totalAmount.toFixed(
      2
    )}</h2>`;
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, curItem) => prevValue + curItem.price,
      0
    );
    return sum;
  }

  constructor(renderHookId) {
    super(renderHookId);
  }

  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems;
  }

  render() {
    const cartEl = this.createRootElement('section', 'cart');
    cartEl.innerHTML = `
      <h2>Total: \$${0}</h2>
      <button>Order Now!</button>
    `;
    this.totalOutput = cartEl.querySelector('h2');
  }
}

class ProductItem extends Component {
  constructor(product, renderHookId) {
    super(renderHookId); // we shold call this first so that the base class is fully initialaised
    this.product = product;
  }

  addToCart() {
    App.addProductToCart(this.product);
  }

  render() {
    const prodEl = this.createRootElement('li', 'product-item');
    prodEl.innerHTML = `
        <div>
          <img src="${this.product.imageUrl}" alt="${this.product.title}" >
          <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>\$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
    const addCartButton = prodEl.querySelector('button');
    addCartButton.addEventListener('click', this.addToCart.bind(this));
  }
}

class ProductList extends Component {
  products = [
    new Product(
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ];

  constructor(renderHookId) {
    super(renderHookId);
  }

  render() {
    this.createRootElement('ul', 'product-list', [
      new ElementAttribute('id', 'prod-list') // In UL element we are setting id='prod-list'
    ]);
    for (const prod of this.products) {
      const productItem = new ProductItem(prod, 'prod-list'); // renderHookId --> 'prod-list'
      productItem.render();
    }
  }
}

class Shop {
  render() {
    this.cart = new ShoppingCart('app'); // ShoppingCart render in 'app' renderHookId
    this.cart.render();
    const productList = new ProductList('app'); // ProductList render in 'app' renderHookId
    productList.render();
  }
}

class App {
  static cart;

  static init() {
    const shop = new Shop();
    shop.render();
    this.cart = shop.cart;
  }

  static addProductToCart(product) {
    this.cart.addProduct(product);
  }
}

App.init();
```
### Overriding Methods and the super() Constructor

* One thing I want to do here is these render calls here right are kind of redundant, we create a product list and then I manually call render, well that should be done as part of the creation process I think because I'm always doing it thereafter manually.

```js
class Shop {
  render() {
    const renderHook = document.getElementById('app');

    this.cart = new ShoppingCart('app'); // 'app' --> renderHookId
    this.cart.render();
    const productList = new ProductList();
    const prodListEl = productList.render();

    renderHook.append(prodListEl);
  }
}
```
###  Using Inheritance Everywhere

```js
class Product {
  constructor(title, image, desc, price) {
    this.title = title;
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}

class ElementAttribute {
  constructor(attrName, attrValue) {
    this.name = attrName;
    this.value = attrValue;
  }
}

class Component {
  constructor(renderHookId) {
    this.hookId = renderHookId;
    this.render()// since we are calling the parent constructor anyway, since since we're sharing this parent, let's just do it in the parent base component itself. 
    // this.render() in the parent class will refer to the render method in the to-be-created object which is based on the subclass.
    // inside of a constructor,This will refer to the object that is being created,
  }

  render(){
    // The only thing I will now also do there is I will add a render method here and this is an empty method, so it doesn't do anything useful here.
    // sub-class render method will override (fully replace) this method
  }

  createRootElement(tag, cssClasses, attributes) {
    const rootElement = document.createElement(tag);
    if (cssClasses) {
      rootElement.className = cssClasses;
    }
    if (attributes && attributes.length > 0) {
      for (const attr of attributes) {
        rootElement.setAttribute(attr.name, attr.value);
      }
    }
    document.getElementById(this.hookId).append(rootElement);
    return rootElement;
  }
}

class ShoppingCart extends Component {
  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: \$${this.totalAmount.toFixed(
      2
    )}</h2>`;
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, curItem) => prevValue + curItem.price,
      0
    );
    return sum;
  }

  constructor(renderHookId) {
    super(renderHookId);
  }

  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems;
  }

  render() {
    const cartEl = this.createRootElement('section', 'cart');
    cartEl.innerHTML = `
      <h2>Total: \$${0}</h2>
      <button>Order Now!</button>
    `;
    this.totalOutput = cartEl.querySelector('h2');
  }
}

class ProductItem extends Component {
  constructor(product, renderHookId) {
    super(renderHookId); 
    this.product = product;
  }

  addToCart() {
    App.addProductToCart(this.product);
  }

  render() {
    const prodEl = this.createRootElement('li', 'product-item');
    prodEl.innerHTML = `
        <div>
          <img src="${this.product.imageUrl}" alt="${this.product.title}" >
          <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>\$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
    const addCartButton = prodEl.querySelector('button');
    addCartButton.addEventListener('click', this.addToCart.bind(this));
  }
}

class ProductList extends Component {
  products = [
    new Product(
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ];

  constructor(renderHookId) {
    super(renderHookId);
  }

  render() {
    this.createRootElement('ul', 'product-list', [
      new ElementAttribute('id', 'prod-list') 
    ]);
    for (const prod of this.products) {
      new ProductItem(prod, 'prod-list'); 
      // const productItem = new ProductItem(prod, 'prod-list'); 
      // productItem.render();
    }
  }
}



class Shop extends Component {

  constructor() {
    super(); // call parent constructor - render hook ID is not required for the shop
    // we definitely need to call super to make sure that the render method is getting triggered.
  }

  render() {
    this.cart = new ShoppingCart('app'); 
    // this.cart.render();
    // const productList = new ProductList('app'); 
    // productList.render();
    new ProductList('app'); 
  }
}


// an alternative here of course would have been to also just call this render like this because we need

// nothing else from the base class, so you might argue that extending it might not make that much sense,

// class Shop {
//   constructor() {
//     this.render();  // here....
//   }
//   render() {
//     this.cart = new ShoppingCart('app'); 
//     new ProductList('app'); 
//   }
// }


class App {
  static cart;

  static init() {
    const shop = new Shop();
    // shop.render();
    this.cart = shop.cart;
  }

  static addProductToCart(product) {
    this.cart.addProduct(product);
  }
}

App.init();
```

### super() Constructor Execution, Order & "this"

* Inside of the parent constructor or inside of methods triggered by the parent constructor, you can't access any of these fields but as I just explained, you also wouldn't be able to access any of the other properties of this subclass here because you can only add properties, you can only use the this keyword in your subclass constructor after the parent class constructor finished execution and that includes any methods that might have been invoked by that parent class constructor.

```js
class ProductList extends Component {
  products = [
    new Product(
      'A Pillow',
      'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
      'A soft pillow!',
      19.99
    ),
    new Product(
      'A Carpet',
      'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
      'A carpet which you might like - or not.',
      89.99
    )
  ];

  constructor(renderHookId) {
    super(renderHookId); // here render called before the data.. created
  }

  render() {
    this.createRootElement('ul', 'product-list', [
      new ElementAttribute('id', 'prod-list') 
    ]);
    for (const prod of this.products) {
      new ProductItem(prod, 'prod-list'); 
    }
  }
}
```
* So that's the problem we're facing here with this render method being called by the parent class constructor. In a constructor, we already go ahead and we execute render and we rely on data which hasn't been created yet.

* So how can we solve that issue then? Well this is a quite common use case, you might wanna do something with some data which might just not be there yet when you get started and indeed, the products data would probably not be hardcoded into our code here but we might be fetching it from a database, so indeed it might really not be loaded when we try to render everything here.

```js

class Component {
  constructor(renderHookId, shouldRender = true) {
    this.hookId = renderHookId;
    //we check if should render is true and if it is, we call render but now we can override this when we call the parent class constructor
    if (shouldRender) {
      this.render();
    }
  }

  render() {}

  createRootElement(tag, cssClasses, attributes) {
    const rootElement = document.createElement(tag);
    if (cssClasses) {
      rootElement.className = cssClasses;
    }
    if (attributes && attributes.length > 0) {
      for (const attr of attributes) {
        rootElement.setAttribute(attr.name, attr.value);
      }
    }
    document.getElementById(this.hookId).append(rootElement);
    return rootElement;
  }
}

class ProductItem extends Component {
  constructor(product, renderHookId) {
    super(renderHookId, false); // so with this we are blocking parent class constructor to call render
    this.product = product; //  we instead call render manually after we know that the product was set.
    this.render(); // here we call render manually
  }

  addToCart() {
    App.addProductToCart(this.product);
  }

  render() {
    const prodEl = this.createRootElement('li', 'product-item');
    prodEl.innerHTML = `
        <div>
          <img src="${this.product.imageUrl}" alt="${this.product.title}" >
          <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>\$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
    const addCartButton = prodEl.querySelector('button');
    addCartButton.addEventListener('click', this.addToCart.bind(this));
  }
}


class ProductList extends Component {
  products = [];

  constructor(renderHookId) {
    super(renderHookId);
    this.fetchProducts();
  }

  fetchProducts() {
    this.products = [
      new Product(
        'A Pillow',
        'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
        'A soft pillow!',
        19.99
      ),
      new Product(
        'A Carpet',
        'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
        'A carpet which you might like - or not.',
        89.99
      )
    ];
    this.renderProducts(); // but I also want to call render products here in fetch products, so also call it here once I got my products.
  }

  renderProducts() {
    for (const prod of this.products) {
      new ProductItem(prod, 'prod-list');
    }
  }

  render() {
    this.createRootElement('ul', 'product-list', [
      new ElementAttribute('id', 'prod-list')
    ]);
    if (this.products && this.products.length > 0) {
      this.renderProducts(); // call renderProducts only after data has been loaded
    }
  }
}
```
### Different Ways of Adding Methods

```js
class ShoppingCart extends Component {
  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: \$${this.totalAmount.toFixed(
      2
    )}</h2>`;
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, curItem) => prevValue + curItem.price,
      0
    );
    return sum;
  }

  constructor(renderHookId) {
    
    super(renderHookId, false);
    // That's what I explained earlier, the fields are translated to properties after the parent constructor called via super() ran. 
    // set up this, not as a field but actually as a property with this order products equal which kind of replaces the field, 
    // just so it just executes such that we can thereafter call render manually.
    this.orderProducts = () => { // either you should use arrow function here or in our trigger event listener
      console.log('Ordering...');
      console.log(this.items); // we also ensure that "this" inside of this function will always refer to the object created by the class
      // and not to what it would normally refer to and in this case, that would be the button of course.
    };

    this.render(); // call render manually.
    // So again, this is just a workaround that's required for this exact use case.
  }

  // Now actually technically there is a difference between storing a function in a property like this.orderProducts and adding a method addProduct that we will see in detail
  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems;
  }

  render() {
    const cartEl = this.createRootElement('section', 'cart');
    cartEl.innerHTML = `
      <h2>Total: \$${0}</h2>
      <button>Order Now!</button>
    `;
    const orderButton = cartEl.querySelector('button');
    // orderButton.addEventListener('click', () => this.orderProducts());
    orderButton.addEventListener('click', this.orderProducts);
    this.totalOutput = cartEl.querySelector('h2');
  }
}
```
### Private Properties

* Refer : private-fields-properties.pdf

* There is one additional, very new feature which I don't want to hide from you though. It's extremely new, browser support is pretty slim right now but will get better and since this is a future oriented course, you'll learn it right now already so that we can use it in the future and in Chrome thankfully, already today

* and that would be private fields, properties and methods, so that private term might not tell you much. Until now though I can tell you we only worked with public methods and public properties and fields but we can actually also work with private properties and fields and methods.

* Now the idea behind public properties and methods is that we can access them from outside of the class and object and that's what we did in some cases already.

* You typically want to make everything public which needs to be accessed from outside, so the things you worked with in your other code. An example would be if you have a product and there you have a buy method, which we don't have in our example but if you have another app and that should probably be triggered from outside the product object because the place where you work with your product object is probably the place where a user can buy it.

* You also on the other hand have some logic, some methods or some properties which you need inside of an object but not outside of it, so the things which you need to make your object work and which you don't really need to trigger or work with from outside, some hardcoded fallback values or some class specific logic.

* product should only be available from inside product list and we can achieve this in Javascript by adding a hash symbol in front of that.

```js
class Product {
  // title = 'DEFAULT';
  // imageUrl;
  // description;
  // price;

  constructor(title, image, desc, price) {
    this.title = title;
    this.imageUrl = image;
    this.description = desc;
    this.price = price;
  }
}

class ElementAttribute {
  constructor(attrName, attrValue) {
    this.name = attrName;
    this.value = attrValue;
  }
}

class Component {
  constructor(renderHookId, shouldRender = true) {
    this.hookId = renderHookId;
    if (shouldRender) {
      this.render();
    }
  }

  render() {}

  createRootElement(tag, cssClasses, attributes) {
    const rootElement = document.createElement(tag);
    if (cssClasses) {
      rootElement.className = cssClasses;
    }
    if (attributes && attributes.length > 0) {
      for (const attr of attributes) {
        rootElement.setAttribute(attr.name, attr.value);
      }
    }
    document.getElementById(this.hookId).append(rootElement);
    return rootElement;
  }
}

class ShoppingCart extends Component {
  items = [];

  set cartItems(value) {
    this.items = value;
    this.totalOutput.innerHTML = `<h2>Total: \$${this.totalAmount.toFixed(
      2
    )}</h2>`;
  }

  get totalAmount() {
    const sum = this.items.reduce(
      (prevValue, curItem) => prevValue + curItem.price,
      0
    );
    return sum;
  }

  constructor(renderHookId) {
    super(renderHookId, false);
    this.orderProducts = () => {
      console.log('Ordering...');
      console.log(this.items);
    };
    this.render();
  }

  addProduct(product) {
    const updatedItems = [...this.items];
    updatedItems.push(product);
    this.cartItems = updatedItems;
  }

  render() {
    const cartEl = this.createRootElement('section', 'cart');
    cartEl.innerHTML = `
      <h2>Total: \$${0}</h2>
      <button>Order Now!</button>
    `;
    const orderButton = cartEl.querySelector('button');
    // orderButton.addEventListener('click', () => this.orderProducts());
    orderButton.addEventListener('click', this.orderProducts);
    this.totalOutput = cartEl.querySelector('h2');
  }
}

class ProductItem extends Component {
  constructor(product, renderHookId) {
    super(renderHookId, false);
    this.product = product;
    this.render();
  }

  addToCart() {
    App.addProductToCart(this.product);
  }

  render() {
    const prodEl = this.createRootElement('li', 'product-item');
    prodEl.innerHTML = `
        <div>
          <img src="${this.product.imageUrl}" alt="${this.product.title}" >
          <div class="product-item__content">
            <h2>${this.product.title}</h2>
            <h3>\$${this.product.price}</h3>
            <p>${this.product.description}</p>
            <button>Add to Cart</button>
          </div>
        </div>
      `;
    const addCartButton = prodEl.querySelector('button');
    addCartButton.addEventListener('click', this.addToCart.bind(this));
  }
}

class ProductList extends Component {
  #products = []; // # means private product

  constructor(renderHookId) {
    super(renderHookId, false); // disable default 
    this.render(); // manual render
    this.fetchProducts(); //this.#fetchProducts(); even you can create private methods
  }

  fetchProducts() { // #fetchProducts() {
    this.#products = [ // # means private product
      new Product(
        'A Pillow',
        'https://www.maxpixel.net/static/photo/2x/Soft-Pillow-Green-Decoration-Deco-Snuggle-1241878.jpg',
        'A soft pillow!',
        19.99
      ),
      new Product(
        'A Carpet',
        'https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Ardabil_Carpet.jpg/397px-Ardabil_Carpet.jpg',
        'A carpet which you might like - or not.',
        89.99
      )
    ];
    this.renderProducts();
  }

  renderProducts() {
    for (const prod of this.#products) { // # means private product
      new ProductItem(prod, 'prod-list');
    }
  }

  render() {
    this.createRootElement('ul', 'product-list', [
      new ElementAttribute('id', 'prod-list')
    ]);
    if (this.#products && this.#products.length > 0) { // # means private product
      this.renderProducts();
    }
  }
}

class Shop {
  constructor() {
    this.render();
  }

  render() {
    this.cart = new ShoppingCart('app');
    new ProductList('app');
    // const list = new ProductList('app');
    // console.log(list.#products); // if we try to access like this we will get an error "Private field must be declared in enclosing class"
  }
}

class App {
  static cart;

  static init() {
    const shop = new Shop();
    this.cart = shop.cart;
  }

  static addProductToCart(product) {
    this.cart.addProduct(product);
  }
}

App.init();

```

### "Pseudo-Private" Properties

* The addition of private fields and properties is relatively new - in the past, such a feature was not part of JavaScript. Hence you might find many scripts that use a concept which you could describe as "pseudo-private" properties.

```js
class User {
    constructor() {
        this._role = 'admin';
    }
}
 
// or directly in an object
 
const product = {
    _internalId: 'abc1'
};

```
* It's a quite common convention to prefix private properties with an underscore (_) to signal that they should not be accessed from outside of the object.

* Important: It's just a convention that should signal something! It does NOT technically prevent access. You CAN run this code without errors for example:

```js
const product = {
    _internalId: 'abc1'
};
console.log(product._internalId); // works!
```
* It's really just a hint that developers should respect. It's not as strict as the "real" private properties introduced recently (#propertyName).

### The "instanceof" Operator

```js
class Person{
  name : "max";
}

const p = new Person();
p --->  // it's this type Person which is interesting.

type p // "object"
```

* I haven't talked about this before because you need to understand classes, to understand the idea of types here. It's not an official type, if we do type of p, we'll see this is type object, this is not type person but still somehow Javascript keeps in mind that this was created based on that person class, it's not of that type but somewhere this has to be stored otherwise the developer tools couldn't show this to us and indeed, that is stored somewhere.

* There is a special operator, instanceof and you can use it to check if an object is created based on a certain class or a certain blueprint.

```js
p instanceof Person // true
```
* this will return true if p was created based on person or if the value stored in p was created based on person and it will return false otherwise and actually in Javascript there are a bunch of built-in classes if you want to call them like this,

### Built-in Classes

```js
const obj = new object(); // same as const obj2 = {};
const Array = new Array(); // []
```
### Understanding Object Descriptors

```js
const person = {name : 'Max', greet(){
  console.log(this.name)
}};

person.greet()// Max
```
* Now actually every property you add and every method you add as well, it's basically a property which holds a function has a so-called descriptor.

```js
Object.getOwnPropertyDescriptor(person) // what you get back is a new object with the so-called property descriptors.
```

* Now that's some metadata stored behind the scenes by Javascript, it influences how the properties can be used, so that some configuration Javascript sets up for you and stores for you which you can change though.

* So let's have a look at the name property, here it is, we see it has a value of Max, that's what we assigned but we see three other configuration items as well - configurable, enumerable and writable.

* Now this simply means that this property, the name property on the person object holds a value of Max that we can assign a new value so that it's writable, that it's configurable which means we can for example delete it and it's enumerable which means it appears in a for/in loop 

```js
Object.defineProperty(person, "name", {
  value:'Max', // value: person.name
  configurable: true,
  enumerable: true,
  writable: false // here writable false
})
```
* what you'll notice is that if I try to access person.name and set this to Maximilian, person still has Max in there, it didn't throw an error but it didn't accept the change.