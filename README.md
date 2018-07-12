# installinNodeOnEc2
Install Node on Ec2

    Enter ssh o remote machine
    Change to ec2-user → sudo su ec2-user
    Download distributtion
	```
        cd /tmp
        wget https://nodejs.org/dist/v7.9.0/node-v7.9.0-linux-x64.tar.gz
	```
    Create a folder to extract distributtion
	```
        sudo mkdir /usr/local/node  (executed from /tmp )
        sudo tar --strip-components 1 -xzvf node-v* -C /usr/local/node  (executed from /tmp )
	```
    Check installation
	```
        usr/local/node/bin/node --version   → will print the version
	```
	
	Now if you want to manage you node process by supervisor
    Create the process to be managed by supervisor (example mynode service)
	```
        cd /etc/supervisor/conf.d
        sudo touch mynode.conf
        sudo vi ./mynode.conf and add this example
            [program:mynode]
            command=/usr/local/node/bin/node /usr/local/mynode/bin/www 
            directory=/usr/local/mynode
            autostart=true
            autorestart=true
            startretries=3
            stderr_logfile=/var/log/mynode/mynode.err.log
            stdout_logfile=/var/log/mynode/mynode.out.log
            user=ec2-user
            #environment=SECRET_PASSPHRASE='this is secret',SECRET_TWO='another secret'
	```
    Restart supervisor
	```
        sudo service supervisord restart
	```
    Check that the processs is running
	```
        sudo /usr/local/bin/supervisorctl status

        [daniel.pena@172 ~]$ sudo /usr/local/bin/supervisorctl status

        mynode RUNNING pid 16446, uptime 0:16:24

    ```
