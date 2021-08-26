FROM bitnami/java:1.8.302-debian-10-r32

#MAINTANER Your Name "anand.shivam44@yahoo.com"

RUN apt-get update
WORKDIR /app

# RUN 

COPY . /app

# EXPOSE 8081

# ENTRYPOINT [ "python3" ]

CMD [ "java", "-jar", "json-data-generator-1.4.1.jar", "purchases-config.json" ]