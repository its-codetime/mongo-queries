# mongodb queries

## 1. Select the database to use

```
use products
```

## 2. get all products

```js
db.products.find();
```

## 3. get product using id

```js
db.products.find({ _id: "3" });
```

## 4. products priced in between 400 to 800

```js
db.products.find({ product_price: { $gt: 400, $lt: 800 } });
```

## 5. product priced outside 400 and 600

```js
db.products.find({ product_price: { $not: { $gt: 400, $lt: 800 } } });
```

## 6. products with price greater than 500

```js
db.products.find({ product_price: { $gt: 500 } });
```

## 7. name and material of all products

```js
db.products.find({}, { product_name: 1, product_material: 1 });
```

## 8. product with id 10

```js
db.products.find({ _id: "10" });
```

## 9. name and material of one product

```js
db.products.find({ _id: "10" }, { product_name: 1, product_material: 1 });
```

## 10. all products which contains 'soft' in product material

```js
db.products.find({ product_material: "Soft" });
```

## 11. Find products with color indigo and product price is 492

```js
db.products.find({ product_color: "indigo", product_price: 492 });
```

## 12. Delete products with same price value

```js
db.products
  .aggregate([
    // group products using price and count them
    { $group: { _id: "$product_price", count: { $count: {} } } },
    // match count > 1
    { $match: { count: { $gt: 1 } } },
  ])
  .forEach((doc) => {
    // remove all docs with the repeated price
    db.products.deleteMany({ product_price: doc._id });
  });
```
