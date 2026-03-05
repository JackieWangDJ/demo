# micro-services/Dockerfile

# ---------- build stage ----------
FROM maven:3.9.9-eclipse-temurin-17 AS builder
WORKDIR /app

# 先拷贝 pom，提高缓存命中
COPY pom.xml ./
RUN mvn -q -e -DskipTests dependency:go-offline

# 再拷贝源码并打包
COPY src ./src
RUN mvn -q -DskipTests package

# ---------- runtime stage ----------
FROM eclipse-temurin:17-jre
WORKDIR /app

# 复制 jar（兼容 jar 名不固定）
COPY --from=builder /app/target/*.jar /app/app.jar

EXPOSE 8080
ENV TZ=Asia/Shanghai

ENTRYPOINT ["java","-jar","/app/app.jar"]