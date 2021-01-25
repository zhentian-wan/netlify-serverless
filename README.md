## Set up a Local Development Environment for Serverless Functions Using Netlify

Netlify makes developing serverless functions easy with the netlify-cli (ntl for short). You'll be able to build and test functions locally as well as publish your functions from the CLI.

```bash
npm i -g netlify-cli
```

Testing:

```bash
ntl -v
```

We will install netlify-cli globally and create a netlify.toml file that will configure where the CLI should look to run functions that we define, in our case functions/. When the application is served up, Netlify runs functions under /.netlify/functions/.

netlify.toml:

```toml
[build]
    publish = "public"
    functions = "functions"

[dev]
    autoLaunch = false
```

functions/hello-world.js

```js
exports.handler = async () => {
  return {
    statusCode: 200,
    body: "Hello world!",
  };
};
```

### RUN:

```bash
ntl dev
```

### Visit

`localhost:8888/.netlify/functions/hello-world`.

## Deploy Serverless Functions to Production on Netlify using the Netlify CLI

```bash
ntl login      ## login
ntl status     ## verify
```

Netlify makes deploying to production a breeze by configuring your Netlify CLI to your Netlify account and integrating with GitHub.

We'll log in to Netlify through the CLI and initialize a site by completing all the options that the CLI runs us through. Once this is done, Netlify will trigger new builds for your site every time you push to GitHub.

```bash
ntl init  ## select "Create new site"/"No Command/public folder"
ntl open  ## open the site
```

or you can do the same thing on the netlify website.

## Circumvent CORS when Accessing a Third-Party API using Netlify Functions

CORS limits websites from communicating with other domains without the full consent of both sites. Consuming data from a 3rd Party REST API makes it difficult since we can't properly configure the appropriate CORS headers. To solve this, we'll set up a proxy server to request the data using a Netlify function that avoids CORS altogether.

The Third-Party API that we'll be working with will provide the following data: id, name, favoriteSong for each corgi.

http://no-cors-api.netlify.app/api/corgis

We will also use node-fetch a light-weight module that brings the fetch API to fetch the data into our application.

functions/load-corgis.js

```js
const fetch = require("node-fetch");
exports.handler = async () => {
  const result = await fetch(
    "http://no-cors-api.netlify.app/api/corgis"
  ).then((res) => res.json());

  return {
    statusCode: 200,
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(result),
  };
};
```

public/index.html:

```js
function loadCorgis() {
    const conrgis = await fetch('/.netlify/functions/load-corgis') // load from local functions, to avoid CORS problem
        .then(res => res.json());

    render(
    html` ${corgis.map((corgi) => html` <${Corgi} corgi=${corgi} /> `)}`,
    document.querySelector('.corgis'),
    );
}
```

## Import and Set Environment Variables from a .env file using Netlify CLI

Create a `.env` file. It is available for myself to use.

But if I want to share with the team. We need to share this `.env` file.

```bash
netlify env:import .env
```

If successfully imported: you will see:

```bash
.----------------------------.
| Imported environment variables |
|----------------------------|
|    Key     |     Value     |
|------------|---------------|
| TEST_VALUE | an test world |
'----------------------------'
```

The `.env` has been uploaded to netlify and saved there. Now even you delete the .env file, it will still be available.


