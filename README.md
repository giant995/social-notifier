# social-notifier
## Prerequisites

You will need the following things properly installed on your computer.

* [Git](https://git-scm.com/)
* [Node.js](https://nodejs.org/)
* [Yarn](https://yarnpkg.com/)

In addition, new projects will also need to set up:

* A [Reddit API client](https://www.reddit.com/prefs/apps)
    * You may use the simpler "script" app type, unless you have additional reasons not to
    * Recommend you follow the "first steps" from [Reddit's quickstart](https://github.com/reddit-archive/reddit/wiki/OAuth2-Quick-Start-Example#first-steps)
    * For more, see Reddit's [API wiki](https://github.com/reddit-archive/reddit/wiki/API)
* A [Slack app](https://api.slack.com/apps)
    * You will need to configure the "Post to specific channels" (`incoming-webgook`) scope
    * You may also need to configure the "Send messages as..." (`chat:write:bot`) scope

## Installation

* `git clone <repository-url>` this repository
* `cd social-notifier`
* `yarn install`
* `cp .env.example .env`
    * Make sure to replace all sample values with the relevant credentials and config variables

## Configuration
Configuration for all social services is set up in `.yml` files in the data directory.

### Reddit
* Config file: [`data/subreddits.yml`](https://github.com/chefconnie/social-notifier/blob/master/data/subreddits.yml)
* Structure:
    ```
    subreddit_name:
      - search term 1
      - search term 2
    AskReddit:
      - plumbus
      - pan-galactic gargle blaster
    ```

### Twitter
Coming soon

## Running / Development
### Developing
* `yarn run build`
* `yarn run start` (iff already built)

### Building
* `yarn run build`

### Deploying
#### First Deploy
Unfortunately, this app will not work on Glitch, because 1) either Reddit or `snoowrap` doesn't like Glitch's runtime; and 2) Glitch will not keep your app alive in the background > 5m anyway 😢

Instead, create (or SSH into) a server, e.g. on DigitalOcean or AWS. If this is your first time creating a server, we recommend following DigitalOcean's [security + usability guidelines](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-18-04).

Then, we recommend following the [CertSimple Linux deployment guide](https://certsimple.com/blog/deploy-node-on-linux), and using the pre-written `notifier.service` file in this repo. The next section assumes you have followed this guide, and cloned the project into `/var/local/social-notifier`.

#### Subsequent Changes
* `ssh user@sub.host.com`
* `cd /var/local/social-notifier`
* `git pull`
* `yarn run build`
* `sudo systemctl restart notifier`

Since `.env` is gitignored, you will need to set production environment variables manually, e.g. by
* Configuring them natively on the server;
* Using `scp` or `rsync` to publish your `.env` file to the server separately; or
* Copying values into a server-side `.env` file manually

#### Misc
To view server logs:
```bash
sudo journalctl --follow -u notifier
```

## Planned Improvements
PRs generally welcome, especially for:

* Twitter support
* Caching last-received results on the server's YAML file
    * (currently some notifications may be duplicated on restart)
