FROM alpine:latest AS builder
WORKDIR /usr/local/src/
RUN apk add git
RUN git clone https://github.com/iganari/package-html-css.git


FROM nginx:1.25.1-alpine
WORKDIR /usr/share/nginx/
RUN mv html html.bk
WORKDIR /root/
COPY --from=builder /usr/local/src/package-html-css/200-03 /usr/share/nginx/html
