#Google Authorize

Get an OAuth2 client with authorized token to be used with googleapis.

---

All I want for christmas is to be able to easily start working with the googleapis. There is a nice [Node quickstart](https://developers.google.com/sheets/api/quickstart/nodejs) that Google provided that allows this. This module is a wrapper of that code but instead of executing a callback function with the resultant OAuth2 client, it returns a Promise that is _thenable_ which resolves the OAuth2 client.

Here's how to use it:

Visit [Node quickstart](https://developers.google.com/sheets/api/quickstart/nodejs) and complete Step 1 to get your client_secret.json.

```bash
npm i google-authorize --save
npm i googleapis --save
```

```javascript
const GoogleAuthorize = require('google-authorize');
// Used to make requests to the googleapis
const google = require('googleapis');

// Use an array of scopes that correlate to to googleapi scopes
// i.e. ['spreadsheets'] -> https://www.googleapis.com/auth/spreadsheets
const googleAuth = new GoogleAuthorize(['spreadsheets']);

// Use the example function, listMajors, from Google
googleAuth.authorize().then(listMajors);

/**
 * Print the names and majors of students in a sample spreadsheet:
 * https://docs.google.com/spreadsheets/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74OgvE2upms/edit
 */
function listMajors(auth) {
  var sheets = google.sheets('v4');
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

```