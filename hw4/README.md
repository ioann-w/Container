*Задание: необходимо создать Dockerfile, основанный на любом образе (вы в праве выбрать самостоятельно). В него необходимо поместить приложение, написанное на любом известном вам языке программирования (Python, Java, C, С#, C++). При запуске контейнера должно запускаться самостоятельно написанное приложение.*

Структура проекта:

![](/hw4/img/1.png)

Содержимое файла `Dockerfile`

```
FROM node
WORKDIR /app
COPY package.json /app
RUN npm install
COPY . .
ENV PORT 3000
EXPOSE $PORT
VOLUME [ "/app/data" ]
CMD ["node", "app.js"]
```

Содержимое файла `.dockerignore`

```
node_modules
Dockerfile
.git
.idea
```
Файл с приложением находится в `app.js`. Его будем запускать.

### Запустим контейнер ###
1. Построим образ с именем `logsapp:volumes` командой: 
```
docker build -t logsapp:volumes .
```
2. Запустим контейнер на основе этого образа командой:
```
docker run -d -p 3000:3000 -v logs:/app/data --rm --name logsapp logsapp:volumes
```

## Результат

По адресу `localhost:3000` находится наше запущенное приложение.

![](/hw4/img/2.png)
