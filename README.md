# Astro Netlify Repro

GH Issue: https://github.com/withastro/adapters/issues/185

## Versions
Astro                    v4.5.0
Node                     v21.1.0
System                   macOS (arm64)
Package Manager          npm
Output                   hybrid
Adapter                  @astrojs/netlify 5.1.3
Integrations             none

##

When using SSR (setting output to `hybrid` and having an SSR route) the build will fail on netlify during the "Deploy" stage with an error pointing to the function bundle file size when it is way below the (50mb zipped, 250mb unzipped) AWS limitation.

```
Deploy did not succeed with HTTP Error 400: [PUT /deploys/{deploy_id}/functions/{name}][400] uploadDeployFunction default  &{Code:400 Message:Failed to create function on AWS Lambda: invalid parameter for lambda creation: Unzipped size must be smaller than 262144000 bytes}
```

The error occurs when the whole deploy bundle (dist + .netlify) exceeds 250mb unzipped.

Should coder remove some files to go below 250mb, the deploy will succeed.
Should coder keep the files but disable the hybrid output and remove the SSR route, the deploy will also succeed.

PS: Error above occured with less files (PDFs) above the limit. This one occurs with more files (images) above the limit:

```
Deploy did not succeed with HTTP Error 400: [PUT /deploys/{deploy_id}/functions/{name}][400] uploadDeployFunction default  &{Code:400 Message:could not parse form file: http: request body too large}
```
