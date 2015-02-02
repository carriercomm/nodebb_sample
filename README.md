# nodebb_sample
sample app to test nodebb applications

##Instructions: initial setup to mongodb

1) <code>clone git repo git clone -b v0.6.x https://github.com/NodeBB/NodeBB.git</code> nodebb (skip this step if you are cloning this repo)

2) for (Ubuntu users) install the required programs: sudo apt-get install git nodejs nodejs-legacy npm redis-server imagemagick build-essential

3) npm install 

4) type <code>mongo</code> in the command shell

5) type <code>use nodebb</code> (creates nodebb db)

6) <code>db.createUser( { user: "nodebb", pwd: "nodebb", roles: [ "readWrite" ] } )</code>

7) <code>sudo vim /etc/mongod.conf </code>

8) To enable authentication, uncomment <code>auth = true</code> Restart MongoDB.

9) <code>sudo service mongod restart</code>

** from this point on logging into mongo you need to authenticate <code>db.auth()</code> : [refer docs](http://docs.mongodb.org/manual/reference/method/db.auth/)

10) go to project root_folder

11) <code>node app --setup</code> in the terminal

* Change the hostname to your domain name.

* Accept the defaults by pressing enter until it asks you what database you want to use. Type mongo in that field.

* Accept the default port, unless you changed it in the previous steps.

* Change your username to nodebb, unless you set it to another username.

* Enter in the password you made in step 6.

* Change the database to nodebb, unless you named it something else.


** if the node setup fails for some reason run <code>./nodebb dev</code> - it will try to start nodebb in dev mode, fail and print the error message

** running the setup also makes you create the first admin account, enter account details there

12) run nodebb: <code>./nodebb start </code>

12) goto [http://localhost:4567/admin](http://localhost:4567/admin) (or whatever ip/port configuration you used)


[refer docs](https://media.readthedocs.org/pdf/nodebb/latest/nodebb.pdf)



##Google Single Sign On Plugin

1) <code>npm install nodebb-plugin-sso-google</code>

2) Create a Google Application via the API Console

3) Locate your Client ID and Secret

4) goto to [localhost:4567/admin](localhost:4567/admin)

5) goto extend -> plugins

6) activate the google sso plugin

7) restart nodebb

8) go back to the admin page

9) goto social authenticaation-> google 

10) paste the client id and secret

restart nodebb

**important for the google signup/login to show up the config.json url must not be localhost but a valid url.  The site is still hosted on localhost:4567.  (Not sure about this part. Originally when it was localhost I did not get the google login, but after changing to a valid url I started getting the google login, then even though I tried changing back to localhost, the google login was still there)

[refer docs](https://media.readthedocs.org/pdf/nodebb/latest/nodebb.pdf)

##NodeBB config.json

[refer docs](https://docs.nodebb.org/en/latest/configuring/config.html)

##NodebBB nginx proxy

follow the tutorial at: [tutorial](https://docs.nodebb.org/en/latest/configuring/proxies/nginx.html)

but instead of using the given server configuration use the below config  

<code>
	server {
		listen 80;
		server_name paratmodlocal.com;
		location /forum {
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
			proxy_set_header Host $http_host;
			proxy_set_header X-NginX-Proxy true;

			proxy_pass http://127.0.0.1:4567/;
			proxy_redirect off;

			# Socket.IO Support
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
		}
	}

</code>

the above configuration is needed becuase so that the url can be redirected to a subdir ([http://localhost:4567/forum](http://localhost:4567/forum))
instead of the root url

to test use the ip address: eg: [192.168.1.5/forum](192.168.1.5/forum)

##Nodebb 3rd party single sign on

1) Set up an oauth2 endpoint on your website.  
An examsple has been provided at this [repo](https://github.com/sajithdil/oauth2orize-example)

2) add this plugin to nodebb [repo](https://github.com/sajithdil/nodebb-plugin-sso-oauth)  
its not on npm so do a <code>npm intsall git+https://github.com/sajithdil/nodebb-plugin-sso-oauth --save </code>(this will also save the dependency to package.json)

3) go to _library.js_ of the above set up plugin and do the configuration required.

thats it, now you can do a sso to a 3rd party website of your choice

