# connect-html-css-js-with-adv-java

# Setting up frontend

# Index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Frontend Example</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Frontend Example</h1>
    <button id="fetchData">Fetch Data</button>
    <div id="dataDisplay"></div>
    <script src="script.js"></script>
</body>
</html>

Styles.css

/* Basic styling */
body {
    font-family: Arial, sans-serif;
}
button {
    padding: 10px;
    margin: 20px;
}
#dataDisplay {
    margin-top: 20px;
}

# script.js

document.getElementById('fetchData').addEventListener('click', function() {
    fetch('/MyWebApp/api/data')
        .then(response => response.json())
        .then(data => {
            document.getElementById('dataDisplay').innerText = JSON.stringify(data);
        })
        .catch(error => console.error('Error fetching data:', error));
});


# Setting up java backend

MyWebApp/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── MyServlet.java
│   │   └── webapp/
│   │       ├── index.html
│   │       └── WEB-INF/
│   │           └── web.xml
│
└── pom.xml


Pom.xml

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://www.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>MyWebApp</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.3.1</version>
                <configuration>
                    <failOnMissingWebXml>false</failOnMissingWebXml>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

# MyServlet.java

package com.example;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/api/data")
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.getWriter().write("{\"message\":\"Hello from the backend!\"}");
    }
}

# Web.xml

<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
    version="3.1">
    <servlet>
        <servlet-name>MyServlet</servlet-name>
        <servlet-class>com.example.MyServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name>
        <url-pattern>/api/data</url-pattern>
    </servlet-mapping>
</web-app>


# Build and Run
Build the Project:
Use Maven to build the WAR file: mvn clean install.
Deploy the WAR File:
Deploy the generated WAR file (MyWebApp.war) to a servlet container like Apache Tomcat.
Access the Frontend:
Open a web browser and navigate to http://localhost:8080/MyWebApp

Handling CORS (Optional)
If your frontend and backend are running on different domains or ports, you need to handle CORS in your servlet.
Example CORS Filter:

package com.example;

import javax.servlet.*;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

@WebFilter("/*")
public class CORSFilter implements Filter {

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        HttpServletResponse httpServletResponse = (HttpServletResponse) response;
        httpServletResponse.setHeader("Access-Control-Allow-Origin", "*");
        httpServletResponse.setHeader("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS");
        httpServletResponse.setHeader("Access-Control-Allow-Headers", "Content-Type");
        chain.doFilter(request, response);
    }
}




