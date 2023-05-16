## Google Cloud Platform
All components are hosted on a single compute instance in the `parrinc` project found [here](https://console.cloud.google.com/welcome?authuser=1&project=parrinc-191702)

### Server
* Machine Type: `n1-standard-4`
* Region: `us-central1 (Iowa)`
* Zone: `us-central1-a`
* CPU: `4 cores`
* Memory: `15 GB`
* Disk: `75 GB`
* Operating System: `Debian 10`

### Backups
We take weekly snapshots of the server that are easily restored via the GCP console, they can be found [here](https://console.cloud.google.com/compute/snapshots?authuser=1&project=parrinc-191702).

##### Steps to restore from snapshot
1. Navigate to the [snapshots page](https://console.cloud.google.com/compute/snapshots?authuser=1&project=parrinc-191702)
2. Click on the snapshot you want to restore from (pay attention to the snapshot date)
3. At the top of the page click on the `CREATE INSTANCE` button 
4. Name the new instance appropriately and select the configuration based on the values [above](/#!server_config.md#Server)
5. Once the new instance is up take note of the **EXTERNAL** IP address and [update the DNS record accordingly](/#!software.md#DNS)
6. At this point the server should be restored
