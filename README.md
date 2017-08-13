# .NET Core hosted on Plesk Linux

When using Plesk Onyx on Linux you can use Docker for your .net core projects, but with this trick you can use one Docker image and host as many .net core projects just by uploading your projects with FTP.

Plesk Onyx 17.5.3 does not have native .net core support which is very sad considering that running .net on linux hosts is much cheaper and possible with .net core.

You can host Dockers with Plesk but currently there are some issues uploading Docker containers, so most of the time you have to publish them to the public Docker Hub which is not ideal. Also there is a strange behavior that updating your running Docker container just does not work, resulting to have a new container installed instead of updating.

I found a way to host your .net core projects just by uploading them via FTP, but hosting the .net core runtime in a docker container. This by re-using the same container again, and using folder mapping. There is only one special thing to consider, and that is that your output assembly should always have the same name.

## Building the container

To build the .net core application for publishing:

```dotnet publish -o:./published```

```
docker build -t dotnetcoreplesk .
docker tag dotnetcoreplesk:latest stefandevo/dotnetcoreplesk:1.0
docker push stefandevo/dotnetcoreplesk:1.0
```

## Uploading container to Plesk

In your Plesk (Web Host Edition) admin panel go to the Docker screen. Next perform a searh for ```dotnetcoreplesk``` and choose the version you want ```1.0.``` and Install. 
Give your container the name of your app you want to install, f.e. **HelloDotNetCore**.
Next remove the check for **Automatic Port Mapping**. You will see that you now get the exposed port 5000; map this to an external port of your choice, f.e. **34000**.
Then, set your **Volume mapping**. For Destination you should add ```/published```, for Source you should point to the location for the website that you have created. ```/var/www/vhosts/{{ YOUR SUBSCRIBER HOSTNAME }}/{{ YOUR DOMAIN }}```. Make sure that the content is cleared before doing this.

You can now start uploading your own published .net core project with FTP in the folder you specified above. 

Next thing to do, is add your Docker container to the Website. You do this by choosing **Docker Proxy Rules** on the chosen website, **Add Rule**, next choose your container; normally the last added container is selected, and also the port mapping is correct (5000 -> 34000 for example).

Once you have done this, you can start the container and your uploaded files will be hosted.

**If you want to update the project, just copy over all files with FTP, and then **Restart** the container for make the changes to take effect.**


