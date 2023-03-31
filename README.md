# Fly WordPress

Deploying WordPress to [Fly.io](https://fly.io/).

_Note that this has only been tested with Fly Apps V2_

## Installation

Follow along to create a MySQL database in Fly and then install WordPress.

First you might want to replace occurrences of `ckreiling` with your own unique identifier,
including in the commands below.

### MySQL

Generate a couple of passwords to use for the WordPress and root users. Feel free to build the image in the pwgen directory or run in an existing app on fly.io with:

```bash
cd pwgen && fly machine run . --restart no -a my-misc-app && fly logs -a my-misc-app
```

Then run the following script in the `mysql/` directory:

```bash
fly apps create ckreiling-blog-mysql
fly volumes create ckreiling_mysql_wp_data --size 10 # gb
fly secrets set MYSQL_PASSWORD=password MYSQL_ROOT_PASSWORD=password # replace passwords
flyctl deploy # deploy MySQL
fly machine update --select --memory 512 # scale memory
```

### WordPress

From the `wordpress/` directory, run the following:

```bash
fly apps create ckreiling-blog-wordpress
fly volumes create ckreiling_wp_data --size 10
fly secrets set WORDPRESS_DB_PASSWORD=password # replace with non-root password
flyctl deploy # deploy WordPress
fly machine update --select --memory 256 # scale memory
```

After the rollout you can navigate to e.g. ckreiling-blog-wordpress.fly.dev and
WordPress will walk you through the installation!

#### Custom Domain

To setup a custom domain with fly is easy. First follow these
short instructions to setup DNS and certificates: https://fly.io/blog/how-to-custom-domains-with-fly/

Next navigate to WordPress settings (`wp-admin/options-general.php`) and set the "Site Address" to your
domain.

You should also update the WP_SITEURL setting in
the `[env]` section of the WordPress `fly.toml` and run
`fly deploy`.
