
Installation Guide For WP Engine's Mercury Environment For Vagrant

==============================================================================

Introduction

------------------------------------------------------------------------------

This guide is intended to help you install WP Engine's Mercury Environment For
Vagrant, and then use this environment to host a website on your machine.
Vagrant uses virtual machine software to essentially create a "second
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

Installation Guide

------------------------------------------------------------------------------

1) Open a Command Line window as described above. Next, go to the link below
   and follow the instructions to install the prereqisuites for the
   environment, but NOT the installation instructions:

   		https://github.com/wpengine/hgv

   All prerequisistes are installed system-wide on your computer, so the
   commands to install them can be typed directly into the Command Line on
   opening.
  
   Make sure you have all prerequisites installed before continuing,
   but not vagrant-ghost.


2) Before installing the environment, make sure that your Command Line window
   is pointing to the correct folder. Failing to do this will install the
   environment in whatever directory (folder) your terminal was pointing at when you
   opened it (most likely your user's home folder on your system). If you are
   unsure what directory you are currently in, it will be listed at the start
   of every new line, before the dollar sign on mac, or '>' on windows.

   The installation commands for the environment will operate on whatever
   directory the command line window is pointing at when they are run. To
   change where the terminal is pointing, type 'cd <FILEPATH>', replacing
   <FILEPATH> with the full path to the folder you want to install the
   environment in. For example, to install the environment in the 'mercury'
   directory in the home directory of user 'john', type
   'cd /Users/john/mercury' on a Mac or 'cd C:\Users\John\Mercury' on a PC.
   
   Alternatively, if you are using a PC, once you have installed Git in step 1
   you can navigate to the directory you want to use in Windows Explorer,
   open it, and right click inside the directory and select "Git Bash Here"
   to open a new Git Bash terminal with the correct directory pre-designated.
   The Git Bash terminal is slightly different from the standard Command Line,
   but for our purposes it can perform all of the same operations outlined in
   these instructions.


3) You should now be ready to follow the installation instructions. Note that
   in step 2) of the instructions it refers to 'changing into' the hgv
   directory. This just means running the cd command again; this time you can
   just run 'cd hgv' to move the terminal's pointer into the new directory.
   You should also bear in mind that running the 'vagrant up' command for the
   first time may take awhile, since the virtual machine needs time to create
   itself. If you need to run the command again for this machine at a later
   date it will be much quicker.

   When you run the command 'vagrant up', you will be asked for a password.
   Just hit enter three times, and you will still be able to continue with the
   loading process with full administrator privileges. This appears to be an
   optional password for mercury developers.

   You'll know when the installation is finished when the prompt reappears.
   If you see an empty line with no text then it's still installing and just
   needs more time. It may take awhile.


4) When the prompt reappears, the environment installation will be complete
   and you should now have a virtual environment running in your terminal.

   NOTE: In the course of running 'vagrant up', you may have seen the message
   'The guest additions on this VM do not match the installed version of
   VirtualBox'. If so, it is recommended that you run 'vagrant plugin install
   vagrant-vbguest' in your terminal when 'vagrant up' has finished. This will
   fix this error the next time you run 'vagrant up', so run 'vagrant reload'
   to reboot the virtual environment for it to take effect (it won't take
   anything like as long the second time). This is not crucial, but this
   warning can signify potential errors in some cases. If you are unsure
   whether this message appeared, you can install the vbguest plugin anyway if
   you want to be sure.


------------------------------------------------------------------------------

Using The Virtual Environment

------------------------------------------------------------------------------

Basic Functionality

If you have just installed and created the virtual environment, your virtual
environment will already be running. To log in to your virtual machine, simply
type 'vagrant ssh'.

To sign into your virtual machine at a later time, you will need to open a
new terminal, and use the 'cd <FILEPATH>' command to change directory to that
of your virtual machine (inside the directory of the virtual environment
itself, not the directory that holds the folder containing all of your
virtual machine files). For example, 'cd /Users/john/mercury/hgv'; when viewed
in Finder/Windows Explorer, the folder will contain a file called
'Vagrantfile'. You can now run 'vagrant up', and then 'vagrant ssh' to boot up
and log in to your virtual machine. Running these commands while in a
different directory will have no effect.

Boot Up the Virtual Environment: 'vagrant up'

Sign In: 'vagrant ssh' (boot up first)

Sign Out: Ctrl + D

Safely Stop the Virtual Environment: 'vagrant halt' (sign out first)

It is generally advisable to stop the virtual machine when not being used, as
it will allocate a fairly significant proportion of your computer's processing
power to itself while running.


Browsing Sites Hosted by the Virtual Machine

