+++
title = "Project and cluster management"
date = 2021-02-10T12:07:15+02:00
weight = 10

+++

## Project Management

### Create a New Project

Clusters are assigned to projects, so in order to create a cluster, you must create a project first. In the Kubermatic Kubernetes Platform (KKP) dashboard, choose `Add Project`:

![Project List](/img/kubermatic/v2.18/tutorials/projects_overview.png?classes=shadow,border "Project List")

Assign your new project a name:

![Add Project Dialog](/img/kubermatic/v2.18/tutorials/project_add.png?classes=shadow,border "Add Project Dialog")

You can assign key-label pairs to your projects. These will be inherited by the clusters and cluster nodes in this project. You can assign multiple key-label pairs to a project.

After you click `Save`, the project will be created. If you click on it now, you will see options for adding clusters, managing project members, service accounts and SSH keys.


### Delete a Project

To delete a project, move the cursor over the line with the project name and click the trash bucket icon.

![Delete Project](/img/kubermatic/v2.18/tutorials/project_delete.png?classes=shadow,border "Delete Project")


### Add an SSH key

If you want to ssh into the project VMs, you need to provide your SSH public key. SSH keys are tied to a project. During cluster creation you can choose which SSH keys should be added to nodes. To add an SSH key, navigate to `SSH Keys` in the Dashboard and click on `Add SSH Key`:

![SSH Key List](/img/kubermatic/v2.18/tutorials/sshkeys_overview.png?classes=shadow,border "SSH Key List")

This will create a pop up. Enter a unique name and paste the complete content of the SSH key into the respective field:

![Add SSH Key Dialog](/img/kubermatic/v2.18/tutorials/sshkeys_add_dialog.png?classes=shadow,border "Add SSH Key Dialog")

After you click on `Add SSH key`, your key will be created and you can now add it to clusters in the same project.


## Manage clusters

### Create cluster

To create a new cluster, open the Kubermatic Kubernetes Platform (KKP) dashboard, choose a project, select the menu entry `Clusters` and click the button `Create Cluster` on the top right.

![Cluster List](/img/kubermatic/v2.18/tutorials/cluster_list.png?classes=shadow,border "Cluster List")

Choose the cloud provider and the datacenter:

![Menu to choose Cloud Provider](/img/kubermatic/v2.18/tutorials/wizard_step_1.png?classes=shadow,border "Menu to choose Cloud Provider")

Enter a name for your cluster and click Next. Here you can also activate [Audit Logging](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/) and [Pod Security Policy](https://kubernetes.io/docs/concepts/policy/pod-security-policy/), assign labels and SSH keys to your cluster, and choose a Kubernetes version.

**Note:**
Disabling the User SSH Key Agent at this point can not be reverted after the cluster creation, which means that ssh key management after creation for this cluster will have to be done manually. More info in [`User SSH Key Agent`]({{< ref "../../tutorials_howtos/administration/user_settings/user_ssh_key_agent/_index.en.md" >}}) 

![General Cluster Settings](/img/kubermatic/v2.18/tutorials/wizard_step_2.png?classes=shadow,border "General Cluster Settings")


In the next step of the installer, enter the credentials for the chosen provider. A good option is to use Kubermatic Presets here, which is `loodse` in this case, instead putting in credentials for every cluster creation:

![Provider Credentials](/img/kubermatic/v2.18/tutorials/wizard_step_3.png?classes=shadow,border "Provider Credentials")

In the Initial nodes section of the wizard you can setup the size, settings and amount of nodes your cluster will start with. Also you can assign labels to your nodes. You can also set [node taints](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) which is a property that allows your node to repel certain pods.

![Node Settings](/img/kubermatic/v2.18/tutorials/wizard_step_4.png?classes=shadow,border "Node Settings")

Lastly, a cluster summary screen will be presented so that you can check out if everything looks ok.

![Cluster Details as a Summary](/img/kubermatic/v2.18/tutorials/wizard_step_5.png?classes=shadow,border "Cluster Details as a Summary")

You will then be forwarded to the cluster page where you can view the cluster creation process:

![Cluster Details in Creation State](/img/kubermatic/v2.18/tutorials/cluster_details_after_creation.png?classes=shadow,border "Cluster Details in Creation State")

After all of the master components are ready, your cluster will create the configured number of worker nodes. Fully created nodes will be marked with a green dot, pending ones with a yellow circle. Clicking on the download icon lets you download the kubeconfig to be able to use `kubectl` with your cluster.

![Cluster Created](/img/kubermatic/v2.18/tutorials/cluster_details_overview.png?classes=shadow,border "Cluster Created")

### Upgrade Cluster

When an upgrade for the cluster is available, a little dropdown arrow will be shown besides the Master Version on the cluster's page:

![Cluster with available Upgrade](/img/kubermatic/v2.18/tutorials/upgrade_version.png?classes=shadow,border "Cluster with available Upgrade")

To start the upgrade, just click on the link and choose the desired version:

![Dialog to choose Upgrade Version](/img/kubermatic/v2.18/tutorials/change_version.png?classes=shadow,border "Dialog to choose Upgrade Version")

After the update is initiated, the master components will be upgraded in the background. Check the checkbox for `Upgrade Machine Deployments` if you wish to upgrade the existing worker nodes as well.

### Edit Cluster

Clusters can be edited by pressing the ellipsis button on the right and then select `Edit Cluster`:

![Select Edit Cluster](/img/kubermatic/v2.18/tutorials/cluster_edit_menu.png?classes=shadow,border "Select Edit Cluster")

![Edit Cluster Dialog](/img/kubermatic/v2.18/tutorials/edit_cluster_dialog.png?classes=shadow,border "Edit Cluster Dialog")

### Delete Cluster

To delete a cluster, navigate to `Clusters` and choose the cluster that you would like to delete. On the top left is a button `Delete`:

![Cluster Deletion Button in the top right corner](/img/kubermatic/v2.18/tutorials/delete_cluster_button.png?classes=shadow,border "Cluster Deletion Button in the top right corner")

To confirm the deletion, type the name of the cluster into the text box:

![Confirmation dialog for Cluster Deletion](/img/kubermatic/v2.18/tutorials/delete_cluster.png?classes=shadow,border "Confirmation dialog for Cluster Deletion")

The cluster will switch into deletion state afterwards, and will be removed from the list when the deletion succeeds.


## Add a New Machine Deployment

To add a new machine deployment navigate to your cluster view and click on the `Add Machine Deployment` button:

![Cluster overview with highlighted add button](/img/kubermatic/v2.18/tutorials/add_machine_deployment.png?classes=shadow,border "Cluster overview with highlighted add button")

In the popup you can then choose the number of nodes (replicas), kubelet version, etc for your newly created machine deployment. All nodes created in this machine deployment will have the chosen settings.

## Edit the Machine Deployment

To add or delete a worker node you can easily edit the machine deployment in your cluster. Navigate to the cluster overview, scroll down to `Machine Deployments` and click on the edit icon next to the machine deployment you want to edit:

![Machine deployment overview with highlighted edit button](/img/kubermatic/v2.18/tutorials/machine_deployment_edit.png?classes=shadow,border "Machine deployment overview with highlighted edit button")

In the popup dialog you can now in- or decrease the number of worker nodes which are managed by this machine deployment, as well as their operating system, used image etc.:

![Machine deployment overview with opened edit modal](/img/kubermatic/v2.18/tutorials/machine_deployment_edit_dialog.png?classes=shadow,border "Machine deployment overview with opened edit modal")
