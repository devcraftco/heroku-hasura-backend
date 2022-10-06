# Hasura for Heroku Container Stack

This repo will allow you to create a new Hasura GraphQL engine instance on Heroku's container stack. To use it, first clone this repository:

```
$ git clone git@github.com:devcraftco/heroku-hasura-backend.git
```

Next, create your Heroku app using the Heroku CLI:

```
$ heroku create <your-app-name> --stack=container
```

If successful, you should see two URLs--the first is the application URL and the second is the Heroku git remote.

Next, create and attach a Heroku Postgres instance to your app. This command uses the Hobby Dev tier, but please note that Heroku will no longer support free tiers on November 28, 2022.

```
$ heroku addons:create heroku-postgresql:hobby-dev -a <your-app-name>
```

Now we need to secure the app before starting it up:

```
$ heroku config:set -a <your-app-name> HASURA_GRAPHQL_ADMIN_SECRET=<your-admin-secret>
```

Additionally, you can disable the web console and use the local Hasura console by running:

```
$ heroku config:set -a <your-app-name> HASURA_GRAPHQL_ENABLE_CONSOLE=false
```

For production usage, it's recommended that you disable dev mode. You can do this by running:

```
$ heroku config:set -a <your-app-name> HASURA_GRAPHQL_DEV_MODE=false
```

Finally, deploy your app:

```
$ git push heroku main
```

PLEASE NOTE: The out-of-the-box configuration for this Hasura instance is insecure. The web console and dev mode are both enabled and there is no admin secret! Please do not skip the steps above otherwise your Hasura instance will be left wide open and accessible remotely.

If you disable the web console (highly recommended), you'll need to install the Hasura CLI and set up a local console. To do this, you can run:

```
$ hasura init <hasura-app-name> --endpoint <heroku-app-url> --admin-secret <your-admin-secret>
$ cd hasura-app-name
$ hasura console
```