# .NET Core hosted on Plesk Linux

When using Plesk Onyx on Linux you can use Docker for your .net core projects, but with this trick you can use one Docker image and host as many .net core projects just by uploading your projects with FTP.

Plesk Onyx 17.5.3 does not have native .net core support which is very sad considering that running .net on linux hosts is much cheaper and possible with .net core.

You can host Dockers with Plesk but currently there are some issues uploading Docker containers, so most of the time you have to publish them to the public Docker Hub which is not ideal. Also there is a strange behavior that updating your running Docker container just does not work, resulting to have a new container installed instead of updating.

I found a way to host your .net core projects just by uploading them via FTP, but hosting the .net core runtime in a docker container. This by re-using the same container again, and using folder mapping. There is only one special thing to consider, and that is that your output assembly should always have the same name.
