How running the Application

Clone the Repository:
git clone https://github.com/<your-username>/jhipster-sample-app.git
cd jhipster-sample-app

Run with Maven:
./mvnw -Pdev

Windows:
mvnw.cmd -Pdev

Access the Application:

Open: http://localhost:7070/
Login: Username admin, Password admin

Optional: Run with Docker:
docker-compose up -d

Stop:
docker-compose down

Prerequisites

JDK: 17 or higher (JDK 17 recommended).
Maven: Included via mvnw.
Git: To clone the repository.
Browser: Chrome, Firefox, or similar.
Port: 7070 must be free.
Docker: Optional for containerized setup.

Troubleshooting

Port Conflict:

Check: netstat -aon | findstr :7070
Stop process: taskkill /PID <PID> /F
Or edit server.port in src/main/resources/application.yml.

Login Issues:

Verify admin user in src/main/resources/config/liquibase/data/user.csv.
Check logs: cat target/spring.log (Windows: type target\spring.log).

Build Errors:

Clean: ./mvnw clean
Debug: ./mvnw -Pdev -Ddebug=true

Contributing
Fork, create a feature branch, commit changes, and open a Pull Request. See CONTRIBUTING.md for details.
License
Apache License 2.0. See LICENSE.
Changes from Original
This repository differs from jhipster/jhipster-sample-app as follows:

Port: Changed from 8080 to 7070 in application.yml.
Spring Profile: Set spring.profiles.active: dev instead of placeholder @spring.profiles.active@.
CORS: Enabled for http://localhost:9000.
Logging: Added target/spring.log and debug levels for org.springframework.
H2 Console: Enabled at /h2-console.
JWT: Added fixed secret for authentication.
