# Strapi Upload Provider for Supabase storage

- This provider is a fork of [shorwood's](https://github.com/shorwood) [strapi upload provider digitalocean](https://github.com/shorwood/strapi-provider-upload-do) for Digital Ocean spaces, but applied to [Supabase storage](https://supabase.io/)

## Parameters

- **apiUrl** : [Supabase API Url](https://supabase.io/docs/reference/javascript/initializing)
- **apiKey** : [Supabase API Key](https://supabase.io/docs/reference/javascript/initializing)
- **bucket** : [Supabase storage bucket](https://supabase.io/docs/guides/storage)
- **directory** : [Directory inside Supabase storage bucket](https://supabase.io/docs/guides/storage)
- **options** : [Supabase client additional options](https://supabase.io/docs/reference/javascript/initializing#with-additional-parameters)

## How to use

1. Install this package

```
npm i strapi-provider-upload-supabase-v4
```

2. Create config in `./config/plugins.js` with content

```
module.exports = ({ env }) => ({
  // ...
  upload: {
    config: {
      provider: "strapi-provider-upload-supabase-v4",
      providerOptions: {
        apiUrl: env("SUPABASE_API_URL"),
        apiKey: env("SUPABASE_API_KEY"),
        bucket: env("SUPABASE_BUCKET"),
        directory: env("SUPABASE_DIRECTORY"),
        options: {},
      },
      actionOptions: {
        upload: {},
        uploadStream: {},
        delete: {},
      },
    },
  },
  // ...
});
```

3. Create `.env` and add to them

```
SUPABASE_API_URL="<Your Supabase url>"
SUPABASE_API_KEY="<Your Supabase api key>"
SUPABASE_BUCKET="strapi-uploads"
SUPABASE_DIRECTORY=""
```

4. Create middleware in `./config/middlewares.js` with content

```
module.exports = ({ env }) => [
  "strapi::errors",
  {
    name: "strapi::security",
    config: {
      contentSecurityPolicy: {
        directives: {
          "default-src": ["'self'"],
          "img-src": ["'self'", "data:", "blob:", env("SUPABASE_API_URL")],
        },
      },
    },
  },
  "strapi::cors",
  "strapi::poweredBy",
  "strapi::logger",
  "strapi::query",
  "strapi::body",
  "strapi::session",
  "strapi::favicon",
  "strapi::public",
];

```

with values obtained from this page:

> https://app.supabase.io/project/<your-project>/settings/api

Parameters `options`, `bucket` and `directory` are optional and you can omit it, they will take the values shown in the example above.

## Resources

- [MIT License](LICENSE.md)

## Links

- [Strapi website](http://strapi.io/)
- [Strapi community on Slack](http://slack.strapi.io)
- [Strapi news on Twitter](https://twitter.com/strapijs)
- [Strapi docs about upload](https://strapi.io/documentation/3.0.0-beta.x/plugins/upload.html#configuration)
- [Strapi Provider DO which inspired this plugin](https://github.com/shorwood/strapi-provider-upload-do)
