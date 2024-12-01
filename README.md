# SearchController Implementation - README

## Overview

This document provides an in-depth explanation of the `SearchController` implementation in this project. The `SearchController` is a RESTful API endpoint designed to handle product search queries, providing a dynamic and user-friendly search functionality for the application. This feature is implemented using **Spring Boot** and adheres to **Test-Driven Development (TDD)** principles, ensuring robust and thoroughly validated behavior.

---

## Responsibilities

The `SearchController` is responsible for the following:

1. **Expose a Search API**  
   The controller listens for HTTP `GET` requests at the `/api/products/search` endpoint. It processes a query parameter to filter and return relevant product items.

2. **Integrate with the Product Database**  
   It utilizes the `ProductItemRepository` to retrieve product data from the database and applies additional filtering to refine results based on user input.

3. **Support Flexible Search Behavior**  
   The search functionality allows for both:
   - **Exact matches**: Identifies product names or descriptions that exactly match the query when enclosed in double quotes (`"example"`).
   - **Partial and case-insensitive matches**: Identifies product names or descriptions containing the query, regardless of case.

4. **Comply with TDD Specifications**  
   The implementation strictly follows predefined test cases to meet expected behaviors. These tests validate scenarios such as exact matches, partial matches, and case insensitivity.

---

## Implementation Details

### Core Components

1. **Endpoint Definition**  
   The search API is defined using the `@GetMapping` annotation:
   ```java
   @GetMapping("/api/products/search")
   public Collection<ProductItem> search(@RequestParam("query") String query)
   ```
   This maps HTTP GET requests to the `search` method, with the query passed as a parameter.

2. **Query Processing**  
   The method distinguishes between two modes of search:
   - **Exact Match:**  
     If the query is wrapped in double quotes, the quotes are stripped, and the method checks for an exact match in either the `name` or `description` fields of a `ProductItem`.
   - **Partial Match (Case-Insensitive):**  
     The query and the product fields are normalized to lowercase, and the method checks if the query is contained within the `name` or `description`.

3. **Data Filtering**  
   The method iterates through all products retrieved from the database and applies the filtering logic:
   ```java
   for (ProductItem item : allItems) {
       boolean matches = isExactMatch(query, item) || isPartialMatch(query, item);
       if (matches) {
           itemList.add(item);
       }
   }
   ```

4. **Integration with `ProductItemRepository`**  
   The controller relies on the `ProductItemRepository` to fetch all product items from the database:
   ```java
   Iterable<ProductItem> allItems = this.productItemRepository.findAll();
   ```

---

### Key Features

- **Exact Match Mode:**  
  Matches products whose `name` or `description` matches the query exactly when wrapped in double quotes (`"example"`).

- **Case-Insensitive Matching:**  
  Searches are not affected by the case of the characters, ensuring a more user-friendly experience.

- **Dynamic Filtering:**  
  Efficient filtering logic ensures results include relevant products while maintaining performance.

---

## Test-Driven Development Approach

The implementation of the `SearchController` adheres to a **TDD workflow**, ensuring high-quality, bug-free functionality. Test cases were written in advance using the **Spock Framework**, a powerful and expressive testing tool. 

### Key Test Cases
- Return items matching the **name**.
- Return items matching the **description**.
- Handle **case-insensitive queries**.
- Support **exact match queries** using quotes.
- Handle **multiple matches** in both name and description.

These tests are defined in the `SearchControllerSpec` file and validate the following scenarios:
- Items are returned only when their `name` or `description` satisfies the query.
- Queries enclosed in double quotes trigger exact-match behavior.
- Partial matches work as expected, regardless of case.

---

## Conclusion

The `SearchController` is a critical part of this project, offering a flexible, user-centric search API. By leveraging Spring Boot, `ProductItemRepository`, and TDD principles, it ensures reliable and maintainable functionality. Its robust design adheres to best practices in API development and seamlessly integrates with the application's broader architecture.

This implementation ensures that users can effectively locate products based on intuitive search behavior, enhancing their overall experience with the application.
