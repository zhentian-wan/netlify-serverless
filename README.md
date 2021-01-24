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
