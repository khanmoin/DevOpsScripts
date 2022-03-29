# build environment
FROM node:16.14.0-alpine as build
WORKDIR /app

# add `/app/node_modules/.bin` to $PATH
ENV PATH /app/node_modules/.bin:$PATH

# for passing env variables from docker-compose
ARG REACT_APP_BASE_URL
ENV REACT_APP_BASE_URL $REACT_APP_BASE_URL

# install dependencies and build app
COPY . ./
RUN yarn
RUN yarn build

# production environment
FROM nginx:stable-alpine
COPY --from=build /app/build /usr/share/nginx/html
COPY --from=build /app/nginx/nginx.conf /etc/nginx/conf.d/default.conf

# expose port 80
EXPOSE 80

# serve the app
CMD ["nginx", "-g", "daemon off;"]
