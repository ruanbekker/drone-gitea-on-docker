# drone-gitea-on-docker
DroneCI and Gitea on Docker

## Usage

Set the following in your `boot.sh`:

```
IP_ADDRESS=192.168.0.6       -> either reachable dns or ip address which will be your clone address and ui addresses.
GITEA_ADMIN_USER="giteauser" -> will be the user you register with in drone
```

Now boot the stack:

```
$ bash boot.sh
```

Head over to: `http://${IP_ADDRESS}:3000/user/settings/applications` and create a new OAuth2 Application and set the Redirect URI to `http://${IP_ADDRESS}:3001/login`

Capture the client id and client secret and populate them in the `boot.sh` in `DRONE_GITEA_CLIENT_ID` and `DRONE_GITEA_CLIENT_SECRET` and run `bash boot.sh` again. This will give drone the correct credentials in order to authenticate with gitea.

Now when you head over to http://${IP_ADDRESS}:3001/ you will be asked to authorize the application and you should be able to access drone.

## Drone CLI

Install Drone CLI:
- https://docs.drone.io/cli/install/

```
$ curl -L https://github.com/drone/drone-cli/releases/latest/download/drone_darwin_amd64.tar.gz | tar zx
$ sudo mv drone /usr/local/bin/drone
$ chmod +x /usr/local/bin/drone
```

Get your Drone Token:
- http://${IP_ADDRESS}:3001/account

```
$ export DRONE_SERVER=http://${IP_ADDRESS}:3001
$ export DRONE_TOKEN=one-from-the-account-page
drone info
```

## Build your first pipeline

Create a test repo in gitea:

![image](https://user-images.githubusercontent.com/567298/110296470-0ad23800-7ffb-11eb-8428-af49d0ebd62d.png)

Commit a `.drone.yml` file for drone:

```
$ cat .drone.yml
kind: pipeline
type: docker
name: hello-world

trigger:
  branch:
    - master
  event:
    - push

steps:
  - name: say-hello
    image: busybox
    commands:
      - echo hello-world
```

Head over to drone and sync your repositories:

![image](https://user-images.githubusercontent.com/567298/110296425-00b03980-7ffb-11eb-9216-76725a62c09e.png)

Activate your repository:

![image](https://user-images.githubusercontent.com/567298/110296623-3523f580-7ffb-11eb-805f-db5db4dab0cb.png)

Push a commit to master and see your pipeline running:

![image](https://user-images.githubusercontent.com/567298/110296747-584ea500-7ffb-11eb-9909-259641a663aa.png)

## More Examples

- https://github.com/ruanbekker/drone-ci-testing
- https://github.com/ruanbekker/drone-demo-python-flask
- https://github.com/ruanbekker/drone-with-go
- https://github.com/ruanbekker/demo-drone-mongodb-tests
- https://github.com/ruanbekker/drone-multi-pipeline
- https://github.com/ruanbekker/docker-jekyll-drone
