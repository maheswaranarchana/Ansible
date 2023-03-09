# Ansible
Let us see each play and its purpose along with what module is used.

    Download Open JDK  – we are using theapt module to install java as a system package
    
    Validate if Java is available –   shell module is used to run a java -version command, 
        the playbook would fail if the command fails (or) not found
   
   Create a Group – we are using a group module is used to create linux user group named tomcat
   
   Create a User – we are creating a new user named tomcat using user module, this user and the 
        group would be used by the tomcat application server as its not recommended to run servers on Root
   
   Directory creation  – the file module is used to create a directory where the tomcat server be installed 
        and operated. CATALINA_BASE /opt/tomcat8 in our case
    
    Download Tomcat installable tar  – the unarchive module is used to download tomcat from the URL and to 
        untar once the file is downloaded. we have combined the efforts of get_url and unarchive together as 
        unarchive can have URL as a source
    
    Moving files to the right directory – Once the untar is done. it would leave us with a big directory with a 
    clumsy name containing the version etc. we are basically moving the content 
    from apache-tomcat-**** to /opt/tomcat8 for simplicity using shell module, you can use copy module too
   
   Creating a Service file for tomcat – we are using the copy module to create a file in remote and the content 
    of the service file is hard coded inside the playbook itself. another way to do it is to use template but this 
    is convenient. we are using |- to maintain the line break while assigning the multiline data to the content variable.
    
    Reloading SystemD – Once the service file is created and placed on the appropriate /etc/systemd directory, we need 
    to reload the system daemon to sync the changes.  Ansible has a dedicated module named systemd for to interact with SystemDaemon
    
    Enabling the tomcat to Auto-Start – We are enabling the tomcat service we just created, for the boot time auto-start. 
    this is done using systemd module too
    
    Connect to the tomcat web interface and validate – we are using the uri module to connect to 
    the http://localhost:8080 to ensure that the tomcat is started and running fine.  we are also using until and retry 
    to wait for the server to come up
