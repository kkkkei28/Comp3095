# Build Stage
# ----------
FROM gradle:8-jdk22 AS builder
COPY --chown=gradle:gradle . /home/gradle/src

WORKDIR /home/gradle/src

RUN gradle build -x test

# ---------------
# Package Stage
# ---------------
FROM openjdk:22-jdk-slim

RUN useradd -ms /bin/bash appuser

USER appuser

RUN mkdir /app

COPY --from=builder /home/gradle/src/build/libs/*jar /app/product-service.jar

ENV MONGO_DB_USERNAME=admin \
    MONGO_DB_PWD=password

# ポート公開
EXPOSE 8184

# アプリケーションのエントリーポイント
ENTRYPOINT ["java", "-jar", "/app/product-service.jar"]
