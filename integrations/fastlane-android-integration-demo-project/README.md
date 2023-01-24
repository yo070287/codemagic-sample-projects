# Fastlane integration Android Demo Project

**Fastlane** is an open source platform aimed at simplifying Android and iOS deployment. If your development team uses Fastlane, it can be used as part of your CI/CD pipeline to build and deploy your applications. Fastlane is preinstalled on the Codemagic build servers.

The codemagic.yaml in the root of this project contains an example of how to run a Fastlane lane as part of your Codemagic CI/CD builds. Refer to the Fastfile for an example Fastlane script that increments the app version code, builds the android app and then publish to Play store. Please refer to the Fastlane [documentation](https://docs.fastlane.tools/) for further information about configuring Fastlane.   

Documentation for YAML builds can be found at the following URL:

https://docs.codemagic.io/getting-started/yaml/

## Uploading a keystore

1. Open your Codemagic Team settings, and go to  **codemagic.yaml settings** > **Code signing identities**.
2. Open **Android keystores** tab.
3. Upload the keystore file by clicking on **Choose a file** or by dragging it into the indicated frame.
4. Enter the **Keystore password**, **Key alias** and **Key password** values as indicated.
5. Enter the keystore **Reference name**. This is a unique name used to reference the file in `codemagic.yaml`
6. Click the **Add keystore** button to add the keystore.

For each of the added keystore, its common name, issuer, and expiration date are displayed.

Default environment variables are assigned by Codemagic for the values on the build machine:

- Keystore path: `CM_KEYSTORE_PATH`
- Keystore password: `CM_KEYSTORE_PASSWORD`
- Key alias: `CM_KEY_ALIAS`
- Key alias password: `CM_KEY_PASSWORD`

## Publishing to Google Play Store

For publishing to Google Play Store you will need to [set up a service account in Google Play Console](https://docs.codemagic.io/yaml-basic-configuration/knowledge-base/google-play-api/) and save the contents of the `JSON` key file as a [secure environment variable](https://docs.codemagic.io/yaml-basic-configuration/variables/environment-variable-groups/#storing-sensitive-valuesfiles) in application or team settings under the name `GCLOUD_SERVICE_ACCOUNT_CREDENTIALS` in the `google_play` group.


Environment variables can be added in the Codemagic web app using the 'Environment variables' tab. You can then and import your variable groups into your codemagic.yaml. For example, if you named your variable group 'google_play', you would import it as follows:

```yaml
workflows:
  workflow-name:
    environment:
      groups:
        - google_play
```

For further information about using variable groups please click [here](https://docs.codemagic.io/yaml-basic-configuration/variables/environment-variable-groups/).

## Running your Fastlane lane

In the codemagic.yaml you should install your dependencies with `bundle install` and then execute the Fastlane lane with `bundle exec fastlane <lane_name>` as follows:

```yaml
  scripts:
    - bundle install
    - bundle exec fastlane release
```

If you need to use a specific version of bundler as defined in the Gemfile.lock file, you should install it with `gem install bundler:<version>` as follows:

```yaml
  scripts:
    - gem install bundler:2.2.27
    - bundle install
    - bundle exec fastlane release   
```

## Artifacts

To gather the .apk and .aab from your build, add an the **artifacts** section to your codemagic.yaml as follows:

```yaml
  artifacts:
    - app/build/outputs/**/**/*.aab
    - app/build/outputs/**/**/*.apk      
```