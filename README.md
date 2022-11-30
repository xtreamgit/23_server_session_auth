# 23_server_session_auth

The source for this repo is: Server Side Session Authentication | Go with Vue.js Authentication
from: NerdCademy

https://www.youtube.com/watch?v=Ck919fGGbCw&t=122s

WARNING: The video provides the wrong information in a couple of places. The first one is on 2:44 in which he uses a docker run command that doesn't work with the rest of the setup in the main.go. The second is that he uses the 172.17.0.2 address in the db.go. The value should be localhost instead. 

For all this to work you need to use this docker run command:

docker run --name auth-psql -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=test -p 5432:5432 -d postgres:14

The model/db.go line 14 should be:

dsn := "host=localhost port=5432 user=admin password=test dbname=admin sslmode=disable"

He uses the docker container's private IP address instead which is incorrect. You can't access a container's private IP from your host environment directly. 

For the main.go app to run correctly, the docker ps should look like this:
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS          PORTS                    NAMES
a5736ac85664   postgres:14   "docker-entrypoint.s…"   47 minutes ago   Up 47 minutes   0.0.0.0:5432->5432/tcp   auth-psql

Notice that the ports column shows 0.0.0.0 mapped to the external port 5432 which is then mapped to the containers internal port 5432. 

In addition to that, he fails to mention that the packages and modules must match the current environment. As a newbie, I need to know this type of information. These details are crucial if you don't want to waste days trying to troubleshoot something that could be solved in 30 seconds. To his credit, he created a video that explains packages and modules with go. 

Okay >>>>>>>> 

After we are set and done, the go.mod should look like this:

module github.com/xtreamgit/23_server_session_auth
go 1.19
------------------------------------------------------------------------
Notice that the url for the module is my github.com repo (xtreamgit/23_server_session_auth).

The main.go file should look like this:

package main

import (
	"github.com/xtreamgit/23_server_session_auth/model"
	// "github.com/xtreamgit/23_server_session_auth/routes"
)

func main() {
	model.Setup()
	// routes.Setup()
}

---------------------------------------------------------------------------

Notice that the import command also uses my repo's URL and the folder name (model).

The two commented lines allows me to test the main.go -> model.Setup() function separately.

This document will be updated as needed. 
BTW: This repo will be recreated in a public repo under a different name. 




