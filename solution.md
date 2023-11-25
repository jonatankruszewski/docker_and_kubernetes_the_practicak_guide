SOLUTION

```sh
# inside each folder
docker build .
#copy id
docker run -it pyhonContainer
docker run -p 3000:3000 nodeContainer

#re run naming
docker run -it --name pythonTest pythonContainerId
docker run -p 3001:3000 --name nodeTest nodeContainerId

#stopping them
docker stop nodeExam pythonBMI

#starting them
docker start nodeExam pythonBMI
docker atttach pythonBMI

# or start them separately and to the pythonBMI restart attached:
docker start -ai pythonBMI

#stop all containers
docker stop $(docker ps -a -q)

# remove those containers and any other that might had been created using those images
docker rm nodeExam pythonBMI

# now I can remove the images
docker rmi imageID1 imageID2
# or docker image prune

#re build with name
docker build . -t node_app_test
docker build . -t python_test

# spin containers
docker run -p 3000:3000 -d --rm --name node_app_container node_app_test
docker run -it --rm --name python_app_container python_test

docker stop python_app_container node_app_container

```