Now that you have created your virtual environment, you will have access to
two pre-loaded WordPress installations. First, follow the instructions under
"Adding A Site to /etc/hosts", and copy and paste the following to the end
your hosts file:

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

To install a local version of your website repository, boot up and log in to
the virtual machine, then run the following commands, replacing the <REPO>
with the name of the website repository. Note that if your repository name is
over twelve characters long, you will need to abbreviate it to under this
length:

'cp /vagrant/hgv_data/config/sites/default-install.yml /vagrant/hgv_data/sites/<REPO>.yml'
(Note the space after 'default-install.yml')

'sudo nano /vagrant/hgv_data/sites/<REPO>.yml'

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

Now run the following commands, replacing <URL> with the complete URL of your
website's repository, and <REPO> as before:

'cd /vagrant/hgv_data/sites/'
'git clone <URL> <REPO>'

Again, <REPO> must match the previously used repository name.

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

192.168.150.20 <SITE>.test
192.168.150.20 www.<SITE>.test
192.168.150.20 php.<SITE>.test

These URLs must match exactly the URLs you added to <SITE>.yml previously.
Exit with Ctrl + X and save the changes as before.

When the repository has finished downloading, sign out of the virtual machine
with Ctrl + D, and enter the command 'vagrant provision' for the changes to
take effect. Note that if you wish to make any changes to your <SITE>.yml
file later, such as changing the URLs or <REPO>, you will need to update
/etc/hosts and run 'vagrant provision' again to register the changes.

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
      
      'cd /vagrant/hgv_data/sites/<REPO>'
      'mkdir tmp'
      'sudo nano locals.mk'

   This last command will create the locals.mk file, and open it in nano. You
   should have been provided with and ftp user and password; add them to this
   file like so:

      ftpuser = <add_your_user_here>
      ftppass = <add_your_password_here>

   Save and exit using Ctrl + X.

3) In the same directory in the terminal, add the following to wp-config.php
   by running 'sudo nano wp-config.php':

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

   You should have been provided with and Amazon Web Services ID and Key
   (<AWS_ID> and <AWS_KEY>), as well as an Envato username and API Key
   (<ENV_USER> and <ENV_KEY>). You can use any of the URLs you added to hosts
   for <WP_SITEURL>, <WP_HOME>, and <COOKIE_DOMAINS> (although you should use
   the same one for all three). Make sure to include single quotes wherever
   substituting your own data here.

4) Update DB_NAME, DB_USER, and DB_PASSWORD in wp-config.php to suit the local
install. DB_NAME and DB_USER should both be set to 'wpe_<REPO>', replacing <REPO>
with the name of your website's repository. By default DB_PASSWORD will be
'wordpress'.

5) You are almost ready to import from the database. Two final commands to
run before starting the download:
   
   'sudo apt-get install lftp'
   'mkdir /vagrant/hgv_data/sites/<REPO>_tmp'

6) Run 'make fullsync'. This will begin the download process; if your site
is fairly large, this will take some time. It may also slow down your internet
connection, so you may wish to execute this command when it is less likely to
inconvenience other users on your network. If you wish to cancel
the download, hit Ctrl + C, and restart it at any time be re-running the
'make fullsync' command.

7) You website's site data should now be successfully imported. The final step
   is to run 

------------------------------------------------------------------------------

Adding A Site To /etc/hosts

------------------------------------------------------------------------------

Follow the instructions at the link below to add the lines given previously to
your /etc/hosts file on your operating system:

https://www.howtogeek.com/howto/27350/beginner-geek-how-to-edit-your-hosts-file/

Note that you should NOT be signed in to your virtual environment when
following these instructions (if you are signed in in a different terminal
window this is ok).

The above instructions may prompt you to use 'vim' in the command line.
This will open a text editor program in your terminal window (like
Notepad/Notes), but it may not seem that intuitive. If so, exit vim by typing
':q' and hitting enter, then re-run the vim command but replace the word 'vim'
with 'nano' for a more user-friendly alternative.

Copy and paste the necessary lines into the end of your hosts file. Save and
exit, being careful not to change anything else. If you are using
'nano' as suggested above, exit by hitting Ctrl + X, then 'y' to save changes,
and then hit return to save the file with the same name and overwrite
the existing version with your updated one.

------------------------------------------------------------------------------

Updating Your Imported Site

------------------------------------------------------------------------------

If changes have been made to your website repository, you can view them in
your virtual environment.

Boot up and sign in to your virtual environment, then enter the following
commands, replacing <REPO> with the name of your website repository:

'cd /vagrant/hgv_data/sites/<REPO>'

'git pull'

After downloading any changes, the updated version will be viewable in the
browser.
