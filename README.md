# watchface-container
This project helps AsteroidOS watchface developers create and use a software container
that simplifies the process of modifying, creating, testing, and deploying watchfaces.

## How to build it
This example uses `podman` but the command is identical if one uses `docker`:
```
podman build -t watchface https://github.com/beroset/watchface-container.git
```

This automatically builds the container from the github repository, so there is no need to
clone the repository if not needed.

## How to use it
Using `podman`, as above, after the container, named `watchface` is created
one can use the `tryme` shell script to set up all of the environment variables and
shared volumes to allow one to use the container.

By default, it will show a gui menu, but one can also invoke it with command line arguments.

For example, create and navigate to an empty directory and issue this command:

```
mkdir test
cd test
../tryme watchface clone decimal-time decimal2
```

That will clone the `decimal-time` watchface as `decimal2` and put the files in your local
directory.

We can then test the new watch face:

```
../tryme watchface test decimal2
```

### Using `adb` via USB
You need to have `adb` installed on the host computer, since that is where the server
runs that actually connects to your watch. Refer to `tryme-adb` on how to start the 
container with the ability to connect to the host's adb server. Continuing the example 
from above, you can use it like this to deploy the cloned and potentially edited 
`decimal2` watchface to your watch (still inside the `test` directory):

```
../tryme-adb watchface -a deploy decimal2 
```

***Note:** This (re-)starts `adb` with the `-a` option, causing it to listen on all network
interfaces, not just `localhost`. As this enables any device in your local network to
communicate with your adb server, you should only do this if you know and trust all
devices in your local network.*


## About wallpaper
By default, the container will look for two files "background.jpg" and "background-round.jpg" to use as 
wallpaper behind the watchface for both testing and for creating thumbnail images for new watchfaces.
If these don't exist, the wallpaper will just be a boring black screen.  It's recommended to create 
at least those two, perhaps copying something pleasing from the AsteroidOS [default wallpapers](https://github.com/AsteroidOS/asteroid-wallpapers)
or creating your own.
