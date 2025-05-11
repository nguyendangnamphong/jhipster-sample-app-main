JHipster Sample Application with Prometheus and Grafana Alerting
This project is a JHipster-based application integrated with Prometheus and Grafana for monitoring and alerting. It demonstrates how to monitor HTTP request rates and send email alerts when the application is overloaded (request rate > 1 request/second). This guide helps you set up, run the application, and trigger an alert to receive an email notification.
Prerequisites
Before starting, ensure you have the following installed on your Windows machine:

Java 17: Download and install from Adoptium.
Maven 3.8+: Download from Maven and add to PATH.
Docker Desktop: Install from Docker and ensure it's running.
Git: Install from Git to clone the repository.
A Gmail account with an App Password for sending alerts:
Go to Google Account Security.
Enable 2-Step Verification.
Create an App Password (e.g., xxxx xxxx xxxx xxxx) under "App passwords".



Setup Instructions
Follow these steps to set up and run the application, Prometheus, and Grafana.
1. Clone the Repository
git clone https://github.com/nguyendangnamphong/jhipster-sample-app-main.git
cd jhipster-sample-app-main

2. Build and Run the JHipster Application

Build the application:
mvn clean install -DskipTests


Run the application:
java -jar target/jhipster-sample-application-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev


Verify the application:

Open a browser and go to: http://localhost:7070.
Test the API with:curl -H "Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTc0NzAyMzI4OCwiYXV0aCI6IlJPTEVfQURNSU4gUk9MRV9VU0VSIiwiaWF0IjoxNzQ2OTM2ODg4fQ.aKclnreJD3xJuIXBnWNc92POYGlwcARctJduxWHtQo4m3DBfW-kdGXhqGXQBrjrBx4elhSxAksOvlamFwAv7Sw" http://localhost:7070/api/users


You should see a JSON response with user data.



3. Run Prometheus and Grafana
Prometheus and Grafana are configured via Docker Compose to monitor the application and send alerts.

Start Docker Desktop:

Ensure Docker Desktop is running.


Run Docker Compose:
cd C:\path\to\jhipster-sample-app-main
docker-compose up -d


Verify Prometheus:

Open: http://localhost:9090
Go to Status > Targets and ensure jhipster-app (host.docker.internal:7070) is UP.


Verify Grafana:

Open: http://localhost:3000
Log in with:
Username: admin
Password: admin
(Skip password change if prompted).





4. Configure Grafana for Alerting

Add Prometheus Data Source:

In Grafana, go to Configuration (gear icon) > Data sources > Add data source.
Select Prometheus.
Set URL: http://prometheus:9090
Click Save & Test. Ensure it shows Data source is working.


Create Contact Point:

Go to Alerting (bell icon) > Contact points > New contact point.
Name: Gmail
Type: Email
Addresses:  phongnguyendangnam04@gmail.com
Click Save contact point > Test to verify email delivery.
Check your Gmail inbox/spam for a test email.


Create Alert Rule:

Go to Alerting > Alert rules > New alert rule.
Rule name: HighRequestRate
Query:
Data source: Prometheus
Query A: rate(http_server_requests_seconds_count{application="jhipsterSampleApplication"}[5m])
Condition: A > 1


Evaluation behavior:
Folder: JHipster Alerts
Pending period: 1m


Annotations:
Summary: High request rate detected
Description: Application is receiving more than 1 request/second for 1 minute.


Labels: severity=warning
Contact point: Gmail
Click Save and exit.



5. Overload the Application to Trigger Alert
To simulate an overload and trigger the alert, send multiple HTTP requests to the application.

Run load test:

Open Command Prompt and run:for /l %i in (1,1,1000) do curl -H "Authorization: Bearer eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTc0NzAyMzI4OCwiYXV0aCI6IlJPTEVfQURNSU4gUk9MRV9VU0VSIiwiaWF0IjoxNzQ2OTM2ODg4fQ.aKclnreJD3xJuIXBnWNc92POYGlwcARctJduxWHtQo4m3DBfW-kdGXhqGXQBrjrBx4elhSxAksOvlamFwAv7Sw" http://localhost:7070/api/users


Alternatively, target the Prometheus endpoint:for /l %i in (1,1,1000) do curl http://localhost:7070/management/prometheus




Wait 1-2 minutes:

The alert rule waits 1 minute (pending period) before triggering.


Check email:

Log in to your Gmail account (e.g., phongnguyendangnam04@gmail.com).
Check inbox or spam for an email from Grafana with subject like "Alerting: HighRequestRate".



6. Verify Alert in Grafana and Prometheus

In Grafana:

Go to Alerting > Alert rules.
Check if HighRequestRate is in Firing state.


In Prometheus:

Open: http://localhost:9090
Query:rate(http_server_requests_seconds_count{application="jhipsterSampleApplication"}[5m])


If the value is > 1, the alert has been triggered.



Troubleshooting

Cannot access http://localhost:9090:

Check if Prometheus is running:docker ps


View logs:docker logs jhipster-sample-app-main-prometheus-1


Ensure port 9090 is free:netstat -aon | findstr :9090
taskkill /PID <PID> /F




Cannot access http://localhost:3000:

Check Grafana logs:docker logs jhipster-sample-app-main-grafana-1


Change port in docker-compose.yml if 3000 is occupied:ports:
  - "3001:3000"




No email received:

Test contact point in Grafana (Alerting > Contact points > Test).
Check Gmail spam folder.
Verify App Password in docker-compose.yml.


Kubernetes containers interfere:

Stop and remove:docker stop <container-id>
docker rm <container-id>


Stop Minikube if used:minikube stop
minikube delete





Presentation Notes
To demonstrate overloading and alerting during your presentation:

Start the JHipster app, Prometheus, and Grafana (Steps 2-3).
Show Prometheus (http://localhost:9090) and Grafana (http://localhost:3000) dashboards.
Run the load test (Step 5) to simulate overload.
Wait 1-2 minutes and show the email alert in your Gmail inbox.
Display the alert status in Grafana (Alerting > Alert rules).

License
This project is licensed under the Apache License 2.0. See the LICENSE file for details.
