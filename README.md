# pc3showandtell Docker Container

I have not really figured out what I wanted to do ... preferably something Fintech related from the Docker Hub marketplace.

Run with:

docker run -d -p 8080:8080 u1ih/hello
- The docker run command first creates a writeable container layer over the specified image, and then starts it using the specified command. That is, docker run is equivalent to the API /containers/create then /containers/(id)/start. A stopped container can be restarted with all its previous changes intact using docker start. See docker ps -a to view a list of all containers.
- --detach , -d		Run container in background and print container ID
- --publish , -p		Publish a container's port(s) to the host
basically means to run the Docker Hub image Uli created for Hello World on port 8080. It will be replaced with whatever I want to do that I can find.


Steps:
# 1. Get Source

> git clone https://github.com/u1i/docker-hello

# 2. Make Changes
From Google Cloud Shell Editor, open up docker-hello/app/index.html and make some changes.

# 3: Build Container
change into the directory: cd docker-hello

docker build . -t myhello
- Build an image from a Dockerfile
- When the URL parameter points to the location of a Git repository, the repository acts as the build context. The system recursively fetches the repository and its submodules. The commit history is not preserved. A repository is first pulled into a temporary directory on your local host. After that succeeds, the directory is sent to the Docker daemon as the context. Local copy gives you the ability to access private repositories using local user credentials, VPN’s, and so forth.
- --tag , -t		Name and optionally a tag in the 'name:tag' format

Docker will call the API to Docker Hub to get the image to build.

# 4: Run Containter
docker run -d -p 8080:8080 myhello

# 5: View the changes
From Google Cloud Shell, somewhere at the upper right hand side, click the "web preview" icon to see the changes.


