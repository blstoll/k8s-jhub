### Sample Kubernetes Cluster Running Deployed via Vagrant

# Installing Ansible on OSX
```bash
sudo -H pip install ansible
```

## Deploying the Cluster
```bash
vagrant up
vagrant ssh_config >> ~/.ssh/config
ansible-playbook site.yml
```

## Installing Jupyter-Hub

#### Update the Secret Token in ~/pods/jhub/config/yaml
Generate a new token:
```bash
ssh jhub-master
openssl rand -hex 32
```
Update the proxy.secretToken value in ~/pods/jhub/config.yaml 

#### Deploy the application
```bash
ssh jhub-master
helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/
helm repo update
helm upgrade --install jhub jupyterhub/jupyterhub \
  --namespace jhub \
  --version=0.8.0 \
  --values ~/pods/jhub/config.yaml
```
