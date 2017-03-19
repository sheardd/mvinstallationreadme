------------------------------------------------------------------------------

Installation Guide For WP Engine's Mercury Environment For Vagrant

------------------------------------------------------------------------------

Installing the Mercury Environment

------------------------------------------------------------------------------

The repository for the Mercury Environment can be viewed at
https://github.com/wpengine/hgv

1) Install all prerequisites:
      
	   - Git (https://git-scm.com/download/)
	      
	   - VMWare (hhttp://www.vmware.com/products/personal-desktop-virtualization.html)
	     or 
	     VirtualBox (http://virtualbox.org)

	   - Vagrant (https://www.vagrantup.com/downloads.html)

	   - Node (https://nodejs.org/en/)

	   - Optional: Install Vagrant Ghost Plugin
	     (https://github.com/10up/vagrant-ghost)
	     (`vagrant plugin install vagrant-ghost`)

   It is worth noting that the Vagrant box used in this installation uses a
   64 bit operating system, and as such it is highly recommended that it
   should be run on a 64 bit machine running a 64 bit operating system of its
   own.


2) Clone the hgv repo:
    `git clone --recursive https://github.com/wpengine/hgv.git`


3) cd into the hgv directory and run `npm install` to install, build and
   deploy script dependencies.


4) Run `vagrant up`

   NOTE: You may need to install vagrant-vbguest (`vagrant plugin install
   vagrant-vbguest`) if you see the message `The guest additions on this VM do
   not match the installed version of VirtualBox`, and then
   reload your vm with `vagrant reload`. This will reconcile version
   disparities between Virtualbox and its guest additions.


5) Now that you have created your virtual environment, you will have access to
   two pre-loaded WordPress installations. Vagrant Ghost should have added
   all of your vm's hosted sites to your host machine's /etc/hosts file, but
   if it fails to do so, copy and paste the following to the end your hosts
   file on your host machine (/etc/hosts):

   192.168.150.20 hgv.test
   192.168.150.20 admin.hgv.test
   192.168.150.20 hhvm.hgv.test
   192.168.150.20 php.hgv.test
   192.168.150.20 cache.hhvm.hgv.test
   192.168.150.20 cache.php.hgv.test
   192.168.150.20 xhprof.hgv.test

   Now visit http://hgv.test/, and you should see the Mercury Vagrant
   documentation page.

   Getting this working is vital for getting access to the documentation for the
   environment.


------------------------------------------------------------------------------

Importing A Website Into the Virtual Environment

------------------------------------------------------------------------------

These instructions assume that you already have a fully-provisioned website
repository set up on a repository hosting service, such as Github.

Run the following in your vm; if your repository name is over twelve
characters long, you will need to shorten it:

`cp /vagrant/provisioning/default-install.yml /vagrant/hgv_data/sites/<REPO-NAME>.yml`

`sudo nano /vagrant/hgv_data/sites/<REPO-NAME>.yml`

Change the content of <REPO-NAME>.yml to the following:

      wp:
        enviro: <REPO-NAME>
        hhvm_domains:
          - <SITE-NAME>.test
          - www.<SITE-NAME>.test
        php_domains:
          - php.<SITE-NAME>.test

Save and reprovision your vm (`vagrant provision`) for the changes to take
effect.

Now add the URLs from your yml file to /etc/hosts on your host machine, for
example:

192.168.150.20 <SITE>.test
192.168.150.20 www.<SITE>.test
192.168.150.20 php.<SITE>.test

You should now be able to visit your URLs in the browser and see a generic
WordPress site. When running `vagrant provision` the vm checks for yml files
in the sites directory, and creates a fresh wordpress installation for each
one, including database setup. To swap this installation for your own site,
simply delete the newly-minted wordpress installation in the sites directory
(named according to the <REPO-NAME> in your yml file), and clone your repo
in its place. Remember that the repo's name must match the one used in your
yml file.

When the repo has finished downloading, you should be able to visit your URLs
and see the first step in the setup of WordPress (language selection).

------------------------------------------------------------------------------

Import Site Data From A Database

------------------------------------------------------------------------------

1) Run the following commands inside your site's directory on the vm:
      
      `mkdir tmp`
      
      `sudo apt-get install lftp'
      
      `sudo nano locals.mk`


2) Add the following to locals.mk:

      `ftpuser = <add_your_user_here>`
      
      `ftppass = <add_your_password_here>`


3) Copy wp-config-sample.php to wp-config.php, then add the following:

      define( 'DEVSITE', true );
      define( 'INTERCEPT_EMAIL', '<EMAIL>' );

      define( 'WP_SITEURL', '<URL>' );
      define( 'WP_HOME', '<URL>' );
      define( 'COOKIE_DOMAINS', '<URL>' );

      define( 'AWS_ACCESS_KEY_ID', '<AWS_ID>' );
      define( 'AWS_SECRET_ACCESS_KEY', '<AWS_KEY>' );

      define( 'WP_TEMP_DIR', '/vagrant/hgv_data/sites/<REPO>/tmp' );
      define( 'ENVATO_USER_NAME', '<ENV_USER>' );
      define( 'ENVATO_API_KEY', '<ENV_KEY>' );

   You can use any one of the URLs you added to hosts for `<WP_SITEURL>`,
   `<WP_HOME>`, and `<COOKIE_DOMAINS>`. Replace all other placeholders with your
   own information.


4) Update `DB_NAME`, `DB_USER`, and `DB_PASSWORD` in wp-config.php to suit the local
install. `DB_NAME` and `DB_USER` should both be set to `wpe_<REPO>`, replacing `<REPO>`
with the name of your website's repository. By default `DB_PASSWORD` will be
'wordpress'.


5) Run `make fullsync`.


6) You website's site data should now be successfully imported. The final step
   is to log in to your site's database and update the URL:

   a) Sign into mysql as the root user (`mysql -u root -p`). By default
      the root user for mysql has no password, so leave this blank and hit
      return.

   b) `use wpe_<REPO>;`

   c) `UPDATE wp_options SET option_value='<URL>' WHERE option_name='siteurl';`

      Replace `<URL>` with the same URL you used in wp-config.php to set
      `WP_SITEURL` and `WP_HOME`.

   d) `sudo service nginx restart`

   e) Run `vagrant provision` to update the changes made.


OPTIONAL: Your local site should now be fully up and running, but if you
wish to gain access to the admin areas and do not have an administrator
account on the live site when you run 'make fullsync', you will need to
follow the instructions below under 'Adding A New Site Administrator'. Do
not attempt to create a new user using the conventional sign-up process, as
this process will not complete properly in a virtual environment.

------------------------------------------------------------------------------

Adding A New Site Administrator

------------------------------------------------------------------------------

If you do not have an administrator account on the live site you are
presumably duplicating, you will need to create a new administrator account on
the local database. Because the site's email connectivity is not configured to
work on a local machine, you will not be able to create a user through the
conventional sign-up form, as sign-up emails will not be sent by the server.
Instead, you will have to access the server's database and add the
administrator manually.

If you have already created a user, delete this user
first, remembering to also delete the `wp_capabilities` row from that
corresponds to your `user_id` in `wp_meta` (`DELETE FROM wp_usermeta WHERE
user_id='<ID>' AND meta_value='a:1:{s:10:"subscriber";b:1;}';`)

   1) `mysql -u root -p` (leave the password blank)

   2) `use wpe_<REPO>;`

   3) `SELECT ID from wp_users ORDER BY ID DESC LIMIT 1;`
      Make a note of the ID number returned.

   4) Enter the following commands, replacing `<ID>` with the value you just
   made a note of plus one, and all other placeholders with your own
   information:

      `INSERT INTO wp_users(ID, user_login, user_pass, user_nicename,
         user_email, user_status, display_name) VALUES ('<ID>', '<USERNAME>',
         MD5('<PASSWORD>'), '<USERNAME>', '<EMAIL>',
         '0', '<NAME>');`

      `INSERT INTO wp_usermeta(umeta_id,
         user_id, meta_key, meta_value) VALUES (NULL, '<ID>',
         'wp_capabilities', 'a:1:{s:13:"administrator";b:1;}');`

      `INSERT INTO wp_usermeta(umeta_id, user_id, meta_key,
         meta_value) VALUES (NULL, '<ID>', 'wp_user_level', '10');`

You should now be able to log in with the information you just added to the
database.

When you log into the admin area, you may be prompted to update your password,
as the server believes you are using an automatically-generated password
following conventional registration. This can be safely ignored.
