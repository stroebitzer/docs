+++
title = "Automatic Etcd Backups and Restore"
date = 2021-09-16T12:07:15+02:00
weight = 20

+++

Through KKP you can set up automatic scheduled etcd backups for your user clusters, and easily restore etcd to its previous state.

Firstly, you need to enable and configure the backup bucket, endpoint and credentials. To see how, check [Backup Buckets Settings]({{< ref "../administration/admin_panel/backup_buckets" >}}) 

## Etcd Backups

By default a new backup configuration resource is created in the cluster namespace for each user cluster once the controllers are enabled.
The new backup configuration resource is created with a default of `@every 20m` interval and `20` backup retention.

However, it's possible to create additional backup configuration if needed, simply by creating a backup configuration resource:

```yaml
apiVersion: kubermatic.k8s.io/v1
kind: EtcdBackupConfig
metadata:
  name: daily-backup
  namespace: cluster-zvc78xnnz7
spec:
  cluster:
    apiVersion: kubermatic.k8s.io/v1
    kind: Cluster
    name: zvc78xnnz7
  schedule: '@every 20m'
  keep: 20
```

Once created, the controller will start a new backup every time the schedule demands it (i.e. at 1:00 AM in the example),
and delete backups once more than `.spec.keep` (20 in the example) have been created. The created backups are named based
on the configuration name and the backup timestamp `$backupconfigname-$timestamp`.

It is also possible to do one-time backups (snapshots).

For more detailed info check [Etcd Backup and Restore]({{< ref "../../cheat_sheets/etcd/backup-and-restore" >}}).

### Creating Etcd Backups

EtcdBackups and Restores are project resources, and you can manage them in the Project view.

![Etcd Backups View](/img/kubermatic/master/ui/etcd_backups.png?classes=shadow,border "Project Etcd Backups")

To create a backup, just click on the `Add Automatic Backup` button. You have a choice of preset daily, weekly or monthly backups,
or you can create a backup with a custom interval and keep time.

![Etcd Backups Configuration](/img/kubermatic/master/ui/add_etcd_backup.png?classes=shadow,border "Etcd Backups Configuration")

To see what backups are available, click on a backup you are interested in, and you will see a list of completed backups.

![Etcd Backups Details List](/img/kubermatic/master/ui/backups_list.png?classes=shadow,border "Etcd Backups Details")

If you want to restore from a specific backup, just click on the restore from backup icon.

![Etcd Backups Restore button](/img/kubermatic/master/ui/restore_backup.png?classes=shadow,border "Restore backup button")

![Etcd Backups Cluster Restore](/img/kubermatic/master/ui/restore_cluster.png?classes=shadow,border "Restore etcd backup for cluster")

#### Etcd Backup Snapshots

You can also create one-time backup snapshots, they are set up similarly to the automatic ones, with the difference that they do not
have a schedule or keep count set.

![Etcd Backup Snapshots](/img/kubermatic/master/ui/backup_snapshots.png?classes=shadow,border "Etcd Backup Snapshots")

## Backup Restores

To restore a cluster etcd from a backup, go into a backup details as shown above and select the backup from which you want to restore. 

What will happen is that the cluster will get paused, etcd will get deleted, and then recreated from the backup. When it's done, the cluster will unpause.

{{% notice note %}}
Keep in mind that this is an etcd backup and restore. The only thing that is restored is the etcd state, not application data or similar.
{{% /notice %}}

{{% notice note %}}
If you have running pods on the user cluster, which are not in the backup, it's possible that they will get orphaned. 
Meaning that they will still run, even though etcd(and K8s) is not aware of them.
{{% /notice %}}

This will create an EtcdRestore object for your cluster. You can observe the progress in the Restore list.

![Etcd Restore List](/img/kubermatic/master/ui/restore_list.png?classes=shadow,border "Etcd Restore List")

In the cluster view, you may notice that your cluster is in a `Restoring` state, and you can not interact with it until it is done.

![Cluster Restoring](/img/kubermatic/master/ui/cluster_restoring.png?classes=shadow,border "Cluster Restoring")

When it's done, the cluster will get un-paused and un-blocked, so you can use it. The Etcd Restore will go into a Completed state.

![Etcd Restore Completed](/img/kubermatic/master/ui/restore_completed.png?classes=shadow,border "Etcd Restore Completed")

