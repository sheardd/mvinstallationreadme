------------------------------------------------------------------------------

Installation Guide For WP Engine's Mercury Environment For Vagrant

------------------------------------------------------------------------------

Introduction

------------------------------------------------------------------------------

This guide is intended to help you install WP Engine's Mercury Environment For
Vagrant, and then use this environment/machine to host a website on your
machine. Vagrant uses virtual machine software to essentially create a "second
computer" inside of your existing one. In simple terms, this allows your
computer to simultaneously host and interact with a website without connecting
to the internet, negating the need to host an unfinished website online.
Virtual Machines also have a few other advantages; they allow you to install
software and make system changes within a closed environment, with no risk of
making unexpected or unwanted changes to your system as a whole. You will be
able to make any changes you want to the system within this local environment,
and when you no longer have any need for it, you can simply delete it.

In order to follow these instructions, you'll need to use the Command Line
(also known as the command prompt or terminal). To access this on a Mac,
you can search your machine using Cmd + Space and just type 'terminal'. On
Windows, there are various ways to open the Command Line, but probably the
easiest way is to either search by hitting Windows + R or using Cortana
(depending on your version of Windows), typing 'command' or 'cmd', then
selecting 'Command Prompt' from the results.

------------------------------------------------------------------------------

Installation Guide - Required Software

------------------------------------------------------------------------------

