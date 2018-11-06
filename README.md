# Google Authorize

Get an OAuth2 client with an authorized token to be used with Google APIs.


All I want for Christmas is to be able to easily start working with the Google APIs. There is a nice [Node quickstart](https://developers.google.com/sheets/api/quickstart/nodejs) that Google provides for this. This module is a wrapper of that code but instead of executing a callback function that passes a resultant OAuth2 client, it returns a Promise that is _thenable_ which resolves the OAuth2 client.

Here's how to use it:

Visit [Node quickstart](https://developers.google.com/sheets/api/quickstart/nodejs) and complete Step 1 to get your client_secret.json.

```bash
npm i google-authorize --save
```

```javascript
const GoogleAuthorize = require('google-authorize');

// Use an array of scopes that correlate to to googleapi scopes
// i.e. ['spreadsheets'] -> https://www.googleapis.com/auth/spreadsheets
const googleAuth = new GoogleAuthorize(['spreadsheets']);

// Authorize and then make a request to the sheets API
googleAuth.authorize().then(listMajors);

function listMajors(auth) {
  var sheets = require('googleapis').sheets('v4');
  sheets.spreadsheets.values.get({
    auth: auth,
    spreadsheetId: '1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms',
    range: 'Class Data!A2:E',
  }, function(err, response) {
    if (err) {
      console.log('The API returned an error: ' + err);
      return;
    }
    var rows = response.values;
    if (rows.length == 0) {
      console.log('No data found.');
    } else {
      console.log('Name, Major:');
      for (var i = 0; i < rows.length; i++) {
        var row = rows[i];
        // Print columns A and E, which correspond to indices 0 and 4.
        console.log('%s, %s', row[0], row[4]);
      }
    }
  });
}

## Change Log
Nov 6 2018 - Fixed undefined callback on fs.writeFile

```
