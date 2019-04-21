Install Metasploit in ArchLinux
-------------------------------
First we must install the RVM:

$ gpg2 --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB

$ \curl -sSL https://get.rvm.io | bash -s stable

$ sudo pacman -S ruby

$ \curl -sSL https://get.rvm.io | bash -s stable --rails

We will install Postgresql:
---------------------------

$ sudo pacman -Syy

$ sudo pacman -S postgresql

$ postgres --version

$ sudo pacman -S git ruby gcc patch curl zlib readline autoconf automake diffutils make libtool bison

$ wget -O rvm.sh https://get.rvm.io

$ ruby -v

$ bash rvm.sh stable --autolibs=enabled --ruby=<ruby version>
  
$ sudo nano /etc/gemrc (we comment the final line)
    #gem: --user-install
$ sudo -u postgres initdb --locale en_US.UTF-8 -E UTF8 -D '/var/lib/postgres/data'

$ sudo systemctl start postgresql

$ sudo systemctl enable postgresql

$ sudo -u postgres createuser msfgit -P -S -R -D
(password: msf)

$ sudo -u postgres createdb -O msfgit msf

$ cd ~

$ mkdir .msf4

$ cd .msf4

$ cat > database.yml

production:
   adapter: postgresql
   database: msf
   username: msfgit
   password: msf
   host: 127.0.0.1
   port: 5432
   pool: 75
   timeout: 5
   
$ sudo pacman -S metasploit

$ source ~/.rvm/scripts/rvm

$ msfconsole


If it fails: The start fails because some Ruby libraries are missing. So we install them by using bundler.

$ bundle install

After the installation process has finished. It's time to start the Metasploit console again.

$ msfconsole
This time the startup should be successful. On the Metasploit console we can check the database connection.
msf > db_status
[*] postgresql connected to msf

Try to search an exploit. If the caching process hasn't been finished it will take some time to list the available modules.
msf > search windows
[!] Database not connected or cache not built, using slow search

After the cache has been initialized successfully the search should be much faster.
msf > search windows
[...]

On the first startup Metasploit Framework automatically creates additional files and folders in the ~/.msf4 directory.
$ ls -l ~/.msf4
total 28
-rw-r--r-- 1 user user  150 31. Mar 09:13 database.yml
-rw-r--r-- 1 user user  267 31. Mar 09:45 history
drwxr-xr-x 2 user user 4096 31. Mar 09:32 local
drwxr-xr-x 3 user user 4096 31. Mar 09:32 logs
drwxr-xr-x 2 user user 4096 31. Mar 09:32 loot
drwxr-xr-x 2 user user 4096 31. Mar 09:32 modules
drwxr-xr-x 2 user user 4096 31. Mar 09:32 plugins

After a reboot make sure the database is running, enter the RVM environment and start the Metasploit console.
$ sudo systemctl status postgresql
● postgresql.service - PostgreSQL database server
   Loaded: loaded (/usr/lib/systemd/system/postgresql.service; disabled)
   Active: inactive (dead)
$ sudo systemctl start postgresql
$ source ~/.rvm/scripts/rvm
$ msfconsole


ENJOY!!!!!
