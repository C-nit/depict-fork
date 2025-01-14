FROM maven:3.5-jdk-8-alpine as builder
WORKDIR /usr/src/app
COPY cdkdepict-lib cdkdepict-lib
COPY cdkdepict-webapp cdkdepict-webapp
COPY pom.xml pom.xml
RUN mvn clean package

FROM openjdk:8u212-jre-alpine as runtime

# we need a font! DejaVu does fine but we only want the Sans-Serif one
RUN apk add --no-cache ttf-dejavu && \
    rm /usr/share/fonts/ttf-dejavu/DejaVuSerif* \
    /usr/share/fonts/ttf-dejavu/DejaVuLGC* \
    /usr/share/fonts/ttf-dejavu/DejaVuSansCond* \
    /usr/share/fonts/ttf-dejavu/DejaVuSansMono* \
    /usr/share/fonts/ttf-dejavu/DejaVuSans-* \
    /usr/share/fonts/ttf-dejavu/DejaVuMath*
WORKDIR /usr/src/app
COPY --from=builder /usr/src/app/cdkdepict-webapp/target/cdkdepict-*.war cdkdepict.war
RUN chgrp -R 0 /usr/src/app && chmod -R g=u /usr/src/app
EXPOSE 8080
ENTRYPOINT ["java",  "-Dserver.port=8080", "-jar", "cdkdepict.war"]
