# Docker-Stack-Nginx-MariaDB
 A Docker stack that has Nginx Proxy Manager and MariaDB.<br>
 The docker-compose file is a modified example from [jc21](https://github.com/NginxProxyManager/nginx-proxy-manager)
<br><br>

<h2>Changes</h2>
 
 - A seprated network for the database `backend` so no other container has access to the nginx database.
 - `frontend` network for the container to be exposed to internet for proper proxy. 
 - `container_name` is added, `nginix-proxy-manger` labelled as `nginx-app` and `mariadb-aria` is labelled as `nginx-db`, you can choose to rename for your deploy.
 <br><br>

## Deployment
1. A) You can clone repo<br>
 B)  Download the `docker-compose.yml`<br>
 C) Copy the following into your own `docker-compose.yml`<br>


```yaml
version: '3'

networks:
  frontend:
    external: true
  backend:

services:
  npm-app:
    image: jc21/nginx-proxy-manager:latest
    container_name: nginx-app
    depends_on: 
      - npm-db
    restart: always
    ports:
      - "80:80"
      - "81:81"
      - "443:443"
    environment:
      - DB_MYSQL_HOST=npm-db
      - DB_MYSQL_PORT=3306
      - DB_MYSQL_USER=npm
      - DB_MYSQL_PASSWORD=MYSQLPASSWORD
      - DB_MYSQL_NAME=npm
    volumes:
      - ./data:/data
      - ./ssl:/etc/letsencrypt
    networks:
      - frontend
      - backend

  npm-db:
    image: jc21/mariadb-aria:latest
    restart: always
    container_name: nginx-db
    environment:
      - MYSQL_ROOT_PASSWORD=ROOTMYSQLPASSWORD
      - MYSQL_DATABASE=npm
      - MYSQL_USER=npm
      - MYSQL_PASSWORD=MYSQLPASSWORD
    volumes:
      - ./db:/var/lib/mysql
    networks:
      - backend
```

2. Then you have to make the external network `frontend` with the following command in terminal:
```bash
docker network create -d bridge frontend
```
3. Run `docker compose up -d` in the directory of the docker compose with terminial

4. Connect to your instance by connecting to the port `81` for admin controls. `127.0.0.1:81`
5. the default login is:
```
Email:      admin@example.com
Password:   changeme
```
- You will be asked to change the login credentials right after logging in for the 1st time.


## Notes 
- Change the `DB_MYSQL_PASSWORD=` to a secure password
- Change the `MYSQL_ROOT_PASSWORD=` to a secure password
- Create the external network `frontend` before deployment otherwise it will not find the network and docker will error out.
- For more documentation go to the [Nginx Proxy Manager Docs](https://nginxproxymanager.com/guide/)