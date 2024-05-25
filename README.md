### Different components  
Data models, repositories, services, and controllers.


### Data Models

1. **User**
   - `id` (Long): Primary key
   - `name` (String): User's name
   - `email` (String): User's email (unique)
   - `bmi` (Double): User's BMI
   - `availableDays` (List<DayOfWeek>): Days user is available for workouts

2. **ExercisePlan**
   - `id` (Long): Primary key
   - `name` (String): Name of the exercise plan (e.g., Beginner, Intermediate, Advanced)
   - `description` (String): Description of the plan
   - `exercises` (List<Exercise>): List of exercises in the plan

3. **Exercise**
   - `id` (Long): Primary key
   - `name` (String): Name of the exercise
   - `description` (String): Description of the exercise
   - `duration` (Integer): Duration of the exercise in minutes
   - `sets` (Integer): Number of sets
   - `restTime` (Integer): Rest time between sets in seconds

4. **WorkoutSession**
   - `id` (Long): Primary key
   - `user` (User): Reference to the user
   - `exercisePlan` (ExercisePlan): Reference to the exercise plan
   - `date` (LocalDate): Date of the workout session

### APIs

1. **User API**
   - `POST /users`: Create a new user
   - `GET /users/{id}`: Retrieve user details
   - `PUT /users/{id}`: Update user details
   - `DELETE /users/{id}`: Delete a user

2. **ExercisePlan API**
   - `GET /exercise-plans`: List all exercise plans
   - `GET /exercise-plans/{id}`: Retrieve a specific exercise plan
   - `POST /exercise-plans`: Create a new exercise plan
   - `PUT /exercise-plans/{id}`: Update an exercise plan
   - `DELETE /exercise-plans/{id}`: Delete an exercise plan

3. **WorkoutSession API**
   - `POST /workout-sessions`: Create a new workout session
   - `GET /workout-sessions/{id}`: Retrieve a specific workout session
   - `GET /workout-sessions/user/{userId}`: List all workout sessions for a user
   - `DELETE /workout-sessions/{id}`: Delete a workout session

### Example Implementation

#### User Entity
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    private Double bmi;

    @ElementCollection(targetClass = DayOfWeek.class)
    @Enumerated(EnumType.STRING)
    private List<DayOfWeek> availableDays;

    // Getters and Setters
}
```

#### ExercisePlan Entity
```java
@Entity
public class ExercisePlan {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String description;

    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Exercise> exercises;

    // Getters and Setters
}
```

#### Exercise Entity
```java
@Entity
public class Exercise {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String description;
    private Integer duration; // Duration in minutes
    private Integer sets;
    private Integer restTime; // Rest time in seconds

    // Getters and Setters
}
```

#### WorkoutSession Entity
```java
@Entity
public class WorkoutSession {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @ManyToOne
    private User user;

    @ManyToOne
    private ExercisePlan exercisePlan;

    private LocalDate date;

    // Getters and Setters
}
```

### Controllers

#### UserController
```java
@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User createdUser = userService.createUser(user);
        return ResponseEntity.ok(createdUser);
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.getUser(id);
        return ResponseEntity.ok(user);
    }

    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
        User updatedUser = userService.updateUser(id, user);
        return ResponseEntity.ok(updatedUser);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return ResponseEntity.noContent().build();
    }
}
```

#### ExercisePlanController
```java
@RestController
@RequestMapping("/exercise-plans")
public class ExercisePlanController {

    @Autowired
    private ExercisePlanService exercisePlanService;

    @GetMapping
    public ResponseEntity<List<ExercisePlan>> getAllExercisePlans() {
        List<ExercisePlan> plans = exercisePlanService.getAllExercisePlans();
        return ResponseEntity.ok(plans);
    }

    @GetMapping("/{id}")
    public ResponseEntity<ExercisePlan> getExercisePlan(@PathVariable Long id) {
        ExercisePlan plan = exercisePlanService.getExercisePlan(id);
        return ResponseEntity.ok(plan);
    }

    @PostMapping
    public ResponseEntity<ExercisePlan> createExercisePlan(@RequestBody ExercisePlan plan) {
        ExercisePlan createdPlan = exercisePlanService.createExercisePlan(plan);
        return ResponseEntity.ok(createdPlan);
    }

    @PutMapping("/{id}")
    public ResponseEntity<ExercisePlan> updateExercisePlan(@PathVariable Long id, @RequestBody ExercisePlan plan) {
        ExercisePlan updatedPlan = exercisePlanService.updateExercisePlan(id, plan);
        return ResponseEntity.ok(updatedPlan);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteExercisePlan(@PathVariable Long id) {
        exercisePlanService.deleteExercisePlan(id);
        return ResponseEntity.noContent().build();
    }
}
```

#### WorkoutSessionController
```java
@RestController
@RequestMapping("/workout-sessions")
public class WorkoutSessionController {

    @Autowired
    private WorkoutSessionService workoutSessionService;

    @PostMapping
    public ResponseEntity<WorkoutSession> createWorkoutSession(@RequestBody WorkoutSession session) {
        WorkoutSession createdSession = workoutSessionService.createWorkoutSession(session);
        return ResponseEntity.ok(createdSession);
    }

    @GetMapping("/{id}")
    public ResponseEntity<WorkoutSession> getWorkoutSession(@PathVariable Long id) {
        WorkoutSession session = workoutSessionService.getWorkoutSession(id);
        return ResponseEntity.ok(session);
    }

    @GetMapping("/user/{userId}")
    public ResponseEntity<List<WorkoutSession>> getAllWorkoutSessionsForUser(@PathVariable Long userId) {
        List<WorkoutSession> sessions = workoutSessionService.getAllWorkoutSessionsForUser(userId);
        return ResponseEntity.ok(sessions);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteWorkoutSession(@PathVariable Long id) {
        workoutSessionService.deleteWorkoutSession(id);
        return ResponseEntity.noContent().build();
    }
}
```

### Services

Each controller interacts with a service layer that contains the business logic.

#### UserService
```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public User getUser(Long id) {
        return userRepository.findById(id).orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }

    public User updateUser(Long id, User user) {
        User existingUser = getUser(id);
        existingUser.setName(user.getName());
        existingUser.setEmail(user.getEmail());
        existingUser.setBmi(user.getBmi());
        existingUser.setAvailableDays(user.getAvailableDays());
        return userRepository.save(existingUser);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

#### ExercisePlanService
```java
@Service
public class ExercisePlanService {

    @Autowired
    private ExercisePlanRepository exercisePlanRepository;

    public List<ExercisePlan> getAllExercisePlans() {
        return exercisePlanRepository.findAll();
    }

    public ExercisePlan getExercisePlan(Long id) {
        return exercisePlanRepository.findById(id).orElseThrow(() -> new ResourceNotFoundException("Exercise Plan not found"));
    }

    public ExercisePlan createExercisePlan(ExercisePlan plan) {
        return exercisePlanRepository.save(plan);
    }

    public ExercisePlan updateExercisePlan(Long id, ExercisePlan plan) {
        ExercisePlan existingPlan = getExercisePlan(id);
        existingPlan.setName(plan.getName());
        existingPlan.setDescription(plan.getDescription());
        existingPlan.setExercises(plan.getExercises());
        return exercisePlanRepository.save(existingPlan);
    }

    public void deleteExercisePlan(Long id) {
        exercisePlanRepository.deleteById(id);
    }
}
```

#### WorkoutSessionService
```java
@Service
public class WorkoutSessionService {

    @Autowired


    private WorkoutSessionRepository workoutSessionRepository;

    public WorkoutSession createWorkoutSession(WorkoutSession session) {
        return workoutSessionRepository.save(session);
    }

    public WorkoutSession getWorkoutSession(Long id) {
        return workoutSessionRepository.findById(id).orElseThrow(() -> new ResourceNotFoundException("Workout Session not found"));
    }

    public List<WorkoutSession> getAllWorkoutSessionsForUser(Long userId) {
        return workoutSessionRepository.findByUserId(userId);
    }

    public void deleteWorkoutSession(Long id) {
        workoutSessionRepository.deleteById(id);
    }
}
```

### Repositories

Each service interacts with a repository layer for database operations.

#### UserRepository
```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```

#### ExercisePlanRepository
```java
public interface ExercisePlanRepository extends JpaRepository<ExercisePlan, Long> {
}
```

#### WorkoutSessionRepository
```java
public interface WorkoutSessionRepository extends JpaRepository<WorkoutSession, Long> {
    List<WorkoutSession> findByUserId(Long userId);
}
```

### Summary

This breakdown includes the data models, services, and APIs necessary for a gym app using Spring Boot. Each part of the application is structured to handle different aspects of user interactions, from selecting an exercise plan to managing workout sessions and providing reminders. This approach ensures a modular and maintainable application design.
