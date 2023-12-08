# Docker-Stack-Nginx-MariaDB
 A Docker stack that has Nginx Proxy Manager and MariaDB.<br>
 The docker-compose file is a modified example from [jc21](https://github.com/NginxProxyManager/nginx-proxy-manager)
<br>
<br>
 <h2>Changes</h2>
 
 - A seprated network for the database `backend` so no other container has access to the nginx database.
 - `frontend` network for the container to be exposed to internet for proper proxy. 
 - `container_name` is labeled as `nginx-app` you can choose to rename for your deploy.