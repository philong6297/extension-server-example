# Carbon update extension

## Preparing the extension

In the file `manifest.json` when packing the extension, field `update_url` should be added and point to the XLM file on the server in the following format:

```json
"update_url": "https://your-extension-hosting-server/manifest.xml",
```

## Setup extension hosting server

To demonstrate a server responsible for storing the updated version of the extension, an Azure Static Web App is utilized. The server should include the following files:

1. The updated extension CRX file.
2. An updates.xml file, which contains information about the updated extension. Every time the browser triggers an update, it will call to this file. The update manifest returned by the server should be an XML document that looks like this:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<gupdate xmlns='http://www.google.com/update2/response' protocol='2.0'>
  <app appid='jajphlgloihdldfhhpgoggkfjkdbbmna'>

    <!-- The version attribute represent the latest version available for the extension. -->
    <!-- The browser use this value to auto-update the extension. -->
    <updatecheck codebase='https://your-extension-hosting-server/extension.crx'
                  version='2.0' />
  </app>
</gupdate>
```

In which:

- **`appid`**: The ID of the extension.
- **`codebase`**: The URL pointing to the CRX file.
- **`version`**: The version of the updated extension.
3. Chrome has strict requirements to allow downloading an extension. One of them is that the extension file must be served with the correct MIME type. In this example, we can use the `staticwebapp.config.json` file to instruct Azure Static Web App which MIME type it must use for the `.crx` file:

```json
  "mimeTypes": {
    ".crx": "application/x-chrome-extension"
  }
```
