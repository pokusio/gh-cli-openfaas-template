# An `OpenFAAS` Github CLI Template

## How to use

* The Github CLI template makes use of one secret : a `Github Personal Access Token`. So :
  * create a Github personal access token
  * create the `OpenFAAS` secret :

```bash
export GH_PERSONAL_ACCESS_TOKEN="ghp_VUnUwKli0kTnMJ0D6kNcjuQEvvpTVYmA3NuJEC"

echo "${GH_PERSONAL_ACCESS_TOKEN}" | faas-cli secret create pokusbot-gh-token
# cat ~/Downloads/derek.pem | faas-cli secret create pokusbot-gh-token
```

* Create a new FAAS function using the github cli template :

```bash
export FAAS_FUNC_NAME="my-new-awesome-function"
export FAAS_TEMPLATE_NAME="pokus-gh-cli-node"

faas-cli template pull https://github.com/pokusio/gh-cli-openfaas-template
faas-cli new --lang "${FAAS_TEMPLATE_NAME}" "${FAAS_FUNC_NAME}"
# will generate the [my-new-awesome-function.yml].
```

* edit the `my-new-awesome-function.yml`, and append this to it :

```Yaml
secrets:
  - pokusbot-gh-token
```

* build n deploy your function :

```bash
# Below is a local ip address :
# OpenFAAS : I locally run k3d with 3 servers n 3 agents
# Docker Registry : I locally run docker-compose
export DOCKHOST_IP_ADDR="192.168.208.7"
# "OF_TEMPLATE_IMAGE_NAME"  must be same image name as the
# image property in the [<name of yoour faas function>.yml]
export OF_TEMPLATE_IMAGE_NAME="${DOCKHOST_IP_ADDR}:5000/pokus/faas-gh-cli-node16:latest"

faas-cli up --build-arg AWESOME=true --image "${OF_TEMPLATE_IMAGE_NAME}" -f my-new-awesome-function.yml ${HERAOHERE}/wehereiwork/my-new-awesome-function/handler.js

```

* test your new github cli augmented faas function :

```bash
# Below is a local ip address :
# OpenFAAS : I locally run k3d with 3 servers n 3 agents
# Docker Registry : I locally run docker-compose
export DOCKHOST_IP_ADDR="192.168.208.7"
curl -X POST http://127.0.0.1:8080/function/my-new-awesome-function \
  -H "Content-Type: application/json" \
  -d '{ "url": "https://randomuser.me/api/", "name": "pokustest"}'

curl -X POST http://${DOCKHOST_IP_ADDR}:8080/function/my-new-awesome-function \
  -H "Content-Type: application/json" \
  -d '{ "url": "https://randomuser.me/api/", "name": "pokustest"}'

```

## How to spin up a new OpenFAAS template

https://github.com/pokusio/gh-cli-openfaas-template

test-using-template

## ANNEX. References

https://github.com/openfaas/faas-cli/issues/420
