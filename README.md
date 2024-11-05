# Spring Boot Project using MySql

## Project Overview

To install the project, follow these steps:

1. Clone the repository to your local machine.
2. Run the project in IDE like Intellij or Eclipse

## Step by Step Process

- At First innitialize the Go to this link https://start.spring.io/ and select Spring `Starter Web`, `Spring security`, `MySQL Driver`, `Spring Session` and `Thymeleaf` all dependencies and download the zip file.

\*\* Set up the db connection using username, password and url `\src\main\resources\application.properties` in that location.

\*\* Then setting up the configuration from `\src\main\java\net\enjoy\springboot\registrationlogin\config\SpringSecurity.java` This SpringSecurity class is a configuration class for setting up security in a Spring Boot application.
This `SpringSecurity` configuration class sets up basic authentication and authorization for a Spring Boot app:

1.  **Password Encoding**: Uses BCrypt for hashing passwords (`passwordEncoder` bean).
2.  **Authorization Rules** (`filterChain` method):

    - Allows unrestricted access to `/register`, `/index`, `/forgot-password`, and home (`/`).
    - Restricts `/users` to users with `ADMIN` role.

3.  **Login Settings**:

    - Custom login page at `/login`.
    - On successful login, redirects to `/users`.

4.  **Logout Settings**:

    - Logout can be accessed at `/logout`.

5.  **Global Authentication** (`configureGlobal` method):

    - Loads users via a custom `UserDetailsService` and uses BCrypt for password verification.

- For `src\main\java\net\enjoy\springboot\registrationlogin\controller\AuthController.java`, The `AuthController` class in a Spring Boot application manages user-related actions:

1.  **Home Page**: `@GetMapping("/index")` renders the home page.
2.  **User Registration**:

    - `@GetMapping("/register")` shows the registration form.
    - `@PostMapping("/register/save")` processes form submissions, checks for duplicate emails, validates input, and saves the user.

3.  **User List**: `@GetMapping("/users")` retrieves and displays all registered users.
4.  **Forgot Password**: `@GetMapping("/forgot-password")` shows the password recovery page.
5.  **Login**: `@GetMapping("/login")` renders the login page.

- For `src\main\java\net\enjoy\springboot\registrationlogin\dto\UserDto.java`, The `UserDto` class is a Data Transfer Object (DTO) used in a Spring Boot application for transferring user data between layers. It leverages Lombok annotations to simplify code by automatically generating getters, setters, constructors, and more.

### Fields

\*\* **id**: Holds the user ID.
\*\* **firstName** and **lastName**: Represent the user's first and last names. They are validated to ensure they’re not empty.
\*\* **email**: Stores the user’s email address, with validation to ensure it's in a valid email format and is not empty.
\*\* **password**: Contains the user's password and is validated to be non-empty.

### Annotations

** `@Getter`, `@Setter`: Auto-generate getters and setters for each field.
** `@NoArgsConstructor`, `@AllArgsConstructor`: Provide a no-args constructor and an all-args constructor for ease of use.
** `@NotEmpty`: Ensures certain fields (e.g., `firstName`, `lastName`, `email`, and `password`) are not left blank.
** `@Email`: Validates that `email` has a correct email format.

- For `src\main\java\net\enjoy\springboot\registrationlogin\entity\User.java`, The `User` class represents a user entity in a Spring Boot application and is mapped to the `users` database table.

### Fields

\*\* **id**: Primary key, auto-generated.
\*\* **name**: Stores the user's name (cannot be null).
\*\* **email**: Stores a unique email for each user (cannot be null).
\*\* **password**: Stores the user's password (cannot be null).

### Relationships

\*\* **roles**: Many-to-Many relationship with the `Role` entity, enabling users to have multiple roles. This is mapped to the `users_roles` join table.

### Annotations

** `@Entity` and `@Table`: Define the class as a JPA entity associated with the `users` table.
** `@ManyToMany`: Specifies the many-to-many relationship with the `Role` class.

- For `src\main\java\net\enjoy\springboot\registrationlogin\security\CustomUserDetailsService.java`, The `CustomUserDetailsService` class is a custom implementation of `UserDetailsService` for integrating Spring Security with the application's user data.

### Purpose

This service loads user-specific data from the database for authentication. It uses the user's email as the identifier and returns user details for Spring Security.

### Key Methods

\*\* **loadUserByUsername(String email)**: Looks up a user by email using `UserRepository`. If found, it returns a `UserDetails` object with the user's email, password, and roles. If not, it throws a `UsernameNotFoundException`.
\*\* **mapRolesToAuthorities(Collection<Role> roles)**: Maps the user's roles to Spring Security authorities, converting each `Role` entity to a `SimpleGrantedAuthority`. This enables role-based access control in the application.

### Annotations

\*\* **@Service**: Marks this class as a service component in Spring, allowing it to be autowired where needed.

- For `src\main\java\net\enjoy\springboot\registrationlogin\RegistrationLoginApplication.java`, The `RegistrationLoginApplication` class is the main entry point for the Spring Boot application.

### Key Components

\*\* **@SpringBootApplication**: This annotation marks the class as a Spring Boot application, enabling auto-configuration, component scanning, and configuration support.
\*\* **main Method**: Calls `SpringApplication.run(...)`, which starts the entire Spring Boot application.

## Usage (base_url: http://localhost:8080)

The following API endpoints are available: **All pages are available in templete folder**

| URLS               | Description                  |
| ------------------ | ---------------------------- |
| `/forgot-password` | View form to forgot password |
| `/`                | View the index page          |
| `/login`           | Show login form              |
| `/register`        | show register form           |
| `/users`           | show all users               |
| `/logout`          | logout from admin            |
