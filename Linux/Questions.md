## Patching Related Questions and Answers
Q. Patching End-To-End Process flow.
1. Create an EPIC/CR for patching one week prior with all details: server name(s), IP(s), activity start and stop time.
2. Send an email to the server owner, application owner, and stakeholders with patching details (start/stop time and ticket). Ask for any concerns at least one day before.
3. On the patching day, verify approval in the CR/EPIC and check email for any raised concerns.
4. Verify the automated backup completed successfully before the patching window for the target server.
5. Inform the application team to stop the application (if required) and perform pre-checks before patching.
6. Change the ticket status to In Progress or Implementation at the scheduled start time.
7. Trigger and monitor the patching job from SSM at the scheduled start time.
8. After patching, instruct the app team to start the application, perform post-checks, if all good along with application health then close the ticket and notify stakeholders by email.


Q. What is patching in Linux?
Ans: Patching in Linux refers to the process of applying updates, fixes, or improvements to the operating system or software packages installed on a Linux system. These patches are typically released by software vendors to address security vulnerabilities, fix bugs, enhance performance, or introduce new features. Patching is essential for maintaining the stability, security, and functionality of a Linux system.

Q. When we are planning to patch production servers. What are the steps you take before and after patching?
Ans: https://github.com/abhijeet4022/youtube-tutorials/blob/main/Mock_Interview/D2_Linux_Q.md#:~:text=Q5.%20When%20you%20are%20planning%20to%20patch%20production%20servers.%20What%20are%20the%20steps%20you%20take%20before%20and%20after%20patching?

Q. What is the drawback or issue that may arise if you update all available packages?
Ans: https://github.com/abhijeet4022/youtube-tutorials/blob/main/Mock_Interview/D2_Linux_Q.md#:~:text=What%20is%20the%20drawback%20or%20issue%20that%20may%20arise%20if%20you%20update%20all%20available%20packages?

Q. How do you check if a reboot is required after patching? - Optional
Ans: https://github.com/abhijeet4022/youtube-tutorials/blob/main/Mock_Interview/D2_Linux_Q.md#:~:text=How%20do%20you%20check%20if%20a%20reboot%20is%20required%20after%20patching?

Q. How do you manage patching for all environments — all servers in a single shot or part-wise?
Ans: https://github.com/abhijeet4022/youtube-tutorials/blob/main/Mock_Interview/D2_Linux_Q.md#:~:text=How%20do%20you%20manage%20patching%20for%20all%20environments%20—%20all%20servers%20in%20a%20single%20shot%20or%20part%2Dwise?

Q. Suppose your two servers are running behind the Load Balancer and supporting one application. During patching, how will you ensure the application does not go down?
Ans: https://github.com/abhijeet4022/youtube-tutorials/blob/main/Mock_Interview/D2_Linux_Q.md#:~:text=Q20.%20Suppose%20your%20two%20servers%20are%20running%20behind%20the%20Load%20Balancer%20and%20supporting%20one%20application.%20During%20patching%2C%20how%20will%20you%20ensure%20the%20application%20does%20not%20go%20down?

Q. How to update only security patches on a Redhat server?
Ans: yum update --security -y

Q. How to update only specific packages on a Redhat server?
Ans: yum update package_name -y
EX: yum update httpd -y

Q. How to update all the system packages on a Redhat server?
Ans:
yum check-update    // To check the available updates
yum update  -y // To update all the files
init 6 Then we have to reboot the system.

Q. How to exclude any package while updating/patch the server?
Ans: yum update --exclude=package_name -y
EX:  yum update --exclude=httpd* -y
yum undate --security --exclude=httpd* --exclude=nginx* -y

Q. Which tool are you using for patching the Linux servers?
Ans: SSM Patch Manager

Q What is kernel.
When any application require hardware to run the application that time kernel will act as a bridge between hardware and application and it will allocate the required amount of hardware to the app.

Q. How to do manual patching
Ans: 
yum clean all		// To clean all the cache
yum check-update		// To check the available updates
yum update --security -y	// To update only security patches
reboot			// To reboot the system

Q. If any server got failed during patching, how will you handle it?
Ans: First, we need to check the reason for the failure. Based on the error, we will take the necessary action to fix the issue. After fixing the issue, we can re-initiate the patching process on that server. or we can manually patch that server after fixing the issue.
if we run the yum update --security manually it will show the error why the patching is getting failed we need to fix that then re-initiate the patching it will work. some time package version conflict will be there sometime space issue also will be there.

