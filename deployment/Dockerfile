FROM node:alpine as builder

WORKDIR /app
COPY . ./
RUN npm install && npm run build 

## 빌드 결과를 nginx에 배포한다.
FROM nginx:latest

## 위에서 생성한 앱의 빌드 산출물을 nginx의 샘플 앱이 사용하던 폴더로 복사
COPY --from=builder /app/dist /usr/share/nginx/html
## Copy web configuration 
COPY --from=builder /app/deployment/default.conf.template /etc/nginx/conf.d/default.conf
#
## 80포트 오픈하고 nginx 실행
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]