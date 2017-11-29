FROM maven:latest

ADD pom.xml /run
WORKDIR /run
RUN ls
RUN mvn -f pom.xml dependency:copy-dependencies