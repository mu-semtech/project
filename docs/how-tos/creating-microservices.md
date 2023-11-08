# Creating a microservice
Creating a microservice is simply 2 steps!

You can find templates on [semantic.works/docs](https://semantic.works/docs) or by searching [`template` on the mu-semtech GitHub page](https://github.com/mu-semtech?q=template)

*Note: the code examples provided use the mu-javascript-template. If you prefer using a different template, adapt accordingly*

## Step 1: Create a Dockerfile

```Dockerfile
# Dockerfile
FROM semtech/mu-javascript-template:1.3.5
LABEL maintainer="yourname@example.com"
```

## Step 2: Create app.*

```js
// app.js
import { app, query } from "mu";
app.get("/count", async function(req, res) {
    const resp = await query("SELECT COUNT(*) WHERE { ?s ?p ?o }");
    res.send(JSON.stringify(resp));
});
```


