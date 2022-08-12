# Instagram Ranked Feed Platform
- This is the frontend portal of the elastic-agent API, to display IG data that stored in elastic cloud service.
- Please start the backend service first , ref : https://github.com/apple0032/elastic-agent
- `http://localhost:3000` is the defalut host name & port for the backend service
- After starting the backend service, try `http://localhost:3000/test` to test it is working

### NOTES
- Please make suer that port 3000 & 8080 is not listening in local machine, otherwise will need to config the listening port manually in the config file. 

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```
- Visit `http://localhost:8080` to see the platform

### Compiles and minifies for production
```
npm run build
```
