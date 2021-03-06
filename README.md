### Fn Init Image for JDK builds - jlink 'custom' JRE with Alpine/Portola and Oracle Linux 8 Slim

See [init-images](https://medium.com/fnproject/even-wider-language-support-in-fn-with-init-images-a7a1b3135a6e) details.

* [Java 14 EA / jlink / Alpine](jdk14-alpine) - current EA
* [Java 13 / jlink / Oracle Linux 8 Slim](jdk13) - current GA
* [Java 13 EA / jlink / Alpine](jdk13-alpine) - deprecated!
* [Java 12 EA / jlink / Alpine](jdk12-alpine) - depreacted!


`java -XshowSettings` will return 
```
  java.runtime.name = OpenJDK Runtime Environment
    java.runtime.version = 12+33
    java.specification.name = Java Platform API Specification
    java.specification.vendor = Oracle Corporation
    java.specification.version = 12
    java.vendor = Oracle Corporation
    java.vendor.url = https://java.oracle.com/
    java.vendor.url.bug = https://bugreport.java.com/bugreport/
    java.version = 12
    java.version.date = 2019-03-19
    ...
```

Push to DockerHub with `delabassee/jdk-xx-init` local

:warning: to fix!

```
docker images | grep jdk-xx-init
docker tag xyz123... delabassee/jdk-xx-init
docker push delabassee/jdk-xx-init
docker tag delabassee/jdk-xx-init jdk-xx-init
```

### Notes

* [jlink args in a file](https://github.com/brunoborges/simple-web-app/blob/master/Dockerfile.jlink)
