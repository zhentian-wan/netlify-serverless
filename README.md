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
