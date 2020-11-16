# Template Storage XSKY XMS by HTTP

## Overview

For Zabbix version: 4.4 and higher

This Zabbix Template includes discovery rules, item prototypes, trigger prototypes and graphs, Zabbix will periodically gather resource metrics from HTTP/HTTPS REST API, convert metrics into JSON object, automatically create resource metrics by discovery rules, and send triggers based on the trigger rules.

This template was tested on:

* XSKY SDS, V4

## Setup

1. Open XSKY SDS console ```http://<admin-ip>:8056``` in browser, and click ```Access Token``` in the right-top ```Gear``` menu.
2. Create an access token with Permission ```Observer``` for Zabbix.
3. Copy the access token in notebook, and set it in Macros of Zabbix Template.

## Zabbix configuration

No specific Zabbix configuration is required.

### Macros used

|Name|Description|Default|
|----|----|----|----|
|{$XMS_ACCESS_TOKEN}|XMS Access Token||
|{$XMS_API_IP}|XMS API Admin IP Address||
|{$XMS_API_PORT}|XMS API Port|8051|
|{$XMS_BUCKET_ALLOCATED_OBJECTS.MAX.WARN}|XMS Bucket Allocated Objects|80|
|{$XMS_BUCKET_ALLOCATED_SIZE.MAX.WARN}|Max Warn Percentage of XMS Bucket Allocated Size|80|
|{$XMS_OSDS_IO_UTIL.MAX.WARN}|Max Warn Percentage of XMS OSDS IO Util|90|
|{$XMS_OSDS_USED_BYTE.MAX.DISASTER}|Max Disaster Percentage of XMS OSDS Used Byte|85|
|{$XMS_OSDS_USED_BYTE.MAX.WARN}|Max Warn Percentage of XMS OSDS Used_Byte|80|
|{$XMS_OS_USER_ALLOCATED_OBJECTS.MAX.WARN}|Max Warn Percentage of XMS OS User Allocated Objects|80|
|{$XMS_OS_USER_ALLOCATED_SIZE.MAX.WARN}|Max Warn Percentage of XMS OS_User Allocated Size|80|
|{$XMS_POOL_DATA_KBYTE.MAX.DISASTER}|Max Disaster Percentage of XMS Pool Data kbyte|90|
|{$XMS_POOL_DATA_KBYTE.MAX.HIGH}|Max High Percentage of XMS Pool Data kbyte|85|
|{$XMS_POOL_DATA_KBYTE.MAX.WARN}|Max Warn Percentage of XMS Pool Data kbyte|80|
|{$XMS_S3_LOAD_BALANCER_ACTIVE_CONNECTIONS.WARN}|Max Warn Value of XMS S3 Load Balancer Active Connections|1024|
|{$XMS_S3_LOAD_BALANCER_CPU_UTIL.WARN}|Max Warn Percentage of XMS S3 Load Balancer CPU Util|80|
|{$XMS_S3_LOAD_BALANCER_MEM_USAGE_PERCENT.WARN}|Max Warn Percentage of XMS S3 Load Balancer MEM Usage|80|

### Template links

There are no template links in this template.

### Discovery rules

|Name|Description|Type|Key and additional info|
|---|---|---|---|
|XMS Disk (Cache) Discovery|Hardware Disks in Storage Cluster|HTTP agent|xms.hosts.disk_cache.discovery|
|XMS Host Discovery|Hosts in Storage Cluster|HTTP agent|xms.hosts.discovery|
|XMS Network Interfaces Discovery|Network interfaces in Storage Cluster|HTTP agent|xms.hosts.network_interfaces.discovery|
|XMS NFS-Gateways Discovery|NFS Gateways in Storage Cluster|HTTP agent|xms.os_nfs_gateways.discovery|
|XMS OS-Bucket Discovery|Object Storage Buckets in Storage Cluster|HTTP agent|xms.os_buckets.discovery|
|XMS OS-User Discovery|Object Storage Users in Storage Cluster|HTTP agent|xms.os_users.discovery|
|XMS OS-Zones Discovery|Object Storage Zones in Storage Cluster|HTTP agen|xms.os_zones.discovery|
|XMS OSD Discovery|OSDs in Storage Cluster|HTTP agent|xms.host.osds.discovery|
|XMS POOL Discovery|Storage Pools|HTTP agent|xms.pools.discovery|
|XMS S3 Load Balancer Discovery|S3 Load Balancers in Storage Cluster|HTTP agent|xms.os_s3_load_balancers.discovery|
|XMS Services Discovery|Host Services in Storage Cluster|HTTP agent|xms.hosts.services.discovery|

