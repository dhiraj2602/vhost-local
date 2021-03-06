# How to set virtual host on local machine (mac)

### Steps
- Make a folder in your root directory (/Users/<username>) by name frontend and set its permission to <username>:wheel
  ```sh
  $ sudo chown -R <username>:wheel /path/to/frontend
  ```
- Add following line to /etc/hosts: 
    ```sh
    127.0.0.1		vmock.localhost
    ```
- Go to /etc/apache2/httpd.conf
  ```sh
  $ sudo vim /etc/apache2/httpd.conf
  ```
- Uncomment following lines:
  ```sh
  LoadModule rewrite_module libexec/apache2/mod_rewrite.so
  Include /private/etc/apache2/extra/httpd-vhosts.conf
  ```
- Uncomment ServerName from /etc/apache2/httpd.conf and set it to vmock.localhost

- Go to /etc/apache2/extra/httpd-vhosts.conf and paste following:
  ```sh
    <VirtualHost *:80>
        ServerName vmock.localhost
        DocumentRoot "/Users/<username>/frontend/dashboard/public"
        Alias "/resume" "/Users/<username>/frontend/resume/public"
        ErrorLog "/private/var/log/apache2/error_log"
        CustomLog "/private/var/log/apache2/access_log" common
        <Directory "/Users/<username>/frontend">
            AllowOverride All
            Require all granted
        </Directory>
    </VirtualHost>
  ```
- Test apache configuration and restart apache
  ```sh
  $ sudo apachectl configtest
  $ sudo apachectl restart
  ```
### Setup Repo folders on local
- Make folders in frontend directory corresponding to each frontend repo/module with the name specified in virtualhost config. For eg. dashboard for dashboard-ui and resume for resume-ui.
    

- Go to each folder in terminal and clone corresponding repo using following commands:
  ```sh
    $ git clone <repo path> .
  ```
- Run build processes
  ```sh
  $ npm install
  $ cp -a .env.dev .env
  $ npm start
  ```
### Disable samesite cookie policy on chrome
- Go to chrome://flags in google chrome, search "SameSite by default cookies" and disable the policy. Google Chrome (this policy) does not allow cookies from different site to be sent, and we are using vmock.localhost and calling \*.vmock.com.

##### vmock.localhost should work for you now.
