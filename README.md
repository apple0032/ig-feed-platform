# Instagram Ranked Feed Platform
- This is the frontend portal of the elastic-agent API, to display IG data that stored in elastic cloud service.
- Developed with Vue.js 
- Please start the backend service first , ref : https://github.com/apple0032/elastic-agent
- `http://localhost:3000` is the defalut host name & port for the backend service
- After starting the backend service, try `http://localhost:3000/test` to test the service is working

### NOTES
- Please make sure that port 3000 & 8080 is not listening in local machine, otherwise will need to config the listening port manually in the config file. 
- Those Instagram media source come from Instagram graph API and the media is only a url that stored in Instagram server which is only accessible temporarily 
- We have two solutions : get Instagram media in real-time OR cronjob script to retrieve data

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
