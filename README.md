Updater
=======
Simple application to download and apply OTA packages.


Server requirements
-------------------
The app sends `GET` requests to the URL defined by the `updater_server_url`
resource (or the `skylineui.updater.uri` system property) and expects as response
a JSON with the following structure:
```json
{
  "response": [
    {
      "timestamp": 1230764400,
      "filename": "ota-package.zip",
      "sha256": "5eb63bbbe01eeed093cb22bb8f5acdc3",
      "buildtype": "OFFICIAL",
      "size": 314572800,
      "download": "https://example.com/ota-package.zip",
      "version": "Reborn"
    }
  ]
}
```

The `timestamp` attribute is the build date expressed as UNIX timestamp.  
The `filename` attribute is the name of the file to be downloaded.  
The `sha256` attribute is a string that uniquely identifies the update.  
The `buildtype` attribute is the string to be compared with the `org.skylineui.build_type` property.  
The `size` attribute is the size of the update expressed in bytes.  
The `download` attribute is the URL of the file to be downloaded.  
The `version` attribute is the string to be compared with the `org.skylineui.build_version` property.  

Additional attributes are ignored.


Build with Android Studio
-------------------------
Updater needs access to the system API, therefore it can't be built only using
the public SDK. You first need to generate the libraries with all the needed
classes. The application also needs elevated privileges, so you need to sign
it with the right key to update the one in the system partition. To do this:

 - Place this directory anywhere in the Android source tree
 - Generate a keystore and keystore.properties using `gen-keystore.sh`
 - Build the dependencies running `make UpdaterStudio` from the root of the
   Android source tree. This command will add the needed libraries in
   `system_libraries/`.

You need to do the above once, unless Android Studio can't find some symbol.
In this case, rebuild the system libraries with `make UpdaterStudio`.
