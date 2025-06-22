This project builds a Docker image based on `tomcat:9.0`, deploying a `.war` application along with enabling access to the **Manager** and **Host Manager** web applications. It also sets up proper user roles using a custom `tomcat-users.xml`.

---

## 📁 Project Structure
.
├── Dockerfile
├── target/
│ └── your-app.war
├── context.xml
└── tomcat-users.xml


---

## 📦 Features

- Deploys your `.war` file to Tomcat.
- Enables `manager` and `host-manager` apps.
- Bypasses security restrictions via `context.xml`.
- Configures admin users and roles via `tomcat-users.xml`.

---


---

## 📦 Features

- Deploys your `.war` file to Tomcat.
- Enables `manager` and `host-manager` apps.
- Bypasses security restrictions via `context.xml`.
- Configures admin users and roles via `tomcat-users.xml`.

---

## 🛠️ Dockerfile Breakdown

**Dockerfile**
FROM tomcat:9.0

# Copy WAR to Tomcat's webapps directory
COPY target/*.war /usr/local/tomcat/webapps/testapp.war

# Restore manager and host-manager apps from original backup
RUN cp -rf /usr/local/tomcat/webapps.dist/manager /usr/local/tomcat/webapps/manager && \
    cp -rf /usr/local/tomcat/webapps.dist/host-manager /usr/local/tomcat/webapps/host-manager

# Remove restrictive context.xml files
RUN rm -rf /usr/local/tomcat/webapps/host-manager/META-INF/context.xml && \
    rm -rf /usr/local/tomcat/webapps/manager/META-INF/context.xml

# Replace with permissive context.xml
COPY context.xml /usr/local/tomcat/webapps/host-manager/META-INF/context.xml
COPY context.xml /usr/local/tomcat/webapps/manager/META-INF/context.xml

# Add user credentials
COPY tomcat-users.xml /usr/local/tomcat/conf/tomcat-users.xml

**Next Steps**
**🛠️ Build the Docker Image**
docker build -t custom-tomcat:latest .

**🚀 Run the Container**
docker run -d -p 8080:8080 --name my-tomcat-app custom-tomcat:latest

**🔐 Accessing Manager Apps**

Visit:
App: http://localhost:8080/testapp
Manager: http://localhost:8080/manager/html
Host Manager: http://localhost:8080/host-manager/html

💡 Make sure your tomcat-users.xml includes users with roles:
manager-gui, admin-gui, etc.


