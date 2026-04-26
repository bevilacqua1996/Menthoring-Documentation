# REST Concepts

### What is REST?
> REST = Representational State Transfer  
> A resource-based architecture.  
> Created by Roy Fielding in 2000.
**Main characteristics:**
*   Stateless
*   Cacheable
*   Layered system
*   Uniform interface
* * *
### Semantic URIs
❌ Bad:
 *   `/getCustomer?id=5`
 *   `/createNewCustomer`
✅ Good:
*   `/api/v1/customers/5`
*   `/api/v1/customers`
**Rules:**
*   Use plural nouns
*   No verbs in the URL
*   Clear hierarchy
* * *
### HTTP Verbs

| Verb | Action | When to use | Status |
| ---| ---| ---| --- |
| GET | Fetch resource | Listing / search | `200` |
| POST | Create resource | Registration | `201` |
| PUT | Update (full) | Replacement | `200` or `204` |
| PATCH | Update (partial) | Targeted change | `200` |
| DELETE | Remove | Deletion | `204` |

* * *
### Versioning
> Prefix the API with `/api/v1`  
> Example:

```sql
GET /api/v1/products
```

* * *
### Status Codes

| Code | Meaning | When to use |
| ---| ---| --- |
| 200 | OK | Successful requests |
| 201 | Created | POST with resource creation |
| 204 | No Content | DELETE/PUT without a response body |
| 400 | Bad Request | Invalid data |
| 404 | Not Found | Resource does not exist |
| 500 | Internal Error | Unexpected failure |

* * *
### Clean Architecture with Spring Boot

```plain
Controller → Service → Repository
```

*   `Controller`: handles HTTP
*   `Service`: business logic
*   `Repository`: data access (JPA, etc.)
> Tip: Do not put business logic in the Controller.
* * *
### Extras (for curiosity or future sessions)
*   Use of DTOs
*   Documentation with Swagger
*   Unified error pattern
*   Logging best practices
*   Tests with Spring Test, Postman, or RestAssured
