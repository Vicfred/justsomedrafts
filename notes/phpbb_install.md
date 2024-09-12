# Install phpbb in gentoo

make directory
```
mkdir -p /var/www/phpbb
```
download phpbb
```
wget -P /var/www/ https://download.phpbb.com/pub/release/3.3/3.3.12/phpBB-3.3.12.tar.bz2
```
decompress the file, delete the file and rename the directory
```
cd /var/www
tar xjvf phpBB-3.3.12.tar.bz2
rm phpBB-3.3.12.tar.bz2
mv phpBB3/ phpbb
```
make config.php and store/ cache/ files/ images/avatars/upload/ writable
```
chmod 666 config.php
chmod -R 777 store/ cache/ files/ images/avatars/upload/
```
INSTALL USING THE INSTALLER VIA WEB

change config.php permissions back to something safe
```
chmod 644 config.php
```
remove the install directory
```
rm -rf install/
```
if loging out and login back fails clear cookies and cache or try other browser

# Install postgresql

setup the user and db for php using postgresql

login to postgresql
```
psql -U postgres
```
create user and setup his password
```
CREATE ROLE vicfred WITH LOGIN;
\password vicfred
```
create the db for that user
```
CREATE DATABASE testdb WITH OWNER vicfred;
```