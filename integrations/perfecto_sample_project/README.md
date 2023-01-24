# Integrating Codemagic with Perfecto


[**Perfecto**](https://www.perfecto.io/) is a cloud-based test automation platform for web and mobile that allows application developers and QA engineers to create and execute tests across devices and browsers at scale. Perfecto offers many ways to integrate with different stages of the software development and testing lifecycle. It is possible to integrate with Perfecto directly from your **codemagic.yaml**

## Configuring Perfecto access

Signing up with [Perfecto](https://www.perfecto.io/) is required in order to get credentials that are needed during an upload process. 

1. Get the Perfecto access token from the Perfecto UI.
1. Open your Codemagic app settings, and go to the **Environment variables** tab.
2. Enter the desired **_Variable name_**, e.g. `PERFECTO_TOKEN`.
3. Copy and paste the Perfecto token string as **_Variable value_**.
4. Enter the variable group name, e.g. **_perfecto_credentials_**. Click the button to create the group.
5. Make sure the **Secure** option is selected.
6. Click the **Add** button to add the variable.

7. Add the variable group to your `codemagic.yaml` file
```yaml
  environment:
    groups:
      - perfecto_credentials
```

## Uploading to Perfecto

Using the following cURL script in a post-build script, **Release APK** and **Release IPA** binaries can be uploaded to the Perfecto platform:

```
curl "https://web.app.perfectomobile.com/repository/api/v1/artifacts" -H   scripts:
    - name: Upload to Perfecto
      script: | 
        curl "https://web.app.perfectomobile.com/repository/api/v1/artifacts" \
        -H "Perfecto-Authorization: $PERFECTO_TOKEN" \
        -H "Content-Type: multipart/form-data" \
        -F "requestPart={\"artifactLocator\":\"PRIVATE:app.aab\",\"artifactType\":\"ANDROID\",\"override\":true}" \
        -F "inputStream=@/path/to/your_binary"
```


## Test Automation

In order to automate tests, desired capabitlies can be set inside your custom made test scripts in your project. For example, if your application requires device sensors such as camera or fingerprint reader, then **sensorInstrument** needs to be set:

```dart
capabilities.setCapability("sensorInstrument", true);
```

With Appium tests **autoInstrument** capability automatically instrument the application and it needs to be set to true:

```dart
capabilities.setCapability("autoInstrument", true);
```
