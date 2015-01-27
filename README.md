# nodebb_sample
sample app to test nodebb applications

#Instructions (initial setup that I went through) - sets up to mongodb (not redis)

1) clone git repo git clone -b v0.6.x https://github.com/NodeBB/NodeBB.git nodebb (skip this step if you are cloning this repo)

2) for (Ubuntu users) install the required programs: sudo apt-get install git nodejs nodejs-legacy npm redis-server imagemagick build-essential

3) npm install 

4) type 'mongo' in the command shell

5) type 'use nodebb' (creates nodebb db)

6) db.createUser( { user: "nodebb", pwd: "nodebb", roles: [ "readWrite" ] } )

7) sudo vim /etc/mongod.conf 

8) To enable authentication, uncomment auth = true. Restart MongoDB.

9) sudo service mongod restart

** from this point on logging into mongo you need to authenticate db.auth() : http://docs.mongodb.org/manual/reference/method/db.auth/

10) go to project root_folder

11) node app --setup

• Change the hostname to your domain name.

• Accept the defaults by pressing enter until it asks you what database you want to use. Type mongo in that field.

• Accept the default port, unless you changed it in the previous steps.

• Change your username to nodebb, unless you set it to another username.

• Enter in the password you made in step 6.

• Change the database to nodebb, unless you named it something else.


** if the node setup fails for some reason run './nodebb dev' - it will try to start nodebb in dev mode, fail and print the error message

** running the setup also makes you create the first admin account, enter account details there

12) run nodebb: ./nodebb start 

12) goto http://localhost:4567/admin (or whatever ip/port configuration you used)


refer: https://media.readthedocs.org/pdf/nodebb/latest/nodebb.pdf



#Google Single Sign On Plugin

1) npm install nodebb-plugin-sso-google

2) Create a Google Application via the API Console

3) Locate your Client ID and Secret

4) goto to localhost:4567/admin

5) goto extend -> plugins

6) activate the google sso plugin

7) restart nodebb

8) go back to the admin page

9) goto social authenticaation-> google 

10) paste the client id and secret

restart nodebb

**important for the google signup/login to show up the config.json url must not be localhost but a valid url. the site is still hosted on localhost:4567. (Not sure about this part. Originally when it was localhost I did not get the google login, but after changing to a valid url I started getting the google login, then even though I tried changing back to localhost, the google login was still there)