### Items collected

|Name|Description|Type|Key and additional info|
|---|---|---|---|
|XMS Cache Disks|Hardware Cache Disks|HTTP agent|xms.hosts.disk_cache.get|
|XMS Cluster|Cluster Overview|HTTP agent|xms.cluster.get|
|XMS Cluster: XMS Cluster Version|SDS Cluster Version|Dependent item|xms.cluster.version|
|XMS Cluster Stats|Cluster Stats|HTTP agent|xms.cluster.stats.get|
|XMS Cluster Stats: XMS Cluster Os Bucket Num|Object Storage Bucket Num|Dependent item|xms.cluster.stats.os_bucket_num|
|XMS Hosts|Storage Hosts|HTTP agent|xms.hosts.get|
|XMS Network Interfaces|Network Interfaces in Storage Cluster|HTTP agent|xms.hosts.network_interfaces.get|
|XMS OS-Buckets|Object Storage Buckets in Storage Cluster|HTTP agent|xms.os_buckets.get|
|XMS OS-NFS-Gateways|Object Storage NFS Gateways in Storage Cluster|HTTP agent|xms.os_nfs_gateways.get|
|XMS OS-S3_Load_Balancer|Object Storage S3 Load Balancers in Storage Cluster|HTTP agent|xms.os_s3_load_balancer.get|
|XMS OS-Users|Object Storage Users in Storage Cluster|HTTP agent|xms.os_users.get|
|XMS OS-Zones|Object Storage Zones in Storage Cluster|HTTP agent|xms.os_zone.get|
|XMS OSDs|OSDs in Storage Cluster|HTTP agent|xms.hosts.osds.get|
|XMS Pools|Storage Pools|HTTP agent|xms.pools.get|
|XMS Services|Host Services in Storage Cluster|HTTP agent|xms.hosts.services.get|

### Triggers

