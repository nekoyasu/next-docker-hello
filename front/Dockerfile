# 1. AlmaLinux 9 をベースイメージにする
FROM almalinux:9 AS builder

# 2. 必要なパッケージをインストール
RUN dnf install -y \
    epel-release \
    dnf-plugins-core && \
    dnf module enable -y nodejs:20 && \
    dnf install -y nodejs npm && \
    dnf clean all

# 3. 作業ディレクトリを作成
WORKDIR /app

# 4. package.json と package-lock.json をコピー
COPY package.json package-lock.json ./

# 5. 依存関係をインストール
RUN npm install

# 6. ソースコードをコピー
COPY . .

# 7. Next.js をビルド
RUN npm run build

# 8. 本番環境用の AlmaLinux イメージ
FROM almalinux:9

# 9. Node.js をインストール
RUN dnf install -y \
    epel-release \
    dnf-plugins-core && \
    dnf module enable -y nodejs:20 && \
    dnf install -y nodejs npm && \
    dnf clean all

# 10. 環境変数設定
ENV NODE_ENV=production

# 11. 作業ディレクトリを作成
WORKDIR /app

# 12. 必要なファイルをコピー
COPY --from=builder /app/package.json /app/package-lock.json ./
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules

# 13. ポートを開放（Next.js のデフォルトポート 3000）
EXPOSE 3000

# 14. サーバーを起動
CMD ["npm", "start"]
