# WordPress Project Seed

This is simply a seed repo for a WordPress project. Use it to jump-start your WordPress project repos, or fork it and customize it to your own liking!




## Setup

Rather than clone the repo, we're going to pull down the latest version into a new repository.

```
# create a new project folder
mkdir yourproject

# navigate to the new directory
cd yourproject

# initialise a new git repo
git init

# pull down the wp-seed project 
git pull https://github.com/edwinwright/wp-seed.git

# initialise the wp submodule
git submodule init

# clone the wp submodule into the working directory 
git submodule update
```



1. Create a database and user
- Copy /conf/local-config-sample.php to /conf/local-config.php
- Edit and add database details
- Navigate to http://www.yourdomain.com/wp/wp-admin/install.php to run through the setup process and install the db.
- Login to the admin site with your newly created user account.
- Go to 'General Settings' and change 'Site Address (URL)' from `http://www.yourdomain.com/wp` to `http://www.yourdomain.com`




## Updating Wordpress

First, deactivate all plugins.

Now update the Wordpress submodule.

```
# navigate to the wordpress submodule directory
cd src/wp

# checkout the new version by tag name
git checkout 4.3.1

# navigate back to the project root
cd ../..

# and commit the changes
git add .
git commit -m "Updated Wordpress to 4.3.1"
```

Now take a look at the wp-config-sample.php file, to see if any new settings have been introduced that you might want to add to wp-config.php.

Open the admin site in the browser, and run the Wordpress upgrade program.

Update Permalinks and .htaccess.

Reactivate plugins.





## Assumptions

* WordPress as a Git submodule in `/wp/`
* Custom content directory in `/content/` (cleaner, and also because it can't be in `/wp/`)
* `wp-config.php` in the root (because it can't be in `/wp/`)
* All writable directories are symlinked to similarly named locations under `/shared/`.

## Questions & Answers

**Q:** Will you accept pull requests?  
**A:** Maybe — if I think the change is useful. I primarily made this for my own use, and thought people might find it useful. If you want to take it in a different direction and make your own customized skeleton, then just maintain your own fork.

**Q:** Why the `/shared/` symlink stuff for uploads?  
**A:** For local development, create `/shared/` (it is ignored by Git), and have the files live there. For production, have your deploy script (Capistrano is my choice) look for symlinks pointing to `/shared/` and repoint them to some outside-the-repo location (like an NFS shared directory or something). This gives you separation between Git-managed code and uploaded files.

**Q:** What version of WordPress does this track?  
**A:** The latest stable release. Send a pull request if I fall behind.

**Q:** What's the deal with `local-config.php`?  
**A:** It is for local development, which might have different MySQL credentials or do things like enable query saving or debug mode. This file is ignored by Git, so it doesn't accidentally get checked in. If the file does not exist (which it shouldn't, in production), then WordPress will used the DB credentials defined in `wp-config.php`.

**Q:** What is `memcached.php`?  
**A:** This is for people using memcached as an object cache backend. It should be something like: `<?php return array( "server01:11211", "server02:11211" ); ?>`. Programattic generation of this file is recommended.

**Q:** Does this support WordPress in multisite mode?  
**A:** Yes, as of WordPress v3.5 which was released in December, 2012.
