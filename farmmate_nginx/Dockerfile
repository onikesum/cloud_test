# NGINX 이미지 사용
FROM nginx:alpine

# NGINX 설정 파일을 컨테이너 내부로 복사
COPY nginx.conf /etc/nginx/nginx.conf
COPY backup_nginx.conf /etc/nginx/conf.d/default.conf

# 80번 포트를 사용하여 NGINX 서버 실행
EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
