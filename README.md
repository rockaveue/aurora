# Upgrading AWS Aurora

You can upgrade Aurora 1 to 2 with zero-downtime patching. Meaning you can upgrade it without using snapshots.

But zero-downtime patching is not available for Aurora 2 to 3, so you need to use the cluster method.

## Upgrading Aurora 1(MySQL 5.6) to 2(MySQL 5.7)

- Created Aurora MySQL 1.23.4 (5.6)

- Choose modify database

*Choosing Aurora MySQL 2.10.2*

![image](/images/1.png)

*Choosing apply immediately*

![image](/images/2.png)

*Cluster changed its status to upgrading first*

![image](/images/3.png)

*Then the instance changed its status*

![image](/images/4.png)

[Other's experience](https://stackoverflow.com/a/65705130)

*After the instance is available*

![image](/images/5.png)

*Cluster log*

![image](/images/6.png)

iteration delay 3000ms

Starting from iteration 54 request stopped going

At iteration 207 requests started to go normally

Around iteration 260 the upgrade completely finished

*Got error while trying in-place upgrade for Aurora MySQL 2 to 3*

![image](/images/7.png)

![image](/images/8.png)



# Upgrading from version 2 to version 3
## Creating snapshot
`aws rds create-db-cluster-snapshot --db-cluster-id aurora-test \  --db-cluster-snapshot-id cluster-57-upgrade-ok-snapshot`

![image](/images/9.png)

*Created snapshot*

![image](/images/10.png)

*Creating Aurora MySQL 3 cluster from snapshot*

![image](/images/11.png)

*Creating instance for the new cluster*

![image](/images/12.png)

*Process of new cluster and instance creation*

![image](/images/13.png)

![image](/images/14.png)

![image](/images/15.png)

![image](/images/16.png)

*Checking newly created cluster version*

![image](/images/17.png)


## References

[Aurora MySQL major version upgrade paths](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Updates.MajorVersionUpgrade.html#AuroraMySQL.Upgrading.Compatibility)

[Using zero-downtime patching](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Updates.Patching.html#AuroraMySQL.Updates.ZDP)

[Example of upgrading from Aurora MySQL version 2 to version 3](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.MySQL80.html#AuroraMySQL.mysql80-upgrade-example-v2-v3)