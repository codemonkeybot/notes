# Deploying Cloud Foundry via Bosh Lite in VirtualBox

## Install Bosh director

### Requirements

* Install [Bosh CLI](http://bosh.io/docs/cli-v2/#install)
* Install VirtualBox

### Install the Director VM

Clone the bosh deployment repo:

```
git clone https://github.com/cloudfoundry/bosh-deployment ~/workspace/bosh-deployment

mkdir -p ~/deployments/vbox

cd ~/deployments/vbox
```

### Configure VM specs

Configure the CPU and Memory for the VirtualBox VM. The default values are 2 CPUs and 4GB memory but for a development version of Cloud Foundry, we recommend to increase to 4 CPUs and 8GB memory.

Edit the `virtualbox/cpi.yml` under `~/workspace/bosh-deployment` folder and increase the `cpus` and `memory` values:

```
# Configure sizes
- type: replace
  path: /resource_pools/name=vms/cloud_properties?
  value:
    cpus: 4
    memory: 8192
    ephemeral_disk: 16_384
```

### Bosh environment

Create the bosh environment:

```
bosh create-env ~/workspace/bosh-deployment/bosh.yml \
--state ./state.json \
-o ~/workspace/bosh-deployment/virtualbox/cpi.yml \
-o ~/workspace/bosh-deployment/virtualbox/outbound-network.yml \
-o ~/workspace/bosh-deployment/bosh-lite.yml \
-o ~/workspace/bosh-deployment/bosh-lite-runc.yml \
-o ~/workspace/bosh-deployment/uaa.yml \
-o ~/workspace/bosh-deployment/credhub.yml \
-o ~/workspace/bosh-deployment/jumpbox-user.yml \
--vars-store ./creds.yml \
-v director_name=bosh-lite \
-v internal_ip=192.168.50.6 \
-v internal_gw=192.168.50.1 \
-v internal_cidr=192.168.50.0/24 \
-v outbound_network_name=NatNetwork
```

### Login to Bosh Director

Configure the enviroment and login to the Bosh director:

```
bosh alias-env vbox -e 192.168.50.6 --ca-cert <(bosh int ./creds.yml --path /director_ssl/ca)
export BOSH_ENVIRONMENT=vbox
export BOSH_CLIENT=admin
export BOSH_CLIENT_SECRET=`bosh int ./creds.yml --path /admin_password`
```

### Setup local routes

Setup a local route for bosh ssh commands or accessing VMs directly:

```
sudo route add -net 10.244.0.0/16 gw 192.168.50.6 # Linux
sudo route add -net 10.244.0.0/16     192.168.50.6 # Mac OS X
```

### Delete Bosh environment.

In case you need to restart the installation process or delete the Bosh director. You need to run this script from the `~/deployments/vbox/` folder.

```
bosh delete-env ~/workspace/bosh-deployment/bosh.yml \
--state ./state.json \
-o ~/workspace/bosh-deployment/virtualbox/cpi.yml \
-o ~/workspace/bosh-deployment/virtualbox/outbound-network.yml \
-o ~/workspace/bosh-deployment/bosh-lite.yml \
-o ~/workspace/bosh-deployment/bosh-lite-runc.yml \
-o ~/workspace/bosh-deployment/uaa.yml \
-o ~/workspace/bosh-deployment/credhub.yml \
-o ~/workspace/bosh-deployment/jumpbox-user.yml \
--vars-store ./creds.yml \
-v director_name=bosh-lite \
-v internal_ip=192.168.50.6 \
-v internal_gw=192.168.50.1 \
-v internal_cidr=192.168.50.0/24 \
-v outbound_network_name=NatNetwork
```

Then, remove the IP routes:

```
sudo route del -net 10.244.0.0/16 gw 192.168.50.6 # Linux
sudo route delete -net 10.244.0.0/16  192.168.50.6 # Mac OS X
```
> To view the existing `routes` in Linux use: `route -n` and Mac OS: `sudo netstat -rn`

## Install Cloud Foundry

### Download the CF project install

Clone the CF deployment repo:

```
git clone https://github.com/cloudfoundry/cf-deployment.git ~/workspace/cf-deployment

cd ~/workspace/cf-deployment
```

### Configure the Bosh blobstore

Cloud Foundry needs a version of the BOSH Lite Warden stemcell in order to build the solution.  To upload this stemcell into the Blobstore, run the following commands:

```
export STEMCELL_VERSION=$(bosh int cf-deployment.yml --path /stemcells/alias=default/version)

bosh upload-stemcell https://bosh.io/d/stemcells/bosh-warden-boshlite-ubuntu-trusty-go_agent?v=$STEMCELL_VERSION
```

### Setup the cloud config

```
yes | bosh update-cloud-config iaas-support/bosh-lite/cloud-config.yml
```

### Build Cloud Foundry

```
yes | bosh -d cf deploy ~/workspace/cf-deployment/cf-deployment.yml \
-o ~/workspace/cf-deployment/operations/bosh-lite.yml \
-o ~/workspace/cf-deployment/operations/use-compiled-releases.yml \
--vars-store ~/deployments/vbox/deployment-vars.yml \
-v system_domain=bosh-lite.com
```

> You can enable debug mode in the Bosh director by exporting the following variable: `export BOSH_LOG_LEVEL=debug`

> *We are using the [bosh-lite.com](http://bosh-lite.com) domain to route the CF apps to a public DNS domain so we can access the apps publicly*