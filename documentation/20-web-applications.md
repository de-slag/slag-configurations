# Web Applications

## 2-Layers Spring MVC Application running on Apache Tomcat

### Spring Boot Initializr

https://spring.io/guides/gs/serving-web-content

Create a Maven project using Spring Initializr
* https://spring.io/guides/gs/serving-web-content/#scratch

Create a Controller and a html-page
* https://spring.io/guides/gs/serving-web-content/#initial

* Make a WAR-File
  * follow https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-create-a-deployable-war-file
  * remove '/' in index.html like '\<a href="/greeting"\>' to '\<a href="greeting"\>'

## 3-Layers Spring MVC Application and MariaDB running on Apache Tomcat

### Spring Boot Initializr

https://spring.io/guides/gs/serving-web-content

Create a Maven project using Spring Initializr
* https://spring.io/guides/gs/serving-web-content/#scratch
* add "MariaDB" as dependency

Create a Controller and a html-page
* https://spring.io/guides/gs/serving-web-content/#initial

* Make a WAR-File
  * follow https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto-create-a-deployable-war-file
  * remove '/' in index.html like '\<a href="/greeting"\>' to '\<a href="greeting"\>'

Next steps:
* connect to a mariadb configured in tomcat context
