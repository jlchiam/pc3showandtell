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

git clone https://github.com/u1i/docker-hello




