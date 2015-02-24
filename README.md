# [ReWP](https://github.com/amekusa/ReWP)
A smart way to rebuild your WordPress site into local.  

\* This is a fork of [VCCW (vagrant-chef-wordpress)](https://github.com/vccw-team/vccw).

## It’s not easy to set up a local development environment for an existing WP site
That's why I made this!

## How to use
1. Clone or download [ReWP](https://github.com/amekusa/ReWP) into local
2. Rename the folder as you like. Your site’s name is better
3. Copy the **document root** folder of your WP site into ReWP
4. Export database of your WP site into a single file (SQL or XML) and place it into ReWP as `db.sql` or `db.xml`
5. Copy `ReWP/provision/default.yml` as `ReWP/site.yml`
6. Edit the `site.yml` to be suited for your site’s specifications
    + `hostname` is the **local version** of your site’s URL.  
    `hostname_old` is the production one
    + `sync_folder` is a relative path to the document root from ReWP
    + `import_sql` must be **true** to import a SQL file
    + `import_wxr` must be **true** to import a XML file
7. In the shell, type commands as follows:

        cd /path/to/your/ReWP
        vagrant up
8. Wait for several minutes. You’ll see lots of console outputs
9. If no error, **you’ve done!** Browse the address you specified as `hostname` in the `site.yml`

## Need help?
Do not hesitate to post your problem in <https://github.com/amekusa/ReWP/issues>.

Thx :)
