## Steps
- Create an encryption secret to encrypt in transmission data
- Create a [Retention Policy](https://kubestash.com/docs/latest/concepts/crds/retentionpolicy/)
- Create storage secret for specific storage provider.
- Create a [BackupStorage](https://kubestash.com/docs/v2024.6.4/guides/backends/overview/) with specific provider information.
- Create a [VolumeSnapshotClass](https://github.com/kubernetes-csi/external-snapshotter/tree/release-5.0) for specific provider.
- Apply a MongoDBArchiver to setup continuous archiving for source MongoDB
- Apply source MongoDB with label provided in MongoDBArchiver selector.
- To restore archived data to any destination MongoDB, we just have to apply a MongoDB with recovery timestamp.