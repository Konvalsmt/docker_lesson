Створені Dockerfile1 та Dockerfile2.

1. Dockerfile1

FROM alpine:latest
MAINTAINER S.Konoval <konsmt@gmail.com>
RUN apk update \
	&& apk add bash  git fortune 
RUN cd //	
RUN mkdir /data
RUN chmod 7777 /data
RUN cd /data
WORKDIR /data
ENTRYPOINT ["git", "clone"] 
CMD ["https://github.com/desktop/dugite.git"]

На основі Alpine Linux створeно Dockerfile1 для image, який при запуску буде стягувати вказаний git репозиторій.
Файли мають зберігатись використовуючи docker volume.
docker volume create my-volume
docker run --mount source=my-volume,target=/data my-git-image

docker pull konsmt/lesson3-doc1:v1

2. Dockerfile2

FROM nginx:alpine
MAINTAINER S.Konoval <konsmt@gmail.com>
RUN apk update \
	&& apk add bash 
RUN cd //	
RUN mkdir /data
RUN chmod 7777 /data
RUN cd /data
#RUN rm -rf /usr/share/nginx/html/*
RUN echo "#!/bin/bash "> /data/runsh.sh
RUN chmod +x /data/runsh.sh
RUN echo "echo 'My name is' "'$NAME'" ' and I am ' "'$AGE'" 'years old.'>> /usr/share/nginx/html/index.html ">> /data/runsh.sh
RUN echo "nginx -g 'daemon off;' ">>/data/runsh.sh
ENTRYPOINT ["/bin/bash","-c","/data/runsh.sh"]

На основі nginx створeно Dockerfile2 для побудови image, 
який буде використовувати environment variables для створення тексту в index.html файлі.
docker run -d -e NAME=James -e AGE=27 my-custom-nginx
index.html: My name is James and I am 27 years old.

docker pull konsmt/lesson3-doc:v2

