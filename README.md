# Stackline Full Stack Assignment

## Overview

This is a sample eCommerce website that includes:
- Product List Page
- Search Results Page
- Product Detail Page

The application contains various bugs including UX issues, design problems, functionality bugs, and potential security vulnerabilities.

## Getting Started

```bash
yarn install
yarn dev
```

## Your Task

1. **Identify and fix bugs** - Review the application thoroughly and fix any issues you find
2. **Document your work** - Create a comprehensive README that includes:
   - What bugs/issues you identified
   - How you fixed each issue
   - Why you chose your approach
   - Any improvements or enhancements you made

We recommend spending no more than 2 hours on this assignment. We are more interested in the quality of your work and your communication than the amount of time you spend or how many bugs you fix!

## Submission

- Fork this repository
- Make your fixes and improvements
- **Replace this README** with your own that clearly documents all changes and your reasoning
- Provide your Stackline contact with a link to a git repository where you have committed your changes

We're looking for clear communication about your problem-solving process as much as the technical fixes themselves.

## Fixes

1. The /api/products call was returning a lot of properties that were unused by the overview / search page. I adjusted it to not include most of the unused info. That info is provided in the API call when selecting an individual product.

`http://localhost:3000/api/products`

```json
{
   "products":[
      {
         "stacklineSku":"E8ZVY2BP3",
         "subCategoryId":53238,
         "title":"Amazon Kindle Paperwhite (16 GB) – Now with a larger display, adjustable warm light, increased battery life, and faster page turns – Black",
         "categoryName":"Tablets",
         "retailPrice":149.99,
         "retailerSku":"B09TMN58KL",
         "categoryId":1702,
         "subCategoryName":"E-Readers",
         "thumbnail":"https://m.media-amazon.com/images/I/71FWKtSIYUL._AC_SL100_.jpg"
      },
      ...
```

You can see the imageUrls and featureBullets are gone and a thumbnail property is added instead.

2. Changed the way parameters are passed when selecting a product. Instead of the client passing back all the product info to the server only product sku is now passed.

For example `http://localhost:3000/product?sku=E8ZVY2BP3`

```json
{
   "stacklineSku":"E8ZVY2BP3",
   "featureBullets":[
      "Kindle Paperwhite – Now with a 6.8” display and thinner borders, adjustable warm light, up to 10 weeks of battery life, and 20% faster page turns.",
      "Purpose-built for reading – With a flush-front design and 300 ppi glare-free display that reads like real paper, even in bright sunlight.",
      "More books in more places – Store thousands of titles, then take them all with you. A single charge via USB-C last weeks, not hours.",
      "Easy on the eyes – Now with adjustable warm light to shift screen shade from white to amber.",
      "Waterproof reading – Built to withstand accidental immersion in water, so you’re good from the beach to the bath.",
      "Find new stories – With Kindle Unlimited, get unlimited access to over 2 million titles, thousands of audiobooks, and more.",
      "Go hands-free – Pair with an Audible subscription and Bluetooth headphones or speakers to listen to your story."
   ],
   "imageUrls":[
      "https://m.media-amazon.com/images/I/71FWKtSIYUL._AC_SL1500_.jpg",
      "https://m.media-amazon.com/images/I/51QCk82iGcL._AC_SL1000_.jpg",
      "https://m.media-amazon.com/images/I/71d6+Ib9muL._AC_SL1000_.jpg",
      "https://m.media-amazon.com/images/I/61FPneVqEpL._AC_SL1000_.jpg",
      "https://m.media-amazon.com/images/I/61wpBGaXTLL._AC_SL1000_.jpg",
      "https://m.media-amazon.com/images/I/51627vI8XvL._AC_SL1000_.jpg"
   ],
   "subCategoryId":53238,
   "title":"Amazon Kindle Paperwhite (16 GB) – Now with a larger display, adjustable warm light, increased battery life, and faster page turns – Black",
   "categoryName":"Tablets",
   "retailPrice":149.99,
   "retailerSku":"B09TMN58KL",
   "categoryId":1702,
   "subCategoryName":"E-Readers"
}
```

This was changed since we shouldn't be passing all this untrusted input from the client and it is also inefficient.

1. We can use Amazon's image API to get a resized version for many of the products from their server for the product thumbnail. So we only fetch the larger images when that product is selected.

1. The category selector wasn't encoding the category name correctly. This cause some categories not to display the subcategory selector. Added URI encoding to handle this.

1. The clear filters wasn't resetting the category. Fixed the set methods so everything is cleared.

1. The subcategories weren't being set correctly since the category parameter wasn't passed to the API.

1. When typing in search terms there was no delay between keystrokes. Added a 500ms debouncer so the user can finish typing before searching.

1. When searching for some terms like "apple" the images were not configured to be allowed from that Amazon subdomain. I updated next.config.ts to stop crashing on that.

1. The version of Next used in the product has security vulnerabilities, it should be updated.

## Design proposal

1. Two things jumped out to me with the initial implementation. If more than 20 items match a category there is no easy way for a user to discover the rest of the items. An infinite scroll could be added to address this.

1. There is no way to multi select categories or subcategories. A nested tree design could address this.

1. If this was a production app the APIs would have to be secured and hardened. For example limiting how many products could be queried at once, throttling, etc.
