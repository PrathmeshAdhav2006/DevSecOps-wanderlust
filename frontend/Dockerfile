FROM node:21
WORKDIR /frontend
COPY package*.json ./
RUN npm i
COPY . .
COPY .env.sample .env.local

#Run Test 
RUN npm run test


EXPOSE 3000

CMD ["npm","run","dev", "--","--host"]
