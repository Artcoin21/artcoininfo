# How to Deploy tachacoininfo

tachacoininfo is splitted into 3 repos:
* [https://github.com/tachacoinproject/tachacoininfo](https://github.com/tachacoinproject/tachacoininfo)
* [https://github.com/tachacoinproject/tachacoininfo-api](https://github.com/tachacoinproject/tachacoininfo-api)
* [https://github.com/tachacoinproject/tachacoininfo-ui](https://github.com/tachacoinproject/tachacoininfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy tachacoin core
1. `git clone --recursive https://github.com/tachacoinproject/tachacoin.git --branch=tachacoininfo`
2. Follow the instructions of [https://github.com/tachacoinproject/tachacoin/blob/master/README.md#building-tachacoin-core](https://github.com/tachacoinproject/tachacoin/blob/master/README.md#building-tachacoin-core) to build tachacoin
3. Run `tachacoind` with `-logevents=1` enabled

## Deploy tachacoininfo
1. `git clone https://github.com/tachacoinproject/tachacoininfo.git`
2. `cd tachacoininfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `tachacoininfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `tachacoininfo` under a process manager (like `pm2`), to restart the process when `tachacoininfo` crashes.

## Deploy tachacoininfo-api
1. `git clone https://github.com/tachacoinproject/tachacoininfo-api.git`
2. `cd tachacoininfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy tachacoininfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/tachacoinproject/tachacoininfo-ui.git`
2. `cd tachacoininfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "TACHACOININFO_API_BASE_CLIENT=/api/ TACHACOININFO_API_BASE_SERVER=http://localhost:3001/ TACHACOININFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `tachacoininfo-ui` on port 3000
4. `npm run build`
5. `npm start`
