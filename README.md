## IntelliJ Docker Plugin Setup
### Add Docker Integration Plugin to IntelliJ
1. Install Docker Integration Plugin to IntelliJ as explained here https://www.jetbrains.com/help/idea/docker.html
2. Restart IntelliJ

### Configure the Docker daemon connection settings
1. Go to Settings/Preferences dialog, click Docker under Build, Execution, Deployment.
2. Add a Docker configuration (The Add (+) button) and specify how to connect to the Docker daemon. I was running on Mac, and selected the `Docker for Mac option`
3. I left the Path mappings with default /Users option
4. The connection settings depend on your Docker version and operating system. For more information, check `Enable Docker Support` section here: https://www.jetbrains.com/help/idea/docker.html
5. If it is set correct, you will see a `Connection Successful` message at the bottom of the dialog.

### Check/Test the Docker Tool Window.
1. Go to your project, and open the `Docker Tool Window` from menu `View -> Tool Windows -> Docker`
2. You can see Containers and Images in your local Docker in this tool window. You can also Start/Stop Docker Plugin from this tool window. https://screencast.com/t/jgi8SreiA
3. You can create/start/Stop/Delete Containers and Images from here.

### Update Docker File Build Configuration
1. Make sure you add this to existing/new JAVA_OPTS ENV variable:
   `-Xdebug -Xrunjdwp:transport=dt_socket,address=5005,server=y,suspend=y`
Ex: I dynamically build Dockerfile with build.sbt. So, added this to  build.sbtas shown here: https://screencast.com/t/bMBecRUvoJG


### Build your docker image
1. I am building my project with SBT. So, `sbt clean docker` will create a docker image. Click on the image in Docker Tool Window and  Note the Image tag name to use while setting up run config next. (text version, as it will remain same for every build)

### Create Run Configuration for the application (Docker) 
          https://screencast.com/t/InY8jhjO
1. Add a New Run Configuration (Run -> Edit Configuration -> Add(+); Select Docker). https://screencast.com/t/L7JKpwnUE
2. Add Image tage notes above to the `Image ID` field in the dialog.
3. Give a static Container Name in the dialog (Ex: app-debug). 
4. Bind any app exposed ports to the host. Also a port that will be used by the debugger. Ex: In my akka application, I have 2559 port exposed. So, I added two 2559:2559 and 5005:5005
5. if your app needs any environment variables add that to the dialog. 
6. you can preview your docker command for this run configuration from `Command Preview`
7. Save the Configuration (Apply/OK)
 
### Create a Remote Debugging Configuration


```

# gdfgdg
gfdgdfg

# gfggs
fggfdgfdfdg
 
```
