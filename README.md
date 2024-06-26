# Learn-API-Auth-Pre-Req-Script

## Description

This collection is configured to automatically fetch the bearer token for every request made within it by using Pre-Request script.

## Installation

### Postman

Postman is a collaboration platform for API development. It simplifies each step of building an API and streamlines collaboration, enabling you to create better APIs faster.

To install Postman, follow these steps:

1. Visit the [Postman website](https://www.postman.com/downloads/).
2. Download the version suitable for your platform.
3. Run the installer.

### Pre-Request Script

Import the file `Learn-API-Auth-Pre-Req-Script.postman_collection.json`. Go to the Script -> Pre-req section, you can be able to locate the code to fetch the Bearer Token.

```js
const crypto = require('crypto-js');
const tokenUrl = "https://" + pm.variables.get("domain") + "/learn/api/public/v1/oauth2/token";
const username = pm.variables.get("username");
const password = pm.variables.get("password");

// form data urlencoded
var formBody = [
    encodeURIComponent("grant_type") + '=' + encodeURIComponent("client_credentials")
];

var requestBase = {
    url: tokenUrl,
    method: 'POST',
    header: "Authorization:Basic " + crypto.enc.Base64.stringify(crypto.enc.Utf8.parse(username + ':' + password)),
    body: {
        mode: 'urlencoded',
        urlencoded: formBody
    }
};

// send request and set the token environment variable
pm.sendRequest(requestBase, function (err, response) {
    console.log(response); // Log the response to the Postman console

    if (!err) {
        // Extracting the access_token from the response
        var responseData = response.json();
        var token = responseData.access_token;

        // Set the extracted value as an environment variable
        pm.variables.set("token", token);
    }
});
```

## Usage

After importing the `Learn-API-Auth-Pre-Req-Script.postman_collection.json` file into Postman, you can use the APIs by sending requests to them. Ensure to set up the environment variables correctly:

- `$LEARN_DOMAIN`: Learn Server FQDN
- `$APP_KEY`: Your application key here
- `$APP_SECRET`: Your application secret here

