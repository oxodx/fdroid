# Oxod F-Droid Repository

This is the single F-Droid-compatible repository for Oxod Android apps. APKs
live in `repo/`; GitHub Actions signs the repository index and deploys it with
GitHub Pages.

## Repository URL

Until a custom domain is configured, clients use:

```text
https://oxodx.github.io/fdroid/repo
```

To use a Cloudflare hostname, first set that hostname as the repository's
custom domain in **GitHub → Settings → Pages**. Point the Cloudflare DNS record
at `oxodx.github.io` and keep it **DNS only** while GitHub validates the
domain. Then set the repository variable `FDROID_REPO_URL` to
`https://your-domain.example/repo` so generated F-Droid metadata advertises
the custom URL.

## Initial GitHub configuration

1. Create the GitHub repository `oxodx/fdroid` and push this local repository.
2. In **Settings → Pages**, choose **GitHub Actions** as the source.
3. Generate an Android keystore dedicated to this F-Droid repository; keep it
   stable, because clients use it to verify repository updates.
4. Add these repository secrets: `FDROID_KEYSTORE_BASE64`,
   `FDROID_KEYSTORE_PASSWORD`, `FDROID_KEY_ALIAS`, and
   `FDROID_KEY_PASSWORD`.
5. Run **Publish F-Droid repository** once to create the initial index.

## Add an app release

Build and sign the APK in its app repository, then add it here using this
filename format:

```text
repo/<application-id>_<version-code>.apk
```

For Magister, that becomes:

```text
repo/nl.oxod.magister_1000000001.apk
```

Commit and push the APK. The Pages workflow rebuilds the shared repository
index, so every app remains available under the same URL.

Each app's release workflow can automate this by cloning `oxodx/fdroid`,
copying its signed APK into `repo/`, and pushing with a fine-grained GitHub
token that has **Contents: Read and write** access only to `oxodx/fdroid`.

For Magister, save that token as the `FDROID_REPOSITORY_TOKEN` secret in the
`oxodx/magister` repository. Its release workflow publishes each signed APK to
this repository automatically.
