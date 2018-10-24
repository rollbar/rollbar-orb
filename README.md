# rollbar-orb
[CircleCI Orb](https://github.com/CircleCI-Public/config-preview-sdk/tree/master/docs) for reporting deploys and uploading sourcemaps to Rollbar.

## Requirements
If you don't have them already, create accounts in [Rollbar](https://rollbar.com/signup) and [CircleCI](https://circleci.com/signup/).

In order to use the Rollbar orb, your CircleCI builds need to run in environment where [curl](https://curl.haxx.se/) and [jq](https://stedolan.github.io/jq/) are installed.

## Usage

To use the Rollbar orb, reference it in your project and then use one of the included commands:
* `notify_deploy` 
* `notify_deploy_started`
* `notify_deploy_finished`
* `upload_sourcemap` 

Documentation for each method is available inline in [orb.yml](https://github.com/rollbar/rollbar-orb/blob/master/src/rollbar/orb.yml).

Here's a very simple example of a CircleCI config that uses the Rollbar orb to report a deploy starting and finishing as well as uploading sourcemaps:

```yaml
version: 2.1
orbs:
    rollbar: rollbar/deploy@volatile
jobs:
  build:
    docker:
      - image: circleci/ruby:latest
    environment:
      ROLLBAR_ACCESS_TOKEN: {{ your `POST_SERVER_ITEM` access token here}}
      ROLLBAR_ENVIRONMENT: development
    steps:
      - checkout
      - rollbar/notify_deploy_started:
          environment: $ROLLBAR_ENVIRONMENT
      - rollbar/upload_sourcemap:
          minified_url: https://rollbar.com/rollbar.min.js
          source_map: static/js/rollbar.js.map
          js_files:  static/js/rollbar.js
     
     # The rest of your build steps go here...

     # Notify Rollbar when your deploy has completed successfully
      - rollbar/notify_deploy_finished:
          deploy_id: $ROLLBAR_DEPLOY_ID
          status: succeeded
```

## Help / Support

If you run into any issues, please email us at [support@rollbar.com](mailto:support@rollbar.com)

For bug reports, please [open an issue on GitHub](https://github.com/rollbar/rollbar-orb/issues/new).


## Contributing

1. [Fork it](https://github.com/rollbar/rollbar-orb)
2. Create your feature branch (```git checkout -b my-new-feature```).
3. Commit your changes (```git commit -am 'Added some feature'```)
4. Push to the branch (```git push origin my-new-feature```)
5. Create a new Pull Request and submit it for review.

