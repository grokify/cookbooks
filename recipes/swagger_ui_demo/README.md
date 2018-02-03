Swagger UI Demo
===============

To get Swagger UI set up and test some APIs, use the following steps.

1. Clone the repo:

```bash
$ git clone https://github.com/swagger-api/swagger-ui
$ cd swagger-ui
```

2. Copy your Swagger spec to the `swagger-ui/dist` folder

```bash
$ cp /path/to/myspec.yaml dist
```

3. Edit the Redirect URL (`oauth2RedirectUrl`) in `swagger-ui-standalone-preset.js`

The Redirect URL is hardcoded to `http://localhost:3200/oauth2-redirect.html`. If you want to change this, edit the `swagger-ui/dist/swagger-ui-standalone-preset.js` file and change that URL.

Here, we will use port `8080`.

```
$ vi dist/swagger-ui-standalone-preset.js
```

4. Add the Redirect URL to your app

In your app console, configure your app to support the Swagger UI Redirect URL

5. Run a server

One way to run a static HTML server is to use `http-server`:

```bash
$ npm install -g http-server
$ http-server dist -p 8080
```

6. Bring up Swagger UI in your browser

Go to the Swagger UI URL, e.g. `http://localhost:8080`

7. Authorize

Click the `Authorize` button and enter your app and user credentials

8. Call the APIs!

You're done, try out the APIs now.