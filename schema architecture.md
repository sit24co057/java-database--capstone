# Smart Clinic Management System – Architecture Design

## 1. Architecture Summary

The Smart Clinic Management System follows a three-tier architecture consisting of the Presentation Tier, Application Tier, and Data Tier. The Presentation Tier uses Thymeleaf templates for Admin and Doctor dashboards and REST APIs for modules such as appointments and patient records. The Application Tier uses Spring Boot, MVC controllers, REST controllers, and services to handle requests and business logic. The Data Tier uses MySQL with Spring Data JPA to store structured data such as patients, doctors, appointments, and administrators. MongoDB with Spring Data MongoDB stores flexible document-based data such as prescriptions. The service layer connects controllers to repositories, ensuring organized data access and maintainable application development.

## 2. Numbered Flow

1. **User Interface:** Admins and doctors access their dashboards through Thymeleaf pages, while other clients use REST API endpoints.

2. **Controller Layer:** Incoming requests are routed to the appropriate MVC controller or REST controller.

3. **Service Layer:** The controller calls the service layer to apply business rules, validate input, and process requests.

4. **Repository Layer:** The service layer interacts with the appropriate repository to perform database operations.

5. **Database Access:** Spring Data JPA accesses MySQL for structured data, while Spring Data MongoDB accesses MongoDB for prescription documents.

6. **Model Binding:** Database records are mapped to Java JPA entities or MongoDB document models.

7. **Response Generation:** MVC controllers pass model data to Thymeleaf templates to render HTML pages. REST controllers return responses, commonly in JSON format, to API clients.
