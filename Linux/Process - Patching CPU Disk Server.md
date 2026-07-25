## Patching End-To-End Process Flow.
1. Create an EPIC/CR for patching one week prior with all details: server name(s), IP(s), activity start and stop time.
2. Send an email to the server owner, application owner, and stakeholders with patching details (start/stop time and ticket). Ask for any concerns at least one day before.
3. On the patching day, verify approval in the CR/EPIC and check email for any raised concerns.
4. Verify the automated backup completed successfully before the patching window for the target server.
5. Inform the application team to stop the application (if required) and perform pre-checks before patching.
6. Change the ticket status to In Progress or Implementation at the scheduled start time.
7. Trigger and monitor the patching job from SSM at the scheduled start time.
8. After patching, instruct the app team to start the application, perform post-checks, if all good along with application health then close the ticket and notify stakeholders by email.

## Server Provisioning Process Flow
1. Server provisioning request will come from the application team with approval by {Service Request(SR)/Jira} with all necessary details (OS type, version, disk, hardware specifications (instance type), network configurations (VPC, Subnet), key pair etc.).
2. Review the request for completeness and clarify any missing information with the application team. We can ask App team if any info is missing on the ticket for server creation.
3. Create the server with the provided details in ticket.
4. Attached data disk if requested and mount it on their desire mount point.
5. Add all the required tag like for backup, monitoring, patching, environment, server owner, application name and owner.
5. Enable the backup for the server as per the organization policy.
6. Install the pre-requisite agents {SSM agent, monitoring agents (node-exporter)} on the server.
7. Install the software packages on the server if app team requested any. or help them to setting up their application.
8. Inform security team to consider the new server as part of security monitoring and compliance. 
9. Notify the application team of the server readiness and provide access details. 
10. Close the provisioning ticket and document the server details for future reference.

## Server decommissioning Process Flow
1. When the server no longer needed or not in use by application team or server owner then they will raise a decommissioning request by mail or ticket (Change Request/Jira) with all necessary details (server name, IP address, reason for decommissioning, data retention requirements).
2. After receiving the decommissioning request, review the request raise ticket if it's not there. Mentioned the Time of decommissioning (15 Days of window).
3. There will be separate task inside the CR or Jira for each team.
   a. Shutdown the server : CloudOps
   b. Uninstall the monitoring agents/Antivirus(crowdstrike) from the server : CloudOps
   c. Disable the backup for the server : CloudOps
   d. Remove the server from security tool : Security Team
   e. Remove the CMDB entry for the server : Service Desk/IT Team
   f. Delete the server from the cloud console : CloudOps
4. After raising the ticket inform the server owner and application team about the decommissioning schedule and get their approval. Also their will be one call for decommissioning approval.
5. At the scheduled time, shutdown the server and wait for few days (15 days) to confirm no application issues are coming which is called as smoke testing period.
6. Till the 14 days if any issue are coming we will turn on the server if not we will proceed with the decommissioning steps. From b to f in serial.
7. Post that inform to application team.

## CPU Utilization Investigation Process Flow
* **Answer :**
1. Verify the Alert:
* Use monitoring tools like CloudWatch, Prometheus, or Grafana to check the CPU utilization.
* If server is not hung log into the server and run top command to confirm real-time CPU usage.
* Use uptime to check system load averages.

2. Identify the Root Cause:
* Run `ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head` to find top CPU-consuming processes or Use `top` and press `Shift + P` to sort by CPU usage.
* Check that particular process is application related or system related.
* Sometimes due to more user load or peeks in application usage, CPU usage can spike. And after few minutes it will come down automatically.

3. System-Level Process Check
* Check if a system service, scheduled cron job is responsible for high CPU usage.
* if a system process is consuming high CPU, check its logs or configuration.
* Any scheduled jobs or cron tasks that might be running at the time of high CPU usage. Wait to complete the job.
* Check memory usage (free -m) — high memory pressure can cause CPU spikes.

4. Application-Level Check:
* If it's an application service causing the issue, take the screenshots and notify the application team (Application Owner/Server Owner).
* Restart the process if it’s unresponsive or consuming abnormally high CPU After taking the necessary approvals.
* If this issue came after application update by application then there might be change of code bug which need to inform to developer/application owner.

5. Reboot Server (if required):
* If server is hung and application is down then take approval and reboot the server.

6. Historical Analysis:
* Check historical data from monitoring tools (like CloudWatch, Prometheus, Grafana) to see if this is a recurring issue.
* If yes like daily or weekly basis, consider scaling the instance size after approval.

## Memory Utilization Investigation Process Flow

## Disk Space Investigation Process Flow
1. We will get ticket for disk utilization issue.
2. Then identify the high disk usage partition/filesystem using `df -hT` command.
3. Navigate to that partition using `cd <mount point>` command.
4. Use `du -sh * | sort -hr` to find the top large files/directories.
5. Then see whether those are OS files or application files.
6. APP Disk
   1. If those are application files then inform the application team to take necessary action. 
   2. And if cleanup is not possible then need to increase the disk size.
7. OS Disk
   1. If those are OS files find out which files are consuming more space.
   2. If log files then try to archive it or delete the old log files. Also verify logrotate is working fine.
   3. If it's cache files then clear the cache using `yum clean all`.
   4. Also, we can unnecessary packages using `yum autoremove`.
   5. Also find out if any unwanted user files are present inside /home or /root directory.
   6. If user files are present then inform the respective user to delete those files.
   7. If old kernels are present then remove the old kernels using `package-cleanup --oldkernels --count=1` command.

### Disk Extension Steps - LVM
1. Increase the disk from AWS
2. Increase the partition using `growpart <disk>  <partition number>` command
    EX: `growpart /dev/sda 2` last partition of disk.
3. Resize the PV using `pvresize <partition>` command
4. Resize the LV using `lvextend -L +<size> <LV path>` command
5. Run `xfs_growfs` or `resize2fs` based on the filesystem to reflect the changes
   - For XFS filesystem: `xfs_growfs <mount point>`
   - For EXT4 filesystem: `resize2fs <partition>`
Note: If OS disk is full then we won't be able to login so it's critical to fix the issue ASAP.


## Cost Optimization Process Flow
1. Tool SpendEffix will generate the report on weekly basis and Lead will share this report with the respective team.
2. We will review the report and identify the unused or underutilized resources.
*  Unused resources could be:
   - Unattached EBS volumes
   - Underutilized EC2 instances (low CPU/Memory utilization)
   - Unused Elastic IPs
   - Idle Load Balancers
   - Old Snapshots
   - Unused AMIs
3. For each identified resource, verify with the application team or resource owner if it is still needed or not.
4. If not needed, raise the ticket for resource deletion and take necessary approval to delete the resource.