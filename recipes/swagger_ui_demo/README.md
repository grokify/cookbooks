Swagger UI Demo
===============

To get Swagger UI set up and test some APIs, use the following steps.

1. Clone the repo:

```bash
$ git clone https://github.com/swagger-api/swagger-ui
$ cd swagger-ui/dist
```

2. Copy your Swagger spec to the `swagger-ui/dist` folder

```bash
$ cp /path/to/myspec.yaml .
```

3. Edit the Redirect URL in `swagger-ui-standalone-preset.js`

The Redirect URL parameter (`oauth2RedirectUrl`) is hardcoded to `http://localhost:3200/oauth2-redirect.html`. If you want to change this, edit the `swagger-ui/dist/swagger-ui-standalone-preset.js` file and change that URL.

Here, we will use port `8080`, so edit file, locate the URL and change it to `http://localhost:8080/oauth2-redirect.html`.

```
$ vi swagger-ui-standalone-preset.js
```

4. Add the Redirect URL to your app

In your app console, configure your app to support the Swagger UI Redirect URL, `http://localhost:8080/oauth2-redirect.html`.

5. Run a server

One way to run a static HTML server is to use `http-server`:

```bash
$ npm install -g http-server
$ http-server . -p 8080
```

6. Bring up Swagger UI in your browser

Go to the Swagger UI URL, e.g. `http://localhost:8080`

7. Load your Swagger spec

In the Swagger UI address bar, enter your spec URL, e.g. `http://localhost:8080/myspec.yaml` and click `Explore`.

8. Authorize

Click the `Authorize` button and enter your app and user credentials

9. Call the APIs!

You're done, try out the APIs now.