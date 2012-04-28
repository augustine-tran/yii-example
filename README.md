Yii on OpenShift
======================

What is [Yii](http://www.yiiframework.com/)
---------------
[Yii](http://www.yiiframework.com/) is a high-performance PHP framework best for developing Web 2.0 applications. Yii comes with rich features: MVC, DAO/ActiveRecord, I18N/L10N, caching, authentication and role-based access control, scaffolding, testing, etc. It can reduce your development time significantly.


This git repository helps you get up and running quickly a Yii installation
on OpenShift.  The backend database is MySQL and the database name is the 
same as your application name (using $_ENV['OPENSHIFT_APP_NAME']).  You can name
your application whatever you want.  However, the name of the database will always
match the application so you might have to update .openshift/action_hooks/build.


Running on OpenShift
----------------------------

Create an account at http://openshift.redhat.com/

Create a php-5.3 application (you can call your application whatever you want)

    rhc app create -a yii -t php-5.3

Add MySQL support to your application

    rhc app cartridge add -a yii -c mysql-5.1

Add this upstream Yii repo

    cd yii 
    git remote add upstream -m master git@github.com:Umasankar-Natarajan/yii-example.git
    git pull -s recursive -X theirs upstream master
    # note that the git pull above can be used later to pull updates to Wordpress
    
Then push the repo upstream

    git push

That's it, you can now checkout your application at (default admin account is admin/OpenShiftAdmin):

    http://yii-$yournamespace.rhcloud.com


NOTES:

GIT_ROOT/.openshift/action_hooks/deploy:
    This script is executed with every 'git push'.  Feel free to modify this script
    to learn how to use it to your advantage.  By default, this script will create
    the database tables that this example uses.

    If you need to modify the schema, you could create a file 
    GIT_ROOT/.openshift/action_hooks/alter.sql and then use
    GIT_ROOT/.openshift/action_hooks/deploy to execute that script (make sure to
    back up your application + database w/ 'rhc app snapshot save' first :) )

Gii Security:
    It would be better that you change the config/main.php file to update the Gii Password and IP Address Protection.
