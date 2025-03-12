FROM almalinux:9
LABEL maintainer="Soushi Arakawa <soushi@ageet.com>"


ARG NEXT_PUBLIC_API_URL
ENV NEXT_PUBLIC_API_URL=${NEXT_PUBLIC_API_URL}

RUN echo ${NEXT_PUBLIC_API_URL}

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
