Did you ever wonder what can you write in a kuberenetes API object? What are your possibilities?

Probably yes and you searched for the kubernetes api reference page and Ctrl+F. Ah! Then you recalled some swaggerish things in OpenShift. After some googling you tried several github projects which are obsoleted today mostly because OpenShift uses swagger 1.2.

No? Maybe, that was me.

# Getting started

Environment variables:

- MASTER_API_SWAGGER_URL: Your OpenShift swagger endpoint, for me it's https://example.com:8443/swaggerapi
- KEEP_API_CALLS: `false` by default, which means the REST API paths will be removed, you can browse the object models only. Keep in mind you can't use swagger's 'Try it' feature due to the missing auth token.

`oc new-app swagger --env MASTER_API_SWAGGER_URL=https://example.com:8443/swaggerapi --env KEEP_API_CALLS=false https://github.com/janosroden/oc-swagger.git`

# How it works?

At the start of the container [api-spec-converter](https://github.com/LucyBot-Inc/api-spec-converter) converts `$MASTER_API_SWAGGER_URL` to swagger 2.0 format. Then depending on `$KEEP_API_CALLS` it removes the REST API paths from the swagger file with [jq](https://stedolan.github.io/jq/manual/v1.5/). Finally the [http-server](https://github.com/indexzero/http-server) serves the static files.

[[deployment.png|alt=deployment]]