Open a Command Line window as described above. Next, install the following
software by typing in the commands given in brackets:

   - Git (https://git-scm.com/download/)
      
   - VMWare (hhttp://www.vmware.com/products/personal-desktop-virtualization.html)
     or 
     VirtualBox (http://virtualbox.org)

   - Vagrant (https://www.vagrantup.com/downloads.html)

   - Node (https://nodejs.org/en/)

   - Optional: Install Vagrant Ghost Plugin
     (https://github.com/10up/vagrant-ghost)

All prerequisistes are installed system-wide on your computer, so the
commands to install them can be typed directly into the Command Line on
opening.

It is worth noting that the Vagrant box used in this installation uses a
64 bit operating system, and as such it is highly recommended that it
should be run on a 64 bit machine running a 64 bit operating system of its
own.

------------------------------------------------------------------------------

Installation Guide

------------------------------------------------------------------------------

Make sure that all prerequisite software listed under 'Installation Guide -
Prerequisites' have been installed before running the following steps.

1) Open a Command Line window as described in the Introduction. Before
   installing the environment, make sure that your Command Line window
   is pointing to the correct folder. Failing to do this will install the
   environment in whatever directory (folder) your terminal was pointing at
   when you opened it (most likely your user's home folder on your system). If
   you are unsure what directory you are currently in, it will be listed at
   the start of every new line, before the dollar sign on mac, or '>' on windows.

   The installation commands for the environment will operate on whatever
   directory the command line window is pointing at when they are run. To
   change where the terminal is pointing, type 'cd <FILEPATH>', replacing
   <FILEPATH> with the full path to the folder you want to install the
   environment in. For example, to install the environment in the 'mercury'
   directory in the home directory of user 'john', type
   'cd /Users/john/mercury' on a Mac or 'cd c:\Users\John\Mercury' on a PC.
   
   Alternatively, if you are using a PC, once you have installed Git in step 1
   you can navigate to the directory you want to use in Windows Explorer,
   open it, and right click inside the directory and select "Git Bash Here"
   to open a new Git Bash terminal with the correct directory pre-designated.
   The Git Bash terminal is slightly different from the standard Command Line,
   but for our purposes it can perform all of the same operations outlined in
   these instructions.


3) Once you have got your terminal pointing at the directory you wish to
   keep the virtual environment in, run the following commands:

      `git clone --recursive https://github.com/wpengine/hgv.git`
      
      `cd hgv`
      
      `npm install`
      
      `vagrant up`

   Bear in mind that running the 'vagrant up' command for the first time may
   take awhile, since the virtual machine needs time to create itself. If you
   need to run the command again for this machine at a later date it will be
   much quicker.

   When you run the command 'vagrant up', you will be asked for a password.
   Just leave this field blank and hit enter three times, and you will still
   be able to run the virtual environment with full administrator privileges.
   This appears to be an optional password for mercury developers.

   You'll know when the installation is finished when the prompt reappears.
   If you see an empty line with no text then it's still installing and just
   needs more time. It may take awhile.


4) When the prompt reappears, the environment installation will be complete
   and you should now have a virtual environment running in your terminal.

   NOTE: In the course of running `vagrant up`, you may have seen the message
   'The guest additions on this VM do not match the installed version of
   VirtualBox'. If so, it is recommended that you run `vagrant plugin install
   vagrant-vbguest` in your terminal when `vagrant up` has finished. This will
   fix this error the next time you run `vagrant up`, so run `vagrant reload`
   to reboot the virtual environment for it to take effect (it won't take
   anything like as long the second time). This is not crucial, but this
   warning can signify potential errors in some cases. If you are unsure
   whether this message appeared, you can install the vbguest plugin anyway if
   you want to be sure.

5) Now that you have created your virtual environment, you will have access to
   two pre-loaded WordPress installations. You should now be able to visit
   http://hgv.test/ and see the Mercury Vagrant documentation page.
   However, if you simply see a 'Not Found' message or something similar,
   follow the instructions under 'Adding A Site to /etc/hosts', and copy and
   paste the following to the end your hosts file on your host machine
   (/etc/hosts):

   ```192.168.150.20 hgv.test
   192.168.150.20 admin.hgv.test
   192.168.150.20 hhvm.hgv.test
   192.168.150.20 php.hgv.test
   192.168.150.20 cache.hhvm.hgv.test
   192.168.150.20 cache.php.hgv.test
   192.168.150.20 xhprof.hgv.test
   ```

   Visit http://hgv.test/ again to check that the domains have been added
   correctly.

   Getting this working is vital for getting access to the documentation for the
   environment.

------------------------------------------------------------------------------

Using The Virtual Environment

------------------------------------------------------------------------------

Basic Functionality

If you have just installed and created the virtual environment, your virtual
environment will already be running. To log in to your virtual machine, simply
type `vagrant ssh`.

To sign into your virtual machine at a later time, you will need to open a
new terminal, and use the `cd <FILEPATH>` command to change directory to that
of your virtual machine (inside the directory of the virtual environment
itself, not the directory that holds the folder containing all of your
virtual machine files). For example, `cd /Users/john/mercury/hgv`; when viewed
in Finder/Windows Explorer, the folder will contain a file called
'Vagrantfile'. You can now run `vagrant up`, and then `vagrant ssh` to boot up
and log in to your virtual machine. Running these commands while in a
different directory will have no effect.

- Boot Up the Virtual Environment: `vagrant up`

- Sign In: `vagrant ssh` (boot up first)

- Sign Out: Ctrl + D

- Safely Stop the Virtual Environment: `vagrant halt` (sign out first)

It is generally advisable to stop the virtual machine when not being used, as
it will allocate a fairly significant proportion of your computer's processing
power to itself while running.


Browsing Sites Hosted by the Virtual Machine

Now that you have created your virtual environment, you will have access to
two pre-loaded WordPress installations. First, follow the instructions under
'Adding A Site to /etc/hosts', and copy and paste the following to the end
your hosts file:

```192.168.150.20 hgv.test
192.168.150.20 admin.hgv.test
192.168.150.20 hhvm.hgv.test
192.168.150.20 php.hgv.test
192.168.150.20 cache.hhvm.hgv.test
192.168.150.20 cache.php.hgv.test
192.168.150.20 xhprof.hgv.test
```

Now visit http://hgv.test/, and you should see the Mercury Vagrant
documentation page.

Getting this working is vital for getting access to the documentation for the
environment.

------------------------------------------------------------------------------

Importing A Website Into the Virtual Environment

------------------------------------------------------------------------------

These instructions assume that you already have a fully-provisioned website
repository set up on a repository hosting service, such as Github.

To install a local version of your website repository, boot up and log in to
the virtual machine, then run the following commands, replacing the <REPO>
with the name of the website repository. Note that if your repository name is
over twelve characters long, you will need to abbreviate it to under this
length:

`cp /vagrant/provisioning/default-install.yml /vagrant/hgv_data/sites/<REPO>.yml`
(Note the space after 'default-install.yml')

`sudo nano /vagrant/hgv_data/sites/<REPO>.yml`

When you run this second command, you will be presented with a text editor in
the terminal window (like Notepad/Notes). Use the arrow keys to navigate, and
modify like any other text file. Do not use Alt + left/right to skip over
words, however, as these commands do something else here.

Change the content of this file to read the following, again replacing <REPO>
with your repository name and <SITE> with the name of your site:

      wp:
        enviro: <REPO>
        hhvm_domains:
          - <SITE>.test
          - www.<SITE>.test
        php_domains:
          - php.<SITE>.test

Please note that any URLs added under hhvm_domains will be loaded using hhvm,
and any added under php_domains will be loaded using php. As a result there
may be a difference in appearance and performance.

The <REPO> name here must match exactly the <REPO> name used previously if
abbreviated. <SITE> can be whatever you wish, but cannot include
spaces or any punctuation apart from periods or dashes. Make a note of all
lines using <SITE>; you will need them later to add to the hosts file.

When you have finished making changes, exit with Ctrl + X, then hit 'y' to
save changes, and return to overwrite the existing file.

Now sign out of the virtual machine with Ctrl + D, and enter the command
'vagrant provision' for the changes to take effect. Note that if you wish to
make any changes to your <REPO>.yml file later, such as changing the URLs or
<REPO>, you will need to update /etc/hosts and run 'vagrant provision' again
to register the changes.

You should now be able to visit your URLs in the browser and see a generic
WordPress site. To swap this for your site, enter the following:

   `rm -r /vagrant/hgv_data/sites/<REPO>`
   
   `cd /vagrant/hgv_data/sites/`
   
   `git clone <URL> <REPO>`

Again, <REPO> must match the previously used repository name. Replace <URL>
with the URl for your website repository.

You may be asked for a username and password. If you do not have an account
with the repository hosting service, a login to use should have been provided
along with these instructions. The password input will be hidden; your typing
will still register even though nothing appears on the screen. If you make a
mistake when entering username or password, the git clone command will simply
fail with no effect when submitted, so just run the 'git clone' command above
again to try again.

The virtual machine will now begin to download the website repository. While
this downloads, you cannot do anything else in this window, so open a second
terminal window and follow the instructions below under 'Adding A Site to
/etc/hosts'. Add the URLs that you saved from <SITE>.yml to your hosts file,
like so:

```
192.168.150.20 <SITE>.test
192.168.150.20 www.<SITE>.test
192.168.150.20 php.<SITE>.test
```

These URLs must match exactly the URLs you added to <SITE>.yml previously.
Exit with Ctrl + X and save the changes as before.

You should now be able to visit any of the three <SITE> URLs in your browser,
and see the first stage of the WordPress setup. If so, you are now ready to
import site data from the database. You do not need to do the WordPress setup;
this will be dealt with when we import from the database.

Please follow the instructions below under 'Import Site Data From A Database'
to complete site setup.

For more information on importing a website into Mercury, please read the
'Add My Own WordPress' section of the Mercury Vagrant documentation.

------------------------------------------------------------------------------

Import Site Data From A Database

------------------------------------------------------------------------------

Before performing the following operations, you should have already followed
the 'Installation Guide' and instructions under 'Importing A Website Into the
Virtual Environment', and be able to visit your site's URL in the browser to
view the first page in the WordPress setup process (language select). If not,
please follow the instructions under 'Importing A Website Into the Virtual
Environment' beforecontinuing.

1) Boot up and log into your virtual machine, in accordance with the
   instructions under 'Using The Virtual Environment'.

2) Run the following commands:
      
      `cd /vagrant/hgv_data/sites/<REPO>`
      
      `mkdir tmp`
      
      `cp wp-config-sample.php wp-config.php`
      
      `sudo apt-get install lftp`
      
      `sudo nano locals.mk`

   This last command will create the locals.mk file, and open it in nano. You
   should have been provided with and ftp user and password; add them to this
   file like so:

      `ftpuser = <add_your_user_here>`
      
      `ftppass = <add_your_password_here>`

   Save and exit using Ctrl + X.

3) In the same directory in the terminal, add the following to wp-config.php
   by running 'sudo nano wp-config.php':

      ```define( 'DEVSITE', true );
      define( 'INTERCEPT_EMAIL', '<EMAIL>' );

      define( 'WP_SITEURL', '<URL>' );
      define( 'WP_HOME', '<URL>' );
      define( 'COOKIE_DOMAINS', '<URL>' );

      define( 'AWS_ACCESS_KEY_ID', '<AWS_ID>' );
      define( 'AWS_SECRET_ACCESS_KEY', '<AWS_KEY>' );

      define( 'WP_TEMP_DIR', '/vagrant/hgv_data/sites/<REPO>/tmp' );
      define( 'ENVATO_USER_NAME', '<ENV_USER>' );
      define( 'ENVATO_API_KEY', '<ENV_KEY>' );
      ```

   You should have been provided with and Amazon Web Services ID and Key
   (<AWS_ID> and <AWS_KEY>), as well as an Envato username and API Key
   (<ENV_USER> and <ENV_KEY>). You can use any of the URLs you added to hosts
   for <WP_SITEURL>, <WP_HOME>, and <COOKIE_DOMAINS>, although you should use
   the same one for all three. Make sure to include single quotes wherever
   substituting your own data here.

4) Update `DB_NAME`, `DB_USER`, and `DB_PASSWORD` in wp-config.php to suit the local
install. `DB_NAME` and `DB_USER` should both be set to `wpe_<REPO>`, replacing `<REPO>`
with the name of your website's repository. By default `DB_PASSWORD` will be
'wordpress'.

5) Run `make fullsync`. This will begin the download process; if your site
is fairly large, this will take some time. It may also slow down your internet
connection, so you may wish to execute this command when it is less likely to
inconvenience other users on your network. If you wish to cancel
the download, hit Ctrl + C, and restart it at any time by re-running the
`make fullsync` command.

6) You website's site data should now be successfully imported. The final step
   is to log in to your site's database and update the URL:

   a) Enter `mysql -u root -p`. You will then be asked for a password; by
      default mercury's wordpress installs will have a blank password for the
      'root' user. Just leave this field empty and hit return.

   b) Enter `use wpe_<REPO>;`, replacing `<REPO>` with the name of your site's
      repository. If you want to double check your site's database in the
      repository, enter `show databases;` to get a complete list. Remember
      that semi-colons on the end of commands are crucial in mysql, as is
      case-sensitivity.

   c) Enter `UPDATE wp_options SET option_value='<URL>' WHERE
      option_name='siteurl';`. Replace `<URL>` with the same URL you used in
      wp-config.php to set `WP_SITEURL` and `WP_HOME`. Exit mysql with
      Ctrl + D.

   d) Enter `sudo service nginx restart`, then log out of the virtual
      environment with Ctrl + D and run `vagrant provision` to update
      the changes made.

   OPTIONAL: Your local site should now be fully up and running, but if you
   wish to gain access to the admin areas and do not have an administrator
   account on the live site when you run `make fullsync`, you will need to
   follow the instructions below under 'Adding A New Site Administrator'. Do
   not attempt to create a new user using the conventional sign-up process, as
   this process will not complete properly in a virtual environment.

TROUBLESHOOTING:

'When I run make fullsync it just says Connecting and sits there not doing
anything, then keeps trying to reconnect'

There is probably an issue with the 'lftp' line in Makefile in your
site repository's directory. The Makefile contains the code that defines the
`make fullsync` command. Open this file in your terminal by navigating to the
site repository directory using `cd`, then run `sudo nano Makefile`. In the
first block of text you will see a line that begins `lftpauth :=` (about nine
lines into the block). This particular error suggests an issue with the IP
address or URL at the end of the line; make sure that the IP address/URL is
correct, as well as the port number (the number after `-p`), and make sure
that your locals.mk file is in the same directory as Makefile, and that the
details inside it are correct and properly laid out. Finally, make sure that
the line `-include locals.mk` is included somewhere above the lftpauth line in
Makefile. If you are unsure about how to interpret this line, copy and send it
to your site administrator and ask them to check the details for you.


------------------------------------------------------------------------------

Adding A New Site Administrator

------------------------------------------------------------------------------

If you do not have an administrator account on the live site you are
presumably duplicating, you will need to create a new administrator account on
the local database. Because the site's email connectivity is not configured to
work on a local machine, you will not be able to create a user through the
conventional sign-up form, as sign-up emails will not be sent by the server.
Instead, you will have to access the server's database and add the
administrator manually:

   1) Boot up and log in to your virtual environment as described in 'Using
      The Virtual Environment'. You won't need to run any further `cd`
      commands to access mysql.

   2) Enter `mysql -u root -p`. You will then be asked for a password; by
      default mercury's wordpress installs will have a blank password for the
      `root` user. Just leave this field empty and hit return.

   3) Enter `use wpe_<REPO>;`, replacing <REPO> with the name of your site's
      repository. If you want to double check the name of your site's
      database on the server, enter `show databases;` to get a complete
      list. Remember that semi-colons on the end of commands are crucial in
      mysql.

   4) Enter `SELECT ID from wp_users ORDER BY ID DESC LIMIT 1;`. Make a note
      of the ID number returned.

   5) Enter the following commands, replacing all placeholders in `< >` with
      your own information. Replace `<ID>` with the value you just made a note of
      plus one.

         `INSERT INTO wp_users(ID, user_login, user_pass, user_nicename,
            user_email, user_status, display_name) VALUES ('<ID>', '<USERNAME>',
            MD5('<PASSWORD>'), '<USERNAME>', '<EMAIL>',
            '0', '<NAME>');`

         `DELETE FROM wp_usermeta WHERE user_id='<ID>' AND
            meta_value='a:1:{s:10:"subscriber";b:1;}';`

         `INSERT INTO wp_usermeta(umeta_id,
            user_id, meta_key, meta_value) VALUES (NULL, '<ID>',
            'wp_capabilities', 'a:1:{s:13:"administrator";b:1;}');`

         `INSERT INTO wp_usermeta(umeta_id, user_id, meta_key,
            meta_value) VALUES (NULL, '<ID>', 'wp_user_level', '10');`

      NOTE: Remember to maintain single quotes wherever used in examples,
      including around placeholders, semi-colons at the end of each command,
      and that commands are case-sensitive.

You should now be able to view your site's login page at <SITEURL>/wp-admin,
and log in with the information you just added to the database. If you make a
mistake when running any of the above and accidentally create bad data, use
any of the following commands to check or delete data, then re-create it with
the previous commands:

   Check user in wp_users:

      `SELECT * FROM wp_users WHERE ID='<ID>';`

   Delete user from wp_users:

      `DELETE FROM wp_users WHERE ID='<ID>';`
      (note there's no * in this command)

   Check wp_capabilities/wp_user_level in wp_usermeta (replace `<VALUE>` with
   the value you wish to check):

      `SELECT * FROM wp_users WHERE user_id='<ID>' AND meta_key='<VALUE>';`

   Delete wp_user_level/wp_capabilities from wp_usermeta(replace `<VALUE>` with
   the value you wish to check):

      `SELECT * FROM wp_user_level WHERE user_id='<ID>' AND meta_key='<DELETE>';`

When you log into the admin area, you may be redirected to the update profile
screen. If this happens, simply delete '/profile.php' from the end of the URL
and hit refresh to access the main dashboard.

You may also be prompted to update your password, as the server believes you
are using an automatically-generated password following conventional
registration. This can be safely ignored, as you created your own password
manually in mysql.

NOTE: A simple alternative to this method would be to create an administrator
account on the live site using the conventional account creation process
before importing site data. Importing site data specifically for this purpose
is certainly possible after initial setup using 'make sync', although this
process may take some time to complete.

------------------------------------------------------------------------------

Adding A Site To /etc/hosts

------------------------------------------------------------------------------

Note that you should NOT be signed in to your virtual environment when
following these instructions (if you are signed in in a different terminal
window this is ok). To make the following changes to your machine you must
have administrator/sudo privileges.

1) run 'sudo nano /etc/hosts' on Mac or Linux or
   'sudo nano c:\windows\system32\drivers\etc\hosts' on Windows.

2) When prompted, enter the password you use to log into the computer you're
   using.

3) Copy and paste the necessary lines into the end of your hosts file.

4) Save and exit, being careful not to change anything else, by hitting
   Ctrl + X, then 'y' to save changes, and then hit return to save the file
   with the same name and overwrite the existing version with your updated
   one.

------------------------------------------------------------------------------

Updating Your Imported Site

------------------------------------------------------------------------------

If changes have been made to your website repository, you can view them in
your virtual environment.

Boot up and sign in to your virtual environment, then enter the following
commands, replacing <REPO> with the name of your website repository:

`cd /vagrant/hgv_data/sites/<REPO>`

`git pull`

After downloading any changes, the updated version will be viewable in the
browser.
