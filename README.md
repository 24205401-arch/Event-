# Event Attendance System (Spring Boot + H2)

A full-stack event management &amp; attendance app with **two separate logins**:

- **Admin login** — fixed account: `User ID: A401`, `Password: 123456`
  Admin can create events and view which users have joined/attended each one.
- **User login** — users register their own account, then log in to join events
  and mark themselves as attended.

## Stack

- Java 17, Spring Boot 3.3.4
- Spring MVC + Thymeleaf (server-rendered pages, no separate frontend build)
- Spring Data JPA + **H2 database** (file-based, so data survives restarts)
- Maven

## Project Structure

```
src/main/java/com/eventapp/
  EventAttendanceApplication.java   - main class
  model/        - User, EventItem, EventRegistration (JPA entities)
  repository/   - Spring Data JPA repositories
  service/      - UserService (login/register), EventService (events, join, attend)
  controller/   - AuthController, UserEventController, AdminController
  config/       - DataSeeder (creates the fixed admin account on first run)

src/main/resources/
  application.properties  - H2 + JPA config
  templates/               - Thymeleaf HTML pages
  static/css/style.css     - shared styling
```

## How to run (VS Code / IntelliJ / terminal)

1. Make sure you have **JDK 17+** and **Maven** installed.
2. Open the project folder.
3. Run:
   ```
   mvn spring-boot:run
   ```
   or build a jar and run it:
   ```
   mvn clean package
   java -jar target/event-attendance-1.0.0.jar
   ```
4. Open **http://localhost:8080** in your browser.

## Logging in

### Admin
- Go to **Admin Login** on the home page.
- User ID: `A401`
- Password: `123456`
- This account is auto-created the first time the app starts (see `DataSeeder.java`), so you don't need to insert it manually.

### User
- Go to **User Login** → **Register a new account**.
- Fill in Full Name, choose a User ID and Password, submit.
- Then log in with those same credentials on the User Login page.

## What each side can do

**Admin:**
- Create new events (title, description, venue, date).
- Delete events.
- Click **View Joined Users** on any event to see every user who joined it,
  when they joined, and whether they've marked themselves attended.

**User:**
- Browse all posted events on the Board.
- Click **Join Event** to register interest.
- Once joined, click **Mark Attended** to confirm attendance (visible to admin).

## Database

H2 is file-based at `./data/eventdb` (created automatically on first run —
no manual setup needed). To inspect the tables directly, start the app and
open the built-in H2 web console:

- URL: **http://localhost:8080/h2-console**
- JDBC URL: `jdbc:h2:file:./data/eventdb`
- Username: `sa`
- Password: *(leave blank)*

Tables created automatically: `app_user`, `event_item`, `event_registration`.

## Notes

- Passwords are stored as plain text in this version to keep the codebase
  simple and easy to explain/demo. For anything beyond a student project,
  swap in `BCryptPasswordEncoder` from `spring-boot-starter-security`.
- Auth is a simple `HttpSession` attribute check (no Spring Security), which
  keeps the controllers short and easy to read for a viva/demo.
