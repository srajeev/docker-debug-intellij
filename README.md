## IntelliJ Docker Plugin Setup

### Step1: Add Docker Integration Plugin to IntelliJ
1. Install Docker Integration Plugin to IntelliJ as explained here https://www.jetbrains.com/help/idea/docker.html
2. Restart IntelliJ

### Step2: Configure the Docker daemon connection settings
https://github.com/srajeev/docker-debug-intellij/blob/master/IntelliJPrefsDocker.png
1. Go to Settings/Preferences dialog, click Docker under Build, Execution, Deployment.
2. Add a Docker configuration (The Add (+) button) and specify how to connect to the Docker daemon. I was running on Mac, and selected the `Docker for Mac option`
3. I left the Path mappings with default /Users option
4. The connection settings depend on your Docker version and operating system. For more information, check `Enable Docker Support` section here: https://www.jetbrains.com/help/idea/docker.html
5. If it is set correct, you will see a `Connection Successful` message at the bottom of the dialog.

### Step3: Check/Test the Docker Tool Window.
     https://github.com/srajeev/docker-debug-intellij/blob/master/DTW.png
1. Go to your project, and open the `Docker Tool Window` from menu `View -> Tool Windows -> Docker`
2. You can see Containers and Images in your local Docker in this tool window. You can also Start/Stop Docker Plugin from this tool window. 
3. You can create/start/Stop/Delete Containers and Images from here.

### Step4: Update Docker File Build Configuration
         https://github.com/srajeev/docker-debug-intellij/blob/master/buildsbt.png
Make sure you add this to existing/new JAVA_OPTS ENV variable:`-Xdebug -Xrunjdwp:transport=dt_socket,address=5005,server=y,suspend=y`
Ex: I dynamically build Dockerfile with build.sbt. So, added this to  build.sbtas shown here: 


### Step5: Build your docker image
I am showing example with SBT build here. So, `sbt clean docker` will create a docker image. Click on the image in Docker Tool Window and  Note the Image tag name to use while setting up run config next. (Note the text version, as it will remain same for every build)

### Step6: Create Run Configuration for the application (Docker) 
      https://github.com/srajeev/docker-debug-intellij/blob/master/DockerRunConfig.png
1. Add a New Run Configuration (Run -> Edit Configuration -> Add(+); Select `Docker`).
2. Add Image tage notes above to the `Image ID` field in the dialog.
3. Give a static Container Name in the dialog (Ex: app-debug). 
4. Bind any app exposed ports to the host. Also a port that will be used by the debugger. Ex: In my akka application, I have 2559 port exposed. So, I added two 2559:2559 and 5005:5005
5. if your app needs any environment variables add that to the dialog. 
6. you can preview your docker command for this run configuration from `Command Preview`
7. Save the Configuration (Apply/OK)
 
### Step7: Create a Remote Debugging Configuration
     https://github.com/srajeev/docker-debug-intellij/blob/master/remoteDebug.png
1. Add a New Run Configuration (Run -> Edit Configuration -> Add(+); Select `Remote`).
2. It pre-populates command line args. But of you want to change the debug port from default 5005, you can change that where `Port:`  field is.
3. Select your project for the module's classpath.
4. Save the Configuration (Apply/OK)

## Run/Debug
1. Put some breakpoints in the code from IntelliJ
2. Run the Docker Run Configuration created above in Step 6. It will start and wait with message "`Listening for transport dt_socket at address: 5005`
3. Now Run the Remote Debugging configuration created above in Step 7. It will start and show this on console `Connected to the target VM, address: 'localhost:5005', transport: 'socket'`
4. Now the application continues through its startup (can see on Docker Tool Window logs). 
5. Testing application to hit debug breakpoints should hit them, and from there on you will have all debugging features like Step Into, Step Over, Step Out, Resume etc.


### References
1. https://www.jetbrains.com/help/idea/docker.html
2. https://blog.jetbrains.com/idea/2017/11/what-does-intellij-idea-2017-3-have-in-store-for-docker-support/
3. https://docs.oracle.com/javase/6/docs/technotes/guides/jpda/architecture.html
4. https://docs.oracle.com/javase/6/docs/technotes/guides/jpda/jpda.html#walk-through
5. https://stackoverflow.com/questions/3591497/java-remote-debugging-how-does-it-work-technically
6. https://www.youtube.com/watch?v=sz5Zv5QQ5ek




