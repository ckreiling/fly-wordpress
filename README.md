# Fly WordPress

Deploying WordPress to [Fly.io](https://fly.io/)

## Installation

Follow along to create a MySQL database in Fly and then install WordPress.

First you might want to replace occurrences of `ckreiling` with your own unique identifier,
including in the commands below.

### MySQL

Generate a couple of passwords to use for the WordPress and root users. I recommend
to use `pwgen`.

More or less you will want to run the following script in the `mysql/` directory:

```bash
fly volumes create ckreiling_mysql_wp_data --size 10 # gb
fly secrets set MYSQL_PASSWORD=password MYSQL_ROOT_PASSWORD=password # replace passwords
flyctl deploy # deploy the app
```

### WordPress

Run the following:

```bash
fly secrets set WORDPRESS_DB_PASSWORD=password # replace with non-root password
flyctly deploy
```

After the rollout you can navigate to e.g. ckreiling-blog-wordpress.fly.dev and
it will walk you through the installation!