|Name|Description|Expression|Severity|Dependencies and additional info|
|---|---|---|---|---|
|Host {#XMS_HOST_NAME} Cache Disk {#XMS_HOST_DEVICE} Status Is error|Check if cache disk status is error|Problem: {Template Storage XSKY XMS by HTTP:xms.host.disk.status[{#XMS_OSD_ID}].str(error)}=1; Recovery: {Template Storage XSKY XMS by HTTP:xms.host.disk.status[{#XMS_OSD_ID}].str(active)}=1|Disaster||
|Host {#HOSTNAME} Status Is error|Check if host status if error|Problem: {Template Storage XSKY XMS by HTTP:xms.hosts.status[{#HOSTID}].str(error)}=1; Recovery: {Template Storage XSKY XMS by HTTP:xms.hosts.status[{#HOSTID}].str(active)}=1|Disaster||
|Host {#HOSTNAME} Status Is offline|Check if host status is offline|Problem: {Template Storage XSKY XMS by HTTP:xms.hosts.status[{#HOSTID}].str(offline)}=1; Recovery: {Template Storage XSKY XMS by HTTP:xms.hosts.status[{#HOSTID}].str(active)}=1|Disaster||
|Host {#HOSTNAME} Status Is warning|Check if host status is warning|Problem: {Template Storage XSKY XMS by HTTP:xms.hosts.status[{#HOSTID}].str(warning)}=1; Recovery: {Template Storage XSKY XMS by HTTP:xms.hosts.status[{#HOSTID}].str(active)}=1|Warning||
|Host {#HOSTNAME} Network {#NETWORKNAME} is offline|Check if network interface is offline|Problem: {Template Storage XSKY XMS by HTTP:host.network_interfaces.status[{#NETWORKID}].str(down)}=1; Recovery: {Template Storage XSKY XMS by HTTP:host.network_interfaces.status[{#NETWORKID}].str(up)}=1|High||
|Object Storage Bucket {#XMS_BUCKET_NAME} Allocated Objects Is Greater Than {$XMS_BUCKET_ALLOCATED_OBJECTS.MAX.WARN}%|Check if allocated objects of os bucket is warning||Warning||
|Object Storage Bucket {#XMS_BUCKET_NAME} Allocated Size Is Greater Than {$XMS_BUCKET_ALLOCATED_SIZE.MAX.WARN}%|Check if allocated size of os bucket is warning||Warning||
|Object Storage User {#XMS_OS_USER_NAME} Allocated Objects Is Greater Than {$XMS_OS_USER_ALLOCATED_OBJECTS.MAX.WARN}%|Check if allocated objects of os user is warning||Warning||
|Object Storage User {#XMS_OS_USER_NAME} Allocated Size Is Greater Than {$XMS_OS_USER_ALLOCATED_SIZE.MAX.WARN}%|Check if allocated size of os user is warning||Warning||
|Host {#HOSTNAME} {#OSDNAME} Data Usage Is Greater Than {$XMS_OSDS_USED_BYTE.MAX.DISASTER}%|Check if osd data usage is disaster||Disaster||
|Host {#HOSTNAME} {#OSDNAME} Data Usage Is Greater Than {$XMS_OSDS_USED_BYTE.MAX.WARN}%|Check if osd data usage is warning||Warning||
|Host {#HOSTNAME} {#OSDNAME} IO Usage Is Greater Than {$XMS_OSDS_IO_UTIL.MAX.WARN}%|Check if osd io usage is warning||Warning||
|Host {#HOSTNAME} {#OSDNAME} Status Is error|Check if osd status is error|Problem: {Template Storage XSKY XMS by HTTP:hosts.osds.status[{#OSDID}].str(error)}=1; Recovery: {Template Storage XSKY XMS by HTTP:hosts.osds.status[{#OSDID}].str(active)}=1|Disaster||
|Host {#HOSTNAME} {#OSDNAME} Status Is Stopped|Check if osd status is stopped|Problem: {Template Storage XSKY XMS by HTTP:hosts.osds.status[{#OSDID}].str(stopped)}=1; Recovery: {Template Storage XSKY XMS by HTTP:hosts.osds.status[{#OSDID}].str(active)}=1|Warning||
|Host {#HOSTNAME} {#OSDNAME} Status Is Warning|Check if osd status is warning|Problem: {Template Storage XSKY XMS by HTTP:hosts.osds.status[{#OSDID}].str(warning)}=1; Recovery: {Template Storage XSKY XMS by HTTP:hosts.osds.status[{#OSDID}].str(active)}=1|Warning||
|Pool {#POOLNAME} Capacity Usage Is Greater Than {$XMS_POOL_DATA_KBYTE.MAX.DISASTER}%|Check if capacity usage of pool is disaster||Disaster||
|Pool {#POOLNAME} Capacity Usage Is Greater Than {$XMS_POOL_DATA_KBYTE.MAX.HIGH}%|Check if capacity usage of pool is high||High||
|Pool {#POOLNAME} Capacity Usage Is Greater Than {$XMS_POOL_DATA_KBYTE.MAX.WARN}%|Check if capacity usage of pool is warning||Warning||
|Pool {#POOLNAME} Status Is degraded|Check if capacity usage of pool is degraded||Warning||
|Pool {#POOLNAME} Status Is error|Check if pool status is error||Disaster||
|Pool {#POOLNAME} Status Is full|Check if pool status is full||Disaster||
|S3 Load Balancer {#XMS_S3_LOAD_BALANCER_NAME} Active Connections Are Greater Than {$XMS_S3_LOAD_BALANCER_ACTIVE_CONNECTIONS.WARN}|Check if active connnections of s3 load balancer is warning|Warning||
|S3 Load Balancer {#XMS_S3_LOAD_BALANCER_NAME} CPU Usage is Greater Than {$XMS_S3_LOAD_BALANCER_CPU_UTIL.WARN}%|Check if cpu usage of s3 load balancer is warning||Warning||
|S3 Load Balancer {#XMS_S3_LOAD_BALANCER_NAME} Memory Usage is Greater Than {$XMS_S3_LOAD_BALANCER_MEM_USAGE_PERCENT.WARN}%|Check if memory usage of s3 load balancer is warning||Warning||
|S3 Load Balancer {#XMS_S3_LOAD_BALANCER_NAME} Status Is Error|Check if s3 load balancer status is error||Disaster||
|Service {#TYPE} of Host {#HOSTNAME} is offline|Check if host is offline|	Problem: {Template Storage XSKY XMS by HTTP:host.service.status[{#HOSTID}.{#TYPE}].str(false)}=1; Recovery: {Template Storage XSKY XMS by HTTP:host.service.status[{#HOSTID}.{#TYPE}].str(true)}=1|High||

## Feedback

Please report any issues with the template at https://support.zabbix.com
