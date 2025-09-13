---
layout: default
title: "Getting Started with Spring Testing"
description: "Step-by-step tutorial for writing your first Spring REST controller unit test"
---

# Getting Started with Spring REST Controller Testing

This page provides a practical, step-by-step guide to writing your first unit test for a Spring REST controller.

## Prerequisites

Before you begin, make sure you have:

- Java 8 or higher
- Spring Boot 2.x or 3.x
- JUnit 5
- Mockito
- Maven or Gradle

## Step 1: Create a Simple Controller

Let's start with a basic REST controller:

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    private final UserService userService;
    
    public UserController(UserService userService) {
        this.userService = userService;
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        if (user != null) {
            return ResponseEntity.ok(user);
        }
        return ResponseEntity.notFound().build();
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody @Valid User user) {
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
}
```

## Step 2: Write Your First Unit Test

Create a test class for the controller:

```java
@ExtendWith(MockitoExtension.class)
class UserControllerTest {
    
    @Mock
    private UserService userService;
    
    @InjectMocks
    private UserController userController;
    
    @Test
    void getUser_WhenUserExists_ShouldReturnUser() {
        // Arrange
        Long userId = 1L;
        User expectedUser = new User(userId, "John Doe", "john@example.com");
        when(userService.findById(userId)).thenReturn(expectedUser);
        
        // Act
        ResponseEntity<User> response = userController.getUser(userId);
        
        // Assert
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).isEqualTo(expectedUser);
        verify(userService).findById(userId);
    }
    
    @Test
    void getUser_WhenUserDoesNotExist_ShouldReturnNotFound() {
        // Arrange
        Long userId = 999L;
        when(userService.findById(userId)).thenReturn(null);
        
        // Act
        ResponseEntity<User> response = userController.getUser(userId);
        
        // Assert
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.NOT_FOUND);
        assertThat(response.getBody()).isNull();
        verify(userService).findById(userId);
    }
    
    @Test
    void createUser_WithValidUser_ShouldReturnCreatedUser() {
        // Arrange
        User userToCreate = new User(null, "Jane Doe", "jane@example.com");
        User savedUser = new User(1L, "Jane Doe", "jane@example.com");
        when(userService.save(userToCreate)).thenReturn(savedUser);
        
        // Act
        ResponseEntity<User> response = userController.createUser(userToCreate);
        
        // Assert
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        assertThat(response.getBody()).isEqualTo(savedUser);
        verify(userService).save(userToCreate);
    }
}
```

## Step 3: Test Different Scenarios

### Testing Validation Errors

```java
@Test
void createUser_WithInvalidEmail_ShouldReturnBadRequest() {
    // Arrange
    User invalidUser = new User(null, "John", "invalid-email");
    
    // Act & Assert
    assertThrows(MethodArgumentNotValidException.class, 
        () -> userController.createUser(invalidUser));
}
```

### Testing Exception Handling

```java
@Test
void getUser_WhenServiceThrowsException_ShouldHandleGracefully() {
    // Arrange
    Long userId = 1L;
    when(userService.findById(userId))
        .thenThrow(new RuntimeException("Database error"));
    
    // Act & Assert
    assertThrows(RuntimeException.class, 
        () -> userController.getUser(userId));
}
```

## Step 4: Best Practices Summary

1. **Use descriptive test names** that explain what is being tested
2. **Follow the Arrange-Act-Assert pattern**
3. **Mock all dependencies** to isolate the controller
4. **Test both success and failure scenarios**
5. **Verify interactions** with mocked dependencies
6. **Keep tests simple and focused**

## Next Steps

- Learn about [testing complex scenarios](index.md#advanced-scenarios)
- Explore [testing patterns](index.md#testing-patterns)
- Check out the [complete example repository](https://github.com/jeck7/spring-testing-examples)

---

[← Back to Home](index.md) | [About the Author](about.md)