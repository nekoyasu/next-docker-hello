# 1. AlmaLinux 9 をベースイメージにする
FROM almalinux:9 AS builder

RUN dnf -y install epel-release && \
    dnf config-manager --set-enabled crb && \
    curl -fsSL https://rpm.nodesource.com/setup_18.x | bash - && \
    dnf -y install nodejs git && \
    dnf clean all
WORKDIR /app

COPY package.json package-lock.json ./

RUN npm install

COPY . .

RUN npm run build

EXPOSE 3000

CMD ["npm", "start"]
