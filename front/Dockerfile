# 1. Node.js の公式イメージを使用
FROM node:20-alpine AS builder

# 2. 作業ディレクトリの作成
WORKDIR /app

# 3. package.json と package-lock.json をコピー
COPY package.json package-lock.json ./

# 4. 依存関係をインストール
RUN npm install

# 5. ソースコードをコピー
COPY . .

# 6. Next.js をビルド
RUN npm run build

# 7. 本番環境用の軽量な Node.js イメージを使用
FROM node:20-alpine

# 8. 環境変数設定
ENV NODE_ENV=production

# 9. 作業ディレクトリを作成
WORKDIR /app

# 10. 必要なファイルをコピー
COPY --from=builder /app/package.json /app/package-lock.json ./
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public
COPY --from=builder /app/node_modules ./node_modules

# 11. サーバーを起動
CMD ["npm", "start"]
