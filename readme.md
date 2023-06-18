# Sweet Bakery Store

Modular web project provides an online store where customers can purchase sweet treats. The system is composed of a web front-end and a REST API back-end

<img src= "https://elements-preview-images-0.imgix.net/f63bfba5-c63d-49e1-8907-75ddbc8f5f63?auto=compress%2Cformat&fit=max&w=1370&s=c8f5e83974fd9a8766577c176eb1fb83" width="450" height="350" />

## *Description*
This Java application enables online ordering of bakery products. It is built using Spring MVC, Spring Security, internationalization (i18n), Java Mail Sender, Liquibase and Rest API. There are three types of users with their own abilities:

Users can:
- Register an account
- Change account details
- Add favorite products
- Write emails to administrators
- Do order
- Track order process
- Write reviews

Administrators and partners have the ability to:
- Change a user's role to partner
- Add and delete categories
- Control user's orders status
- Receive emails from users

Partners can:
- Add and delete products
- See user's purchased products
- See all order status

The application functionality includes user authentication and authorization system, account details change option, email notifications, product details such as product description, cost, images, and order system. Liquibase is used for database migration and Rest API for creating a modular program.

Various optimizations have been done to improve performance such as writing unit tests and optimizing code.

## *Modules*

- Web [link](https://github.com/GoharHakobyan95/sweet-bakery-store-web)
- RestApi [link](https://github.com/GoharHakobyan95/sweet-bakery-store-rest)
- Common [link](https://github.com/GoharHakobyan95/sweet-bakery-store-common)

## *Libraries*

Lombok, ModelMapper, MapStruct, JWT, Hibernate Validator, MySql Connector J, Spring Security, Spring Data Jpa, Spring Web, Spring Mail, Spring Thymeleaf, Spring Devtools, Spring Test, JUnit, Mockito Core, Liquibase Core, Commons IO . 

## *Few Code Examples*
### Endpoint to create an order

``` java
/**
* This endpoint is used to create an order for the given product with the given quantity. The endpoint requires two request parameters, id and quantity.
* The current user is authenticated using an authentication token provided earlier.
* The order data is obtained from createOrderDto.
* Prior to saving the order, it is verified that the product exists and the store has enough items to satisfy the order.
* If the order is successfully saved, an OrderResponseDto object is returned with status code CREATED (201).
*/
    @PostMapping
    public ResponseEntity<OrderResponseDto> createAnOrder(@AuthenticationPrincipal CurrentUser currentUser,
                                                          @Valid @RequestBody CreateOrderDto createOrderDto,
                                                          @RequestParam("id") Integer productId,
                                                          @RequestParam("quantity") Integer quantity) {
        log.info("User {} wants to create a new order", currentUser.getUser().getEmail());
        Optional<Product> optProduct = productService.findById(productId);
        if (optProduct.isEmpty() ||
                optProduct.get().getInStore() == 0 ||
                optProduct.get().getInStore() < quantity) {
            log.warn("Product doesn't exist or the count of product in order is more than count in store.");
            return ResponseEntity.status(HttpStatus.NOT_FOUND).build();
        }
        Product product = optProduct.get();
        log.info("Order has been created for user {}", currentUser.getUser().getEmail());
        return ResponseEntity.status(HttpStatus.CREATED)
                .body(orderMapper.map(orderService.saveOrder(currentUser.getUser(), createOrderDto, product, quantity)));
    }
``` 

## *Contributors*

Gohar Hakobyan, Ani Soghomonyan

### *Email:* hak.goh95@gmail.com 
