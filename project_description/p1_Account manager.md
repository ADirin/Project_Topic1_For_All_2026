# 📋 Table of Contents

- [Project Overview](#project-overview)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Sprint 1: Database Implementation](#sprint-1-database-implementation)
- [Sprint 2: Unit Testing & Code Coverage](#sprint-2-unit-testing--code-coverage)
- [Sprint 3: Jenkins CI/CD](#sprint-3-jenkins-cicd)
- [Sprint 4: Docker & Xming](#sprint-4-docker--xming)
- [Non-Functional Requirements](#non-functional-requirements)
- [Acceptance Criteria](#acceptance-criteria)
- [Learning Outcomes](#learning-outcomes)
- [Grading](#grading)
- [Resources](#resources)

---

# 📖 Project Overview

## Duration

**8 Weeks** (4 Sprints, 2 Weeks Each)

## Goal

Build a complete DevOps pipeline for a JavaFX FXML application with full CI/CD integration, containerization, and GUI deployment.

## Team

**Size:** 3–4 members per group

---

# 🛠 Technology Stack

## Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 17 LTS | Application Language |
| JavaFX | 17.0.2 | UI Framework |
| FXML | - | UI Layout |
| MariaDB | 10.11 | Database |
| Maven | 3.8.6 | Build Tool |

# 📂 Project Structure

```text
account-app/
├── pom.xml                                    # Maven configuration
├── README.md                                  # Project documentation
├── LICENSE                                    # License file
│
├── docs/
│   ├── architecture-diagram.png               # System architecture
│   ├── user-guide.md                          # User guide
│   └── deployment-guide.md                    # Deployment guide
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── accountapp/
│   │   │           ├── Main.java
│   │   │           ├── controller/
│   │   │           │   └── AccountController.java
│   │   │           ├── model/
│   │   │           │   ├── User.java
│   │   │           │   └── DatabaseConnection.java
│   │   │           ├── dao/
│   │   │           │   └── UserDAO.java
│   │   │           ├── service/
│   │   │           │   └── ValidationService.java
│   │   │           └── utils/
│   │   │               ├── PasswordHasher.java
│   │   │               └── EmailValidator.java
│   │   │
│   │   └── resources/
│   │       ├── fxml/
│   │       │   └── account-view.fxml
│   │       ├── styles/
│   │       │   └── application.css
│   │       └── images/
│   │           └── logo.png
│   │
│   └── test/
│       ├── java/
│       │   └── com/
│       │       └── accountapp/
│       │           ├── dao/
│       │           │   └── UserDAOTest.java
│       │           ├── service/
│       │           │   └── ValidationServiceTest.java
│       │           └── integration/
│       │               └── DatabaseIntegrationTest.java
│       └── resources/
│           └── test-database.properties
│
├── database/
│   ├── schema.sql
│   └── data.sql
│
├── docker/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   ├── .dockerignore
│   ├── mariadb/
│   │   └── init.sql
│   └── jenkins/
│       └── Dockerfile
│
├── jenkins/
│   └── Jenkinsfile
│
└── scripts/
    ├── setup-db.sh
    ├── run-app.sh
    ├── run-gui-windows.sh
    ├── run-gui-linux.sh
    └── run-gui-mac.sh
```

# 🏗 Sprint 1: Database Implementation (Weeks 1–2)

## 🎯 Sprint Goal

Implement the core application with a JavaFX FXML interface and MariaDB database integration.

---

# ✨ Application Features

## User Interface (FXML)

### Form Fields

| Field | Type | Validation |
|-------|------|------------|
| Last Name | Text Field | Required, minimum 2 characters |
| First Name | Text Field | Required, minimum 2 characters |
| Email | Text Field | Required, valid email format |
| Phone Number | Text Field | Required, valid phone number |
| Username | Text Field | Required, minimum 4 characters |
| Password | Password Field | Required, minimum 6 characters |
| Re-type Password | Password Field | Must match password |

### UI Features

- ✅ Real-time validation with visual feedback
- ✅ Password strength indicator (Weak / Medium / Strong)
- ✅ Email validation while typing
- ✅ Tooltips with validation hints
- ✅ Error labels displayed next to invalid fields
- ✅ Responsive layout
- ✅ Keyboard shortcuts
  - **Enter** → Submit
  - **Esc** → Cancel

### Buttons

| Button | Function |
|---------|----------|
| Create Account | Submit the form |
| Clear Fields | Reset all fields |
| Cancel | Close the application |

---

# 🗄 Database Implementation

## Schema Design

**`database/schema.sql`**

```sql
CREATE DATABASE accountdb;
USE accountdb;

-- Users table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    last_name VARCHAR(50) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20) NOT NULL,
    username VARCHAR(50) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        ON UPDATE CURRENT_TIMESTAMP,
    is_active BOOLEAN DEFAULT TRUE,
    last_login TIMESTAMP NULL
);

-- Audit log table
CREATE TABLE audit_log (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    action VARCHAR(50) NOT NULL,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    details TEXT,
    FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE SET NULL
);

-- Performance indexes
CREATE INDEX idx_email ON users(email);
CREATE INDEX idx_username ON users(username);
CREATE INDEX idx_created_at ON users(created_at);
```

## Database Features

- 🔌 Connection pooling with **HikariCP**
- 🛡 Prepared statements to prevent SQL injection
- 💾 Transaction management for atomic operations
- 📝 Audit logging for all user operations
- 🔄 Migration scripts for version control

## DAO Operations

| Method | Description |
|---------|-------------|
| `createUser(User user)` | Insert a new user |
| `getUserById(int id)` | Retrieve a user by ID |
| `getUserByEmail(String email)` | Retrieve a user by email |
| `getUserByUsername(String username)` | Retrieve a user by username |
| `updateUser(User user)` | Update user details |
| `deleteUser(int id)` | Soft delete a user |
| `getAllUsers()` | List all active users |
| `authenticateUser(String username, String password)` | Authenticate a user |

---

# 📦 Sprint 1 Deliverables

## Code

- Complete JavaFX FXML UI
- JavaFX controller implementation
- Database schema and migration scripts
- DAO implementation with CRUD operations
- Database connection utility

## Documentation

- Database schema diagram
- Setup instructions
- User guide
- DAO API documentation

## Testing

- Manual testing of all form validations
- Database connection testing
- Basic CRUD operation testing

---

# 🧪 Sprint 2: Unit Testing & Code Coverage (Weeks 3–4)

## 🎯 Sprint Goal

Implement comprehensive unit testing, achieve **80%+ code coverage**, and configure a Jenkins CI/CD pipeline.

---

# 🧬 Testing Implementation

## Unit Testing with JUnit 5

### Test Categories

#### 1. Model Tests

```java
@ExtendWith(MockitoExtension.class)
public class UserTest {

    @Test
    void testUserCreation_ValidData_Success() {

        User user = new User(
            "Doe",
            "John",
            "john@example.com",
            "1234567890",
            "johndoe",
            "password"
        );

        assertNotNull(user);
        assertEquals("Doe", user.getLastName());
        assertEquals("John", user.getFirstName());
        assertEquals("john@example.com", user.getEmail());
    }
    
    @Test
    void testUser_InvalidEmail_ThrowsException() {
        assertThrows(IllegalArgumentException.class, () -> {
            new User("Doe", "John", "invalid-email", 
                    "1234567890", "johndoe", "password");
        });
    }
}
````
#### 2. Service Layer Tests
java
@ExtendWith(MockitoExtension.class)
public class ValidationServiceTest {
    private ValidationService validationService;
    
    @BeforeEach
    void setUp() {
        validationService = new ValidationService();
    }
    
    @Test
    void testValidateEmail_Valid_Success() {
        assertTrue(validationService.isValidEmail("user@example.com"));
        assertTrue(validationService.isValidEmail("user.name@example.co.uk"));
    }
    
    @Test
    void testValidateEmail_Invalid_Failure() {
        assertFalse(validationService.isValidEmail("invalid"));
        assertFalse(validationService.isValidEmail("user@"));
        assertFalse(validationService.isValidEmail("@example.com"));
    }
    
    @Test
    void testPasswordStrength_Weak_ReturnsLowScore() {
        int strength = validationService.checkPasswordStrength("123");
        assertTrue(strength < 3);
    }
    
    @Test
    void testPasswordStrength_Strong_ReturnsHighScore() {
        int strength = validationService.checkPasswordStrength("Str0ng!Passw0rd");
        assertTrue(strength >= 4);
    }
}
#### 3. DAO Layer Tests
java
@ExtendWith(MockitoExtension.class)
public class UserDAOTest {
    @Mock
    private DataSource mockDataSource;
    
    @Mock
    private Connection mockConnection;
    
    @Mock
    private PreparedStatement mockStatement;
    
    @Mock
    private ResultSet mockResultSet;
    
    @InjectMocks
    private UserDAO userDAO;
    
    @Test
    void testCreateUser_Success() throws SQLException {
        User user = new User("Doe", "John", "john@example.com", 
                            "1234567890", "johndoe", "password");
        
        when(mockDataSource.getConnection()).thenReturn(mockConnection);
        when(mockConnection.prepareStatement(anyString(), anyInt()))
            .thenReturn(mockStatement);
        when(mockStatement.executeUpdate()).thenReturn(1);
        
        boolean result = userDAO.createUser(user);
        assertTrue(result);
        
        verify(mockStatement).setString(1, "Doe");
        verify(mockStatement).setString(2, "John");
        verify(mockStatement).setString(3, "john@example.com");
    }
    
    @Test
    void testCreateUser_DuplicateEmail_ThrowsException() {
        User user = new User("Doe", "John", "exists@example.com", 
                            "1234567890", "johndoe", "password");
        
        when(mockDataSource.getConnection()).thenReturn(mockConnection);
        when(mockConnection.prepareStatement(anyString(), anyInt()))
            .thenReturn(mockStatement);
        
        SQLException sqlException = new SQLException("Duplicate entry", "23000");
        when(mockStatement.executeUpdate()).thenThrow(sqlException);
        
        assertThrows(SQLException.class, () -> {
            userDAO.createUser(user);
        });
    }
}
#### 4. Integration Tests
java
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class DatabaseIntegrationTest {
    private static final String DB_URL = "jdbc:mariadb://localhost:3306/accountdb_test";
    private static final String DB_USER = "testuser";
    private static final String DB_PASSWORD = "testpass";
    
    private UserDAO userDAO;
    private Connection connection;
    
    @BeforeAll
    void setUp() throws SQLException {
        connection = DriverManager.getConnection(DB_URL, DB_USER, DB_PASSWORD);
        userDAO = new UserDAO(connection);
        
        // Initialize test database
        try (Statement stmt = connection.createStatement()) {
            stmt.execute("TRUNCATE TABLE users");
            stmt.execute("ALTER TABLE users AUTO_INCREMENT = 1");
        }
    }
    
    @Test
    void testFullUserLifecycle() throws SQLException {
        // Create
        User user = new User("Test", "User", "test@example.com", 
                            "1234567890", "testuser", "password");
        boolean created = userDAO.createUser(user);
        assertTrue(created);
        
        // Read
        User retrieved = userDAO.getUserByEmail("test@example.com");
        assertNotNull(retrieved);
        assertEquals("Test", retrieved.getLastName());
        
        // Update
        retrieved.setLastName("Updated");
        boolean updated = userDAO.updateUser(retrieved);
        assertTrue(updated);
        
        // Verify update
        User updatedUser = userDAO.getUserById(retrieved.getId());
        assertEquals("Updated", updatedUser.getLastName());
        
        // Delete
        boolean deleted = userDAO.deleteUser(retrieved.getId());
        assertTrue(deleted);
        
        // Verify deletion
        User deletedUser = userDAO.getUserById(retrieved.getId());
        assertNull(deletedUser);
    }
}
## 📊 JaCoCo Code Coverage
### Coverage Targets
Metric	Target
Line Coverage	≥ 80%
Branch Coverage	≥ 70%
Class Coverage	≥ 90%
Method Coverage	≥ 85%
Coverage Reports
📊 HTML report in target/site/jacoco
📄 XML report for CI/CD integration
📈 Dashboard with coverage trends
Jenkins Pipeline Setup
Jenkinsfile
pipeline {
agent any
    tools {
        maven 'maven-3.8.6'
        jdk 'openjdk-17'
    }
    
    environment {
        DATABASE_URL = 'jdbc:mariadb://localhost:3306/accountdb_test'
        MAVEN_OPTS = '-Xmx2048m'
        SONAR_HOST_URL = 'http://sonarqube:9000'
    }
    
    stages {
        stage('📦 SCM Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/your-org/account-app.git',
                    credentialsId: 'github-credentials'
            }
        }
        
        stage('🔍 Code Quality Analysis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.host.url=${SONAR_HOST_URL}'
            }
        }
        
        stage('🔨 Build') {
            steps {
                sh 'mvn clean compile'
            }
            post {
                success {
                    echo 'Build completed successfully! ✅'
                }
                failure {
                    echo 'Build failed! ❌'
                }
            }
        }
        
        stage('🧪 Unit Tests') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                    publishHTML([
                        reportDir: 'target/surefire-reports',
                        reportFiles: 'index.html',
                        reportName: 'Test Results'
                    ])
                }
            }
        }
        
        stage('🔗 Integration Tests') {
            steps {
                sh 'mvn verify -Pintegration-tests'
            }
        }
        
        stage('📊 Code Coverage Report') {
            steps {
                sh 'mvn jacoco:report'
            }
            post {
                always {
                    publishHTML([
                        reportDir: 'target/site/jacoco',
                        reportFiles: 'index.html',
                        reportName: 'Code Coverage Report'
                    ])
                }
            }
        }
        
        stage('📦 Package') {
            steps {
                sh 'mvn package'
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
        
        stage('🚀 Deploy to Staging') {
            when {
                branch 'main'
                expression { currentBuild.result == 'SUCCESS' }
            }
            steps {
                sh 'mvn deploy'
            }
        }
    }
    
    post {
        success {
            emailext (
                subject: "✅ SUCCESS: ${env.JOB_NAME} - Build ${env.BUILD_NUMBER}",
                body: "The build was successful.\n\n" +
                      "Build URL: ${env.BUILD_URL}\n" +
                      "Commit: ${env.GIT_COMMIT}\n" +
                      "Branch: ${env.GIT_BRANCH}",
                to: 'team@example.com'
            )
        }
        failure {
            emailext (
                subject: "❌ FAILURE: ${env.JOB_NAME} - Build ${env.BUILD_NUMBER}",
                body: "The build has failed.\n\n" +
                      "Build URL: ${env.BUILD_URL}\n" +
                      "Commit: ${env.GIT_COMMIT}\n" +
                      "Branch: ${env.GIT_BRANCH}",
                to: 'team@example.com'
            )
        }
    }
}
### 📦 Sprint 2 Deliverables
### Testing
- Complete JUnit test suite
- Mockito mocks for database testing
- Test coverage reports (80%+)
- Integration tests
- Performance tests

#### CI/CD
- Jenkins pipeline configuration
- Automated test execution
- Code quality gates
- Email notifications
- Artifact storage

#### Documentation
- Testing strategy document
- Coverage report analysis
- Jenkins setup guide
- Continuous integration guide

## 🔧 Sprint 3: Jenkins CI/CD Implementation (Weeks 5-6)
### Sprint Goal
Implement comprehensive CI/CD pipeline with Jenkins, including code quality gates, automated builds, and deployment.

### 🔄 CI/CD Pipeline Features
#### Jenkins Configuration
#### Multibranch Pipeline:
🌿 Branch detection (main, develop, feature/*)
🔄 Automatic build triggers on commit
📊 Build status badges
🔀 Pull request integration

#### Pipeline Stages:
Stage	Purpose	Tools
1. SCM Checkout	Git clone with credentials	Git
2. Code Quality	Static code analysis	SonarQube, PMD
3. Build	Compile and package	Maven
4. Unit Tests	Execute test suite	JUnit, Mockito
5. Integration Tests	Database integration	TestNG
6. Coverage	Code coverage report	JaCoCo
7. Security	Vulnerability scanning	OWASP, Snyk
8. Package	Create JAR artifact	Maven
9. Deploy	Deploy to staging	Scripts

### Enhanced Application Features
#### Logging with Log4j2
````xml
<!-- src/main/resources/log4j2.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
        <RollingFile name="RollingFile" fileName="logs/application.log"
                     filePattern="logs/application-%d{yyyy-MM-dd}.log">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy interval="1" modulate="true"/>
            </Policies>
        </RollingFile>
    </Appenders>
    <Loggers>
        <Root level="INFO">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="RollingFile"/>
        </Root>
    </Loggers>
</Configuration>
````
#### Configuration Management

````properties
# src/main/resources/application.properties
# Database Configuration
db.host=localhost
db.port=3306
db.name=accountdb
db.user=appuser
db.password=app123
Security
bcrypt.strength=10
jwt.secret=mySecretKey
Email Configuration
email.smtp.host=smtp.gmail.com
email.smtp.port=587
email.username=no-reply@example.com
email.password=emailpassword
Logging
log.level=INFO
log.file=logs/application.log
````

#### Monitoring & Error Tracking
- 📊 Application health checks
- 📈 Performance metrics with Micrometer
- 🔍 Error tracking with Sentry
- 📉 Custom dashboard (Grafana)

### 📦 Sprint 3 Deliverables
#### CI/CD
- Complete Jenkins pipeline
- Automated build and deployment
- Code quality gates
- Email notifications
- Build status badges

#### Monitoring
- Application logging setup
- Performance monitoring
- Error tracking
- Health checks

#### Documentation
- CI/CD pipeline documentation
- Deployment guide
- Troubleshooting guide
- SLA/SLO documentation
####🐳 Sprint 4: Docker & Xming (Weeks 7-8)
### Sprint Goal
Containerize the application with Docker, implement Docker Compose for orchestration, and enable GUI display using Xming.

### 🐳 Docker Implementation
#### Containerization Strategy
#### Multi-stage Docker Build
````dockerfile
docker/Dockerfile
Stage 1: Build
FROM maven:3.8.6-openjdk-17 AS build
LABEL maintainer="devops-team@example.com"
LABEL version="1.0.0"
WORKDIR /app
Copy pom.xml and download dependencies
COPY pom.xml .
RUN mvn dependency:go-offline
Copy source and build
COPY src ./src
RUN mvn clean package -DskipTests
Stage 2: Runtime
FROM openjdk:17-jdk-slim
LABEL maintainer="devops-team@example.com"
LABEL version="1.0.0"
WORKDIR /app
Install X11 dependencies for GUI
RUN apt-get update && apt-get install -y   
libx11-6   
libxext6   
libxrender1   
libxtst6   
libxi6   
libxcb1   
libx11-xcb1   
&& rm -rf /var/lib/apt/lists/*
Create non-root user
RUN groupadd -r appuser && useradd -m -r -g appuser appuser
Copy JAR from build stage
COPY --from=build /app/target/account-app-*.jar app.jar
Switch to non-root user
USER appuser
Health check
HEALTHCHECK --interval=30s --timeout=3s   
CMD java -cp app.jar com.accountapp.HealthCheck || exit 1
Expose ports
EXPOSE 5900
Start application
ENTRYPOINT ["java", "-jar", "app.jar"]
````
### Docker Compose Orchestration

````yaml
docker/docker-compose.yml
version: '3.8'
services:
Database Service
mariadb:
image: mariadb:10.11
container_name: account-db
environment:
MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD:-root123}
MYSQL_DATABASE: ${DB_NAME:-accountdb}
MYSQL_USER: ${DB_USER:-appuser}
MYSQL_PASSWORD: ${DB_PASSWORD:-app123}
volumes:
- mariadb_data:/var/lib/mysql
- ./mariadb/init.sql:/docker-entrypoint-initdb.d/init.sql
- ./mariadb/my.cnf:/etc/mysql/conf.d/my.cnf
ports:
- "3306:3306"
networks:
- app-network
healthcheck:
test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
interval: 10s
timeout: 5s
retries: 5
deploy:
resources:
limits:
memory: 512M
reservations:
memory: 256M
Application Service
app:
build: .
container_name: account-app
depends_on:
mariadb:
condition: service_healthy
environment:
DATABASE_HOST: mariadb
DATABASE_PORT: 3306
DATABASE_NAME: ${DB_NAME:-accountdb}
DATABASE_USER: ${DB_USER:-appuser}
DATABASE_PASSWORD: ${DB_PASSWORD:-app123}
DISPLAY: ${DISPLAY:-host.docker.internal:0}
JAVA_OPTS: "-Xms256m -Xmx512m"
volumes:
- /tmp/.X11-unix:/tmp/.X11-unix
- ./logs:/app/logs
- ./config:/app/config
ports:
- "5900:5900"
networks:
- app-network
deploy:
resources:
limits:
memory: 512M
reservations:
memory: 256M
restart: unless-stopped
Jenkins Service
jenkins:
image: jenkins/jenkins:lts-jdk17
container_name: account-jenkins
environment:
JENKINS_OPTS: "--httpPort=8080"
volumes:
- jenkins_home:/var/jenkins_home
- /var/run/docker.sock:/var/run/docker.sock
- ./jenkins/jenkins.yaml:/var/jenkins_home/jenkins.yaml
ports:
- "8080:8080"
- "50000:50000"
networks:
- app-network
restart: unless-stopped
volumes:
mariadb_data:
driver: local
jenkins_home:
driver: local
networks:
app-network:
driver: bridge
ipam:
config:
- subnet: 172.28.0.0/16
````
### 🖥 Xming Integration
#### Windows with Xming
````bash
#!/bin/bash
scripts/run-gui-windows.sh
echo "🚀 Starting Account Application with Xming..."
Check if Xming is running
if ! pslist Xming > /dev/null 2>&1; then
echo "❌ Xming is not running. Please start Xming first."
echo "   1. Download Xming from: http://www.straightrunning.com/Xming/"
echo "   2. Install and run Xming"
echo "   3. Try again"
exit 1
fi
echo "✅ Xming detected"
Set DISPLAY for Windows
export DISPLAY=host.docker.internal:0.0
Check Docker is running
if ! docker info > /dev/null 2>&1; then
echo "❌ Docker is not running. Please start Docker first."
exit 1
fi
echo "✅ Docker detected"
Run the container with GUI support
echo "🔄 Starting container..."
docker run -it   
--rm   
--name account-app-gui   
-e DISPLAY=$DISPLAY   
-e QT_X11_NO_MITSHM=1   
-v /tmp/.X11-unix:/tmp/.X11-unix   
--network account-app_app-network   
--env-file .env   
account-app:latest   
java -jar app.jar
echo "✅ Application closed"
````

#### Linux with X11
````bash
#!/bin/bash
scripts/run-gui-linux.sh
echo "🚀 Starting Account Application on Linux..."
Allow X server connections
echo "🔄 Allowing X server connections..."
xhost +local:docker
Set DISPLAY
export DISPLAY=:0
Check Docker is running
if ! docker info > /dev/null 2>&1; then
echo "❌ Docker is not running. Please start Docker first."
exit 1
fi
echo "✅ Docker detected"
Run container
echo "🔄 Starting container..."
docker run -it   
--rm   
--name account-app-gui   
-e DISPLAY=$DISPLAY   
-v /tmp/.X11-unix:/tmp/.X11-unix   
--network account-app_app-network   
--env-file .env   
account-app:latest   
java -jar app.jar
Revoke permissions
echo "🔄 Revoking X server permissions..."
xhost -local:docker
echo "✅ Application closed"
````

#### Mac with XQuartz
````bash
#!/bin/bash
scripts/run-gui-mac.sh
echo "🚀 Starting Account Application on Mac..."
Check if XQuartz is running
if ! ps aux | grep -v grep | grep XQuartz > /dev/null; then
echo "❌ XQuartz is not running."
echo "   Please start XQuartz from Applications/Utilities"
exit 1
fi
echo "✅ XQuartz detected"
Allow connections from localhost
echo "🔄 Allowing X server connections..."
xhost +localhost
Set DISPLAY
export DISPLAY=docker.for.mac.localhost:0
Check Docker is running
if ! docker info > /dev/null 2>&1; then
echo "❌ Docker is not running. Please start Docker first."
exit 1
fi
echo "✅ Docker detected"
Run container
echo "🔄 Starting container..."
docker run -it   
--rm   
--name account-app-gui   
-e DISPLAY=$DISPLAY   
-v /tmp/.X11-unix:/tmp/.X11-unix   
--network account-app_app-network   
--env-file .env   
account-app:latest   
java -jar app.jar
Revoke permissions
echo "🔄 Revoking X server permissions..."
xhost -localhost
echo "✅ Application closed"
````
#### Environment Variables
````bash
.env - Environment configuration
Database
DB_ROOT_PASSWORD=root123
DB_NAME=accountdb
DB_USER=appuser
DB_PASSWORD=app123
Application
APP_NAME=AccountApp
APP_VERSION=1.0.0
JAVA_OPTS=-Xms256m -Xmx512m
Security
BCRYPT_STRENGTH=10
JWT_SECRET=mySuperSecretKey123!
Display
DISPLAY=host.docker.internal:0.0

````
#### 🏥 Health Check
````java
// src/main/java/com/accountapp/HealthCheck.java
package com.accountapp;
import com.accountapp.model.DatabaseConnection;
import java.sql.Connection;
public class HealthCheck {
    public static void main(String[] args) {
        boolean healthy = true;
        
        // Check database connection
        try {
            Connection conn = DatabaseConnection.getInstance().getConnection();
            if (conn != null && !conn.isClosed()) {
                System.out.println("✅ Database: OK");
                conn.close();
            } else {
                System.out.println("❌ Database: FAILED");
                healthy = false;
            }
        } catch (Exception e) {
            System.out.println("❌ Database: ERROR - " + e.getMessage());
            healthy = false;
        }
        
        // Check application status
        if (healthy) {
            System.out.println("✅ Application: HEALTHY");
            System.exit(0);
        } else {
            System.out.println("❌ Application: UNHEALTHY");
            System.exit(1);
        }
    }
}
````
### 📦 Sprint 4 Deliverables
#### Docker
- Dockerfile with multi-stage build
- Docker Compose configuration
- Volume management for data persistence
- Network configuration
- Health checks
- Resource limits
#### Xming Configuration
- Xming setup instructions
- Display environment variables
- Cross-platform support (Windows, Linux, Mac)
- GUI rendering through X11 forwarding
#### Documentation
- Docker deployment guide
- Xming setup guide
- Troubleshooting guide
- Final project demo video
- Performance testing results
### 📚 Documentation Requirements
- 📖 Developer guide with setup instructions
- 📖 User guide with screenshots
