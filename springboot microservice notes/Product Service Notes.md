## Docker compose
---
- so each service will be containerized
- this just get all dependencies for all future services

``` yml
services:
  mongodb:

    image: mongo:7.0.5

    container_name: mongodb

    ports:

      - "27017:27017"

    environment:

      MONGO_INITDB_ROOT_USERNAME: root

      MONGO_INITDB_ROOT_PASSWORD: password

      MONGO_INTDB_DATABASE: product-service

    volumes:

      - mongodb_data:/data/db


volumes:

  mongodb_data:
```


Docker compose break down
-  **`services:`**
	- define container

- This section

## Product Class
--- 
**`package com.learning.microservices.product.model;`** this is a namespace in C#

**`@Document(value = "product")`**:

- Marks the class as a **MongoDB document** (a collection in MongoDB).
- The `value = "product"` specifies that this class will be mapped to the **"product"** collection in MongoDB.

**`@Data`**:

- From **Lombok**, this annotation automatically generates:
    - **Getters** and **Setters** for all fields.
    - `toString()`, `equals()`, and `hashCode()` methods.
    - **Constructor** that takes all fields (except `id`).

**`@AllArgsConstructor`**:
    
- Automatically generates a constructor with **all fields** as parameters (except `id` because it's generated by MongoDB).

- basically if I do not have it i have to manually make a constructor
``` java
public class Product {
    private String id;
    private String name;
    private String description;
    private String skuCode;
    private BigDecimal price;

    public Product(String id, String name, String description, String skuCode, BigDecimal price) {
        this.id = id;
        this.name = name;
        this.description = description;
        this.skuCode = skuCode;
        this.price = price;
    }
}


// create it like this
Product product = new Product("123", "Product A", "A great product", "SKU123", new BigDecimal("19.99"));

```

A **constructor** is a special method in a class that is used to **initialize** objects. It's called **automatically** when you create an object from that class.



**`@NoArgsConstructor`**:

- Automatically generates a **no-argument constructor**, which is required by MongoDB for object mapping.

Here’s the key reason:

- MongoDB **doesn’t know** what values to pass when it creates the object, so it uses the **no-argument constructor** to create an **empty object**.
- Once the object is created, MongoDB then **fills in the fields** using the data from the document it retrieved.

### **How Reflection Works**:

1. **Reflection** allows MongoDB to **dynamically** create an object of the `Product` class without needing to know beforehand what values to pass.
2. **The no-argument constructor** is required so MongoDB can instantiate (create) an object without passing any values, and then it can set the field values after the object is created.

- basically, mongodb needs to instantiate an object with the current fields but when instantiate do not fill it with anything cause like you don't know what you need, u need just like a create a box, then u fill in paremetes via **reflection**

`@Builder`: Provides a **builder pattern** to create objects easily (e.g., `Product.builder().name("Product").build()`).

``` java
Product product = Product.builder()
                        .name("Product A")
                        .description("A great product")
                        .skuCode("SKU123")
                        .price(new BigDecimal("19.99"))
                        .build();

// MongoDB will automatically generate the `id` when it's saved.

```

## Repo
---
``` java
package com.learning.microservices.product.model;

import com.learning.microservices.product.model.Product;

import org.springframework.data.mongodb.repository.MongoRepository;

public interface ProductRepository extends MongoRepository<Product, String> {

}
```

- **`import org.springframework.data.mongodb.repository.MongoRepository;`**:  
    This imports the **`MongoRepository`** interface from **Spring Data MongoDB**. **`MongoRepository`** provides built-in methods to interact with MongoDB, like **save, find, delete, etc.**, without having to write any SQL queries.

By extending **`MongoRepository<Product, String>`**, the **`ProductRepository`** automatically inherits several methods from the **`MongoRepository`** interface to interact with the MongoDB database, such as:

- **`save(Product product)`**: Saves a `Product` to the database.
- **`findById(String id)`**: Finds a `Product` by its `id`.
- **`delete(Product product)`**: Deletes a `Product` from the database.
- **`findAll()`**: Retrieves all `Product` documents from the database.

- basically free CRUD stuff

- You don’t need to provide the **implementation** for these methods yourself. Spring Data MongoDB will automatically generate the implementation at runtime for you.
- You just **define the interface** and Spring handles the database interaction for you.


#### **Separation of Concerns**:

- The **repository layer** separates **data access logic** from the rest of your application (like business logic in the service layer).
- This makes your code easier to manage, **test**, and **maintain** because each layer has a specific responsibility:
    - **Controller Layer**: Handles HTTP requests and responses.
    - **Service Layer**: Contains business logic.
    - **Repository Layer**: Handles interactions with the database.