# Gym-app
Use case for a  gym app that helps users choose an exercise plan, enter their details, and provides reminders and timers for their workouts.

### Use Case: Gym App

**Primary Actor**: User (Gym Member)

**Preconditions**: The user has installed the gym app on their device.

**Postconditions**: The user receives a personalized exercise plan and reminders for workout days.

#### Use Case Diagram
```plaintext
+---------------------+        +-----------------+
|                     |        |                 |
|   Choose Exercise   | -----> |   Enter Details |
|        Plan         |        |                 |
|                     |        +-----------------+
+---------------------+                 |
                                        v
                          +-------------------------+
                          |                         |
                          |   Generate Exercise     |
                          |        Plan             |
                          |                         |
                          +-------------------------+
                                        |
                                        v
                          +-------------------------+
                          |                         |
                          |    Set Workout Days     |
                          |       Reminders         |
                          |                         |
                          +-------------------------+
                                        |
                                        v
                          +-------------------------+
                          |                         |
                          |       Start Workout     |
                          |                         |
                          +-------------------------+
                                        |
                                        v
                          +-------------------------+
                          |                         |
                          |   Timer for Rest Time   |
                          |       Between Sets      |
                          |                         |
                          +-------------------------+
```

### Use Case Description

#### 1. **Choose Exercise Plan**
   - **Actor**: User
   - **Description**: The user selects one of the three available exercise plans (e.g., Beginner, Intermediate, Advanced).
   - **Trigger**: User opens the app and chooses an exercise plan.
   - **Preconditions**: The user is logged into the app.
   - **Postconditions**: The selected exercise plan is set for the user.

#### 2. **Enter Details**
   - **Actor**: User
   - **Description**: The user enters their personal details, such as name, days of the week they have time to practice, and BMI.
   - **Trigger**: User selects an exercise plan.
   - **Preconditions**: The user has chosen an exercise plan.
   - **Postconditions**: User details are saved in the app.

#### 3. **Generate Exercise Plan**
   - **Actor**: System
   - **Description**: Based on the user’s details, the app generates a personalized exercise plan.
   - **Trigger**: User submits their details.
   - **Preconditions**: User details are entered.
   - **Postconditions**: A personalized exercise plan is created and saved for the user.

#### 4. **Set Workout Days Reminders**
   - **Actor**: System
   - **Description**: The app sets reminders for the user on the days they have chosen to work out.
   - **Trigger**: Personalized exercise plan is generated.
   - **Preconditions**: User has provided their available workout days.
   - **Postconditions**: Reminders are scheduled on the user’s device for workout days.

#### 5. **Start Workout**
   - **Actor**: User
   - **Description**: On the workout day, the user starts the exercise session through the app.
   - **Trigger**: User receives a reminder for the workout day.
   - **Preconditions**: It is a scheduled workout day.
   - **Postconditions**: The workout session begins.

#### 6. **Timer for Rest Time Between Sets**
   - **Actor**: System
   - **Description**: The app provides a timer to count the resting time between exercise sets.
   - **Trigger**: User completes a set of exercises during the workout session.
   - **Preconditions**: User is in an active workout session.
   - **Postconditions**: The user is notified when the rest time is over, and they should proceed to the next set.

### Use Case Steps

#### Choose Exercise Plan
1. User opens the app.
2. User navigates to the exercise plan selection screen.
3. User selects one of the three available plans (Beginner, Intermediate, Advanced).

#### Enter Details
4. User enters their name.
5. User selects the days of the week they are available for workouts.
6. User enters their BMI.
7. User submits the information.

#### Generate Exercise Plan
8. System processes the user's details.
9. System generates a personalized exercise plan based on the selected plan and user details.

#### Set Workout Days Reminders
10. System schedules reminders for the selected workout days.
11. System sends a confirmation notification to the user about the scheduled reminders.

#### Start Workout
12. On a scheduled workout day, the user receives a reminder.
13. User opens the app and starts the workout session.

#### Timer for Rest Time Between Sets
14. User completes the first set of exercises.
15. System starts a rest timer.
16. System notifies the user when the rest period is over.
17. User proceeds to the next set of exercises.
18. Steps 14-17 repeat until the workout session is completed.

### Summary
This use case provides a clear flow for how a gym app can guide a user from choosing an exercise plan, entering personal details, generating a personalized workout plan, setting reminders, and managing the workout sessions with rest timers. This ensures that users stay on track with their fitness goals efficiently.
