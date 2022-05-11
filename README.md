# Update App
`cd /opt/webapps/<APP_NAME>`

## Pull latest changes
`git pull`

## Update the databasee
```
cd api
source .env/bin/activate
python manage.py migrate
```

## Rebuild the App
```
cd /opt/webapps/<APP_NAME>/web
npm install
npm run build
```

## Restart supervisor
```
supervisorctl reread
supervisorctl update
```


# New Deployment
```
cd /opt/webapps
git clone <git_repo>
```

## Create virtualenv
```
cd <APP_NAME>/api
virtualenv .env
source .evn/bin/activate
```

## Update local setting

### Database
```
DATABASE = {
  'default': {
    <ENTER_INFO_FROM_DB>
  }
}
```

### Allowed Hosts
```
ALLOWED_HOSTS = ['<IP_ADDRESS>', '<DOMAIN>', ]
```

## Initialize DB
```
python manage.py migrate
python manage.py createsuperuser
```

## Daphne
```
pip install daphne
```

### Supervisor
```
TODO
```

## Build Angular app
```
npm install
npm run build
```

## Nginx
Create file `/etc/nginx/sites-enabled/<APP_NAME>`

```
upstream <APP_NAME> {
  TODO
}

server {
  listen 80;
  server_name <DOMAIN>;
  root /opt/webapps/<APP_NAME>/web/dist/<APP_NAME>;
  
  location /api {
    try_files $uri @proxy_to_app;
  }
  
  location /admin {
    try_files $uri @proxy_to_app;
  }
  
  location @proxy_to_app {
    proxy_pass http://<APP_NAME>;
    
    TODO
  }
  
  location / {
    try_files $uri $uri/ /index.html;
  }
}
```

`nginx -s reload`
