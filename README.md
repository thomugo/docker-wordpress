# About this Repo

James Swineson's WordPress variant. Suitable if you:
* use a low-end Docker public cloud service
* is migrating an old WordPress installation to Docker
* need to put the whole WordPress installation dir into a volume
* and still needs auto upgrading

Runtime:
* Apache 2
* PHP 7

Preinstalled:
* WordPress
* wp-cli
* some useful Apache modules
* some useful PHP modules

## Notes
Please mount `/var/www/html` as a volume.

## Environment Variables
* `WORDPRESS_ROOT`: the path you want to install WordPress (if not exist) to. Defaults and relative to `/var/www/html`.
* `WORDPRESS_DB_HOST`, `WORDPRESS_DB_USER`, `WORDPRESS_DB_PASSWORD`: MySQL things.
* `WORDPRESS_FIX_PERMISSION`: set to chmod everything to a correct permission. Needed if you are migrating or some directory is not able to be written by WordPress. May take >6hrs on a large site. Don't set it on every container start.
* `WORDPRESS_NO_INSTALLATION`: explicitly skip WordPress detection and auto-installation. Useful if you want a clean LAMP environment.
* `WORDPRESS_UPDATE`: set to upgrade WordPress and all plugins on container start.
* `WORDPRESS_BEHIND_REVERSE_PROXY`: Set to use Apache2's mod_remoteip to parse correct ip if behind a reverse proxy. Also set `WORDPRESS_REVERSE_PROXY_HEADER` to your reverse proxy's IP header (defaults to `X-Forwarded-Proto`) and `WORDPRESS_REVERSE_PROXY_ADDR` to your reverse proxy's IP address (or CIDR or domain).

## FAQ
### Behind HTTPS proxy
You may run into problems like WordPress loading all assets over HTTP or redirect loop in `/wp-login.php`. To solve this, add the following code to the top of `/wp-config.php` right below `<?php` tag:
```php
// enable HTTPS detection behind proxy
define('FORCE_SSL_ADMIN', true);
// in some setups HTTP_X_FORWARDED_PROTO might contain
// a comma-separated list e.g. http,https
// so check for https existence
if (strpos($_SERVER['HTTP_X_FORWARDED_PROTO'], 'https') !== false)
       $_SERVER['HTTPS']='on';
```
See [Administration Over SSL](https://codex.wordpress.org/Administration_Over_SSL) for more information on this.

### Migrating from Nginx installation
If you cannot visit any static links (they should be 404), please add a `.htaccess` file to the sote root using content from [htaccess](https://codex.wordpress.org/htaccess).

If you use Wordfence WAF or any other security plugin, you may need to redo setup of them. 

Everything else should work. 

==============

This is the Git repo of the Docker [official image](https://docs.docker.com/docker-hub/official_repos/) for [wordpress](https://registry.hub.docker.com/_/wordpress/). See [the Docker Hub page](https://registry.hub.docker.com/_/wordpress/) for the full readme on how to use this Docker image and for information regarding contributing and issues.

The full readme is generated over in [docker-library/docs](https://github.com/docker-library/docs), specifically in [docker-library/docs/wordpress](https://github.com/docker-library/docs/tree/master/wordpress).

See a change merged here that doesn't show up on the Docker Hub yet? Check [the "library/wordpress" manifest file in the docker-library/official-images repo](https://github.com/docker-library/official-images/blob/master/library/wordpress), especially [PRs with the "library/wordpress" label on that repo](https://github.com/docker-library/official-images/labels/library%2Fwordpress). For more information about the official images process, see the [docker-library/official-images readme](https://github.com/docker-library/official-images/blob/master/README.md).

[![Travis CI](https://img.shields.io/travis/docker-library/wordpress/master.svg)](https://travis-ci.org/docker-library/wordpress/branches)

<!-- THIS FILE IS GENERATED BY https://github.com/docker-library/docs/blob/master/generate-repo-stub-readme.sh -->
