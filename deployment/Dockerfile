## 리액트 빌드
FROM node:alpine as builder

WORKDIR /app

# install app dependencies
COPY package.json ./
COPY package-lock.json ./

# Fix permissions for packages
# RUN npm config set unsafe-perm true
RUN npm install && npm install -g serve

# Bundle app source
COPY . ./

RUN chown -R node:node /app/node_modules && npm run build

##
## 리액트 빌드 결과를 nginx에 배포한다.
##
FROM nginx:latest
#
## 위에서 생성한 앱의 빌드 산출물을 nginx의 샘플 앱이 사용하던 폴더로 복사
COPY --from=builder /app/build /usr/share/nginx/html
#
## default.conf 복사: project root directory에 미리 default.conf.template파일 있어야 함
COPY ./deployment/default.conf.template /etc/nginx/conf.d/default.conf
#
## 80포트 오픈하고 nginx 실행
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]