title: Hosting this blog on openshift
link: http://www.ashwinupadhyaya.com/2013/hosting-this-blog-on-openshift/
author: ashwin67
description: 
post_id: 1360
created: 2013/09/21 09:46:51
created_gmt: 2013/09/21 09:46:51
comment_status: open
post_name: hosting-this-blog-on-openshift
status: publish
post_type: post

# Hosting this blog on openshift

I'm sure there are hundreds of posts already on how to do this. [Openshift](https://www.openshift.com) has a one click installation as well. It's ok. I'll still write my take on it. The idea is to learn something about how openshift works and how to deploy a simple application on the cloud. A one click install does not provide learning opportunity. However, taking the longer route and doing a few mistakes in the process does exactly that. Let me make it clear, this is not written after I completed the process. Instead, I am yet to start the process. By the time I finish, I should be finished with the porting of my existing blog from a rented server space to openshift cloud. I owe a lot to [this blog by amit shah](http://log.amitshah.net/2011/12/blog-moved-to-wordpress-on-openshift/) apart from the documentation found on openshift and [stackoverflow](http://stackoverflow.com/). And to those who don't know what is openshift, it is just like amazon [AWS](https://aws.amazon.com). What is required before you start? 

  1. Some knowledge of [GIT](http://git-scm.com/)
  2. A user account in openshift.
Before you start.. 
  1. Create an account in openshift, go to the settings page and create a namespace as well.
  2. Install all the tools that are required for deploying your application (in this case, a wordpress application) into the cloud. Installation of ruby, git and rhc are documented in the [following openshift documentation](https://www.openshift.com/developers/rhc-client-tools-install).
  3. I also prefer to have these things done in a linux environment as everything just works out of the box (or rather command line). In windows, I normally have to set a whole lot of environment variables, download [putty](http://www.putty.org/) for ssh related stuff and so on. Anyway, it is a personal choice. You could try [cygwin](http://www.cygwin.com/) though I haven't tried it myself in this particular case.
  4. Once those are installed, it is time to setup SSH keys. This allows you to remotely access your application. On linux, all you have to do is run 
    * **rhc setup**.
  5. On windows, you will have to play around with putty first. A detailed explanation is given here: [Openshift Remote access](https://www.openshift.com/developers/remote-access).
Prepare backend... 
  1. First, type 
    * **rhc cartridge list**
    * This gives a list of all the cartridges that are available for you to create apps. Cartridges are nothing but platforms; ex: php, node.js, python, ruby etc. You will have to use the correct version numbers in the below commands to get things working correctly.
  2. In the terminal, just type the following: 
    * **rhc app create -a xyz-t php-5.3 **
    * where  xyz is the name of the application and can be anything. This will take a few seconds and will create an application on the server. Also a copy of the code for your application will be checked out locally into a folder with the same name as your application.
  3. Now, add mysql cartridge to this app using the command: 
    * **rhc cartridge add mysql-5.1 -a xyz**
    * Note down the credentials that are output by the terminal somewhere.
  4. Now, add phpmyadmin which is just like the previous step: 
    * **rhc cartridge add phpmyadmin-4 -a xyz**
    * The credentials should be the same as the previous one.
  5. Now login to the following link: https://xyz-namespace.rhcloud.com/phpmyadmin/ . This is your own phpmyadmin page. Replace the app name and namespace with your own. Create a new user and a database there. The database should have full privileges for the user.
Now deploy wordpress... 
  1. Download the wordpress bundle from [Wordpress.org](http://wordpress.org/). Unzip that file and place the contents of that file into the php folder of your app folder. It should look similar to the [following example](https://github.com/openshift/wordpress-example).
  2. Now open wp-config-sample.php file in a text editor and fill in the details there. You should use [environment variables provided by Openshift](https://www.openshift.com/page/openshift-environment-variables#Notes)to do this. So, the contents would like this: 
    * define('DB_NAME', $_ENV['OPENSHIFT_APP_NAME']);
    * define('DB_USER', $_ENV['OPENSHIFT_MYSQL_DB_USERNAME']);
    * define('DB_PASSWORD', $_ENV['OPENSHIFT_MYSQL_DB_PASSWORD']);
    * define('DB_HOST', $_ENV['OPENSHIFT_MYSQL_DB_HOST']);
  3. Also generate the secret keys as described in the
  4. In case of any problems, you could look at the [wordpress 5 minute installation help](http://codex.wordpress.org/Installing_WordPress#Famous_5-Minute_Install).
  5. Now create/modify the following file: .openshift/action_hooks/deploy and make it look like the following: [Wordpress-example-deploy](https://github.com/openshift/wordpress-example/blob/master/.openshift/action_hooks/deploy).
  6. Next, push your repository to openshift: 
    * **git add --all**
    * **git commit -a -m "Type some commit message here"**
    * **git push -u origin master**
  7. That's it. Your wordpress blog is now almost ready. In the browser, type <app>-<namespace>.rhcloud.com. Replace <app> and <namespace> with what you have given.
  8. Now, enter all the details that is asked there. Your blog is now ready and you can start adding posts/pages.
  9. If you already have a blog somewhere else, you can now import the contents from there to your new installation using the tools provided in the control panel.

Alias and custom domain

  1. In the wordpress control panel, in the general settings tab, change the wordpress address and site address to your custom domain.
  2. Type the following command so that your custom domain is shown: 
    * **rhc alias add xyz <del>www.ashwinupadhyaya.com</del>**
  3. In your custom domain provider's cpanel, you will have to change the cname so that it points correctly to your <app>-<namespace>.rhcloud.com.

There are further things to be done in order to gain full functionality. For example, facebook integration, change of uploads directory etc can be done. I'll not cover those here. And yes, by the time I finished this blog, I am now on openshift.