Q. If after patching you are issue how to rollback the patches?
Ans:
yum history list		//  To check the all transaction ID
yum history info <Trans. id>        //  To check the information about the Transaction id.
yum history undo <Trans. id>	//  To roll back into the previous step


############################
Then we will come here
############################

## Performance and utilization Questions and Answers
Q. How do you check CPU utilization on a Linux server?
Q. How to fix if application is responding slow due to high CPU utilization?
Q. How do you check memory utilization on a Linux server?
Q. How to fix if application is responding slow due to high memory utilization?
Q. How do you check disk utilization on a Linux server?
Q. How to fix the high disk utilization issue?
Q. Which monitoring tool you are using to monitor the server performance?
Q. How will you receive the alert.


## Service Related Questions and Answers
Q. What is the process of server provisioning in your organization?
Q. What is the process of decommissioning a server in your organization?


## ITIL Process Related Questions and Answers
Q. What is SLA?
* SLA stands for Service Level Agreement. It is a commitment/written agreement between a service provider company and a client company, covering aspects such as service quality, availability, and responsibilities.

Priority Response and Recovery Availability:

Priority Code	Description 	Target Response Time(Response SLA)	Target Resolution(Resolution SLA)
1                 Critical	       15–30 minutes	                  4 hours
2	                High	         1 hour	                          8 hours
3	               Medium	         4 hours	                      3 business days
4	                 Low	         24 hours	                      5 business days

Q. What is ITIL process?
- ITIL is a set of rules that provides the selection, planning, delivery, maintenance, and overall lifecycle of IT services within a business.

Q. What is Incident Management?
- Incident management describes the necessary actions taken by an organization to analyze, identify, and correct current problems, while also taking actions that can prevent future incidents.

Q. What is an Incident?
- An incident is an unexpected event that affects business operations. It starts with INT.
   Example: Suddenly a disk failed. Server got hanged, application went down.

Q. What is Change Management?
- In ITIL, a change is the addition, removal or any kind of modification of anything/current setup that could have a direct or indirect effect on a service. It starts with CNG or CR.
   Example: Patching, OS upgrade, configuration change, Disk extension.

Q. What is a Service Request?
- A formal user request for something new to be provided is known as a service request. It starts with SR.
   Example: Like App need one more Server to be provision, One extra disk to be add on server.

Q. What is Problem Management?
- Problem management is the process of identifying, analyzing, and resolving the root causes of incidents to prevent their recurrence and minimize the impact on business operations.

Q. What is full form of RTO and RPO?
- RTO stands for Recovery Time Objective, RTO (Recovery Time Objective) is the maximum acceptable time to restore a system or service after a failure or disaster.
Example:
   If your RTO is 30 minutes,
   The cluster must be fully operational within 30 minutes of the outage.
- RPO stands for Recovery Point Objective, which is the maximum acceptable amount of data loss measured in time that a system or application can tolerate after a failure before it significantly impacts the business.

Q. What is the full form of ITIL ?
- ITIL stands for Information Technology Infrastructure Library.

####
- What is the incremental backups and full backup.
- What is snapshot.
- What is CI and CD
- What is Continuous delivery and continuous deployment
- What is Agile and waterfall model in devops.
- What is maintenance window ?
- What is downtime ?
- if aws ec2 backup filed which is configured by aws backup service how to fix that
Q How to Explain This During a Recruiter Call (Very Important)
- Ans: My official designation during the internship was Software Engineer, but my actual responsibilities were focused on cloud operations — working with AWS, Terraform via env0, Jenkins, Linux systems, monitoring using Grafana, and operational support. That’s why I’m positioning myself as a CloudOps Engineer now.
Q If They Ask Directly: “Was this an internship?
- “Yes, it started as an internship, but it involved hands-on work with cloud and operations tools, so I’m counting it as professional experience.”

Q. How to design HA web server in AWS.
Q. What is transit gateway
Q. What is VPN
Q. How many vpc endpoint do you know and what are the usecase of endpoint.
Q. How to resolve the Merge conflict.
Q. How to rollback to previous commit.
Q. What is git stash
Q. What is promethus
Q. What is grafana
Q. What is the port number of grafana and promethues
Q. What is monitoring server
Q. What is node exporter
Q. What is the node exported configuration file
Q. What is scrap metric
Q. What is the agent of grafana
Q. What kind of database promethus uses
Q. How many days promethue can hold the data in db.
Q. How alerts triggered.

Q. How to fetch the output of other module in terraform.?


Q. Explain linux boot process.
