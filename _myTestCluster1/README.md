# Quick Setup
## Pre deployment
#### See the Prerequisities [here](https://github.com/kube-hetzner/terraform-hcloud-kube-hetzner#%EF%B8%8F-prerequisites)

Then run the following commands substituting the `<CAPITAL_VALUES_SOROUNDEDBY_left-right arrows>`

```bash
# Create a new directory
mkdir   ../_myTestCluster2
cd      ../_myTestCluster2

# Get the latest 'kube.tf.example' (if you don't have a cutomized one already)
curl -sL https://raw.githubusercontent.com/kube-hetzner/terraform-hcloud-kube-hetzner/master/kube.tf.example -o kube.tf

# Get the latest 'hcloud-microos-snapshots.pkr.hcl'.
curl -sL https://raw.githubusercontent.com/kube-hetzner/terraform-hcloud-kube-hetzner/master/packer-template/hcloud-microos-snapshots.pkr.hcl -o hcloud-microos-snapshots.pkr.hcl

# Export the Hetzner cloud token for both `packer` & `terraform`
export HCLOUD_TOKEN="<YOUR_HCLOUD_TOKEN>"
export TF_VAR_hcloud_token='<YOUR_HCLOUD_TOKEN>'

# Run the `packer` - this will create relevant snapshots in Hetzner to be used for the control-plane and worker nodes.
packer init hcloud-microos-snapshots.pkr.hcl
packer build hcloud-microos-snapshots.pkr.hcl

# Create a relevant context to your project
hcloud context create <PROJECT-NAME>
```

## Notes
You will find your `kube.tf` file and it must be customized to suit your needs - Or you may use the one in the `_myTestCluster1/` directory for a very basic and quick deployment

It is required to generate new ssh-key pair and update the relevant fields in the `kube.tf` to point to those files

### Caution
In the `_myTestCluster1/` directory and its `kube.tf` file, there are already created and exposed an experimental temporary ssh-key pair ONLY FOR TESTING THE CODE and termintaing the environment ASAP!
* `experimental-test.pem` & `experimental-test.pem.pub`

###### Source: https://github.com/kube-hetzner/terraform-hcloud-kube-hetzner#getting-started

---

## Deployment
In the previously created project folder that contains your customized `kube.tf` run the following Terraform commands:

```bash
terraform init --upgrade
terraform validate
terraform apply -auto-approve
```

### Usage
Please find the auto generated kubernetes configuration YAML file with `ls -l *kubeconfig.yaml` in your project directory and run the following command to configure `kubectl` use that config file for authenticating and intracting with the newly deployed cluster.

* `export KUBECONFIG=./k3s_kubeconfig.yaml`

###### Surce: https://github.com/kube-hetzner/terraform-hcloud-kube-hetzner#-installation

---