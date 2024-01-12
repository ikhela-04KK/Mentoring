FROM nginx:latest

# RUN rm /etc/nginx/conf.d/default.conf
COPY /apps/api/ngnix/default.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE ${NGNIX_DOCKER_PORT}

# 0e8f5338dafff80ef71cca6e01c8e1901b9aa68f5d0897c9769ca825fd70077b