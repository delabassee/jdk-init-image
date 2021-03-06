FROM delabassee/fn-cache:latest as cache-stage

#FROM maven:3.6.2-jdk-14-alpine as build-stage
FROM openjdk:13 as build-stage
WORKDIR /function
RUN curl https://www-eu.apache.org/dist/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz -o apache-maven-3.6.2-bin.tar.gz 
RUN tar -zxvf apache-maven-3.6.2-bin.tar.gz

ENV PATH="/function/apache-maven-3.6.2/bin:${PATH}"

ENV MAVEN_OPTS -Dhttp.proxyHost= -Dhttp.proxyPort= -Dhttps.proxyHost= -Dhttps.proxyPort= -Dhttp.nonProxyHosts= -Dmaven.repo.local=/usr/share/maven/ref/repository
ADD pom.xml /function/pom.xml
ADD src /function/src
RUN ["mvn", "package"]

RUN ["mvn", "package", \
    "dependency:copy-dependencies", \
    "-DincludeScope=runtime", \
    "-Dmdep.prependGroupId=true", \
    "-DoutputDirectory=target" ]

#RUN /usr/java/openjdk-13/bin/jlink --compress=2 --no-header-files --no-man-pages --strip-java-debug-attributes --output /function/fnjre #--add-modules $(/usr/java/openjdk-13/bin/jdeps --print-module-deps /function/target/function.jar)

RUN jlink --output /function/jre-all --add-modules $(java --list-modules >&1 | sed 's/@13/,/g' | sed -e ':a' -e 'N' -e '$!ba' -e 's/\n/ /g' | sed 's/ //g')

RUN /usr/java/openjdk-13/bin/jlink --output /function/jre-0 --add-modules $(/usr/java/openjdk-13/bin/jdeps --print-module-deps /function/target/function.jar)

RUN /usr/java/openjdk-13/bin/jlink --no-header-files --no-man-pages --strip-java-debug-attributes --output /function/jre-1 --add-modules $(/usr/java/openjdk-13/bin/jdeps --print-module-deps /function/target/function.jar)

RUN /usr/java/openjdk-13/bin/jlink --no-header-files --no-man-pages --strip-java-debug-attributes --compress=1 --output /function/jre-2 --add-modules $(/usr/java/openjdk-13/bin/jdeps --print-module-deps /function/target/function.jar)


RUN /usr/java/openjdk-13/bin/jlink --no-header-files --no-man-pages --strip-java-debug-attributes --compress=2 --output /function/jre-3 --add-modules $(/usr/java/openjdk-13/bin/jdeps --print-module-deps /function/target/function.jar)



# --strip-debug --> java.io.IOException: Cannot run program "objcopy": error=2, No such file or directory

FROM oraclelinux:8-slim
WORKDIR /function

COPY --from=build-stage /function/target/*.jar /function/
COPY --from=build-stage /function/jre-all/ /jre-all/
COPY --from=build-stage /function/jre-0/ /jre-0/
COPY --from=build-stage /function/jre-1/ /jre-1/
COPY --from=build-stage /function/jre-2/ /jre-2/
COPY --from=build-stage /function/jre-3/ /jre-3/
#COPY --from=cache-stage /libfnunixsocket.so /lib

#ENTRYPOINT [ "/function/fnjre/bin/java", \
#    "--enable-preview", \
#    "-cp", "/function/*", \
#    "com.fnproject.fn.runtime.EntryPoint" ]

#CMD ["com.example.fn.HelloFunction::handleRequest"]
#CMD /bin/ash