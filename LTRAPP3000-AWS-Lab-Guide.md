#### AWS SDWAN Deployment ####
Introduction
=============

Instructor will explain the architecture of the devops rig and after all SDWANs have been deployed can go into great detail on this config
=========
- [] Instructor will deliver high level verbal explanation and provide diagram over view of devops rig
- [] Instructor will explain lab workflow and how it follows standard rapid iteration techniques of Devops Engineers and the red green process of the Software Development Lifecyle process
- [] Instructor will give real world examples of using this architecture in devops and netops organization and the historical evolution of this rig and process
- [] Participants are encouraged to take notes in their notes.md file in their git branch

To save time, the Instructor has already uploaded the Cisco SDWAN VHDs to the cloud provider and converted VHD convertion process is:
- [] uplaod the vmanage, vbond, vedge VHD to AWS S3
- [] take a snapshot of the VHD
- [] convert the VHD to AMI 
- [] copy the AMI to each region where it will be deployed

Since these are simple tasks, and since they are only performed when  new VHD is released with a new SDWAN version 
They have not been added into the automation for this lab due to time constraints. 
The will be added in however at a later date and in the meantime can be found in the lab repo path here:
devops-ontap/sdwan/test-tasks/aws_deploy_ami_s3/input/aws_deploy_ami.py

#### Points Worth Noting, Lab World Versus Real World ####
Some simplications have been made to this lab to allow the Instructor to easily help any student.
These will need to be modified before taking this to production in your own environment. 
The instructor will go over these simplications - such as: EC2-Console Access is enabled on the instances for ease of Instructor to connect to different students envs when required to assist them

#### Each Participant will Complete the Below Steps ####
- [] deploy thethe AWS Cloud Underlay and SDWAN instances with the base configuration via automation pipeline.
- [] learn how to perform required manual steps in the SDWAN Vmanage GUI that have been decoupled from the CLI.
- [] learn how to test/verify traffic flow 
- [] learn how to generate and install an Enterprise CA and certificate
- [] Configure Cisco SMART Licensing for the Smart Account and Virtual Accounts for multi-tenancy and virtual accounts and onboard CAT8KV

#### The following has been pre-configured in DNS and Software.Cisco.com for the purpose of this lab to simulate licensing for a MSP Partner

- []  Smart Account: devops-ontap.com
- []  Profile Name: vbond.devops-ontap.com
- [] PRIMARY vbond name: vbond.devops-ontap.com
- [] Organization name: vbond.devops-ontap.com
- [] Controller Type: vbond

#### Students will perform the following steps upon entering lab: ####

- [] Open Intellij IDE and Terminal
- [] Open the following new tabs in the Intellij Terminal: login, pipes, git, vmanage, vbond, vsmart, vedge, keys

#### The login to the lab will time out after a few minutes of inactivity so after breaks you will need to log back in ####
you will be able to do so simply by going to the 'login' tab and pressing the arrow up key and the enter key on your keyboard

#### you will be performing a number of git commands and you may just arrow up in the git tab instead of re-typing these commands repeatedly: ####
- [] git add filename
- [] git commit {{"refactoring"}}
- [] git push
- [] git log

#### you will be resetting your pipeline after making design changes via the 'pipes' tab simply by pressing the arrow up key and then the enter key ####
Example:
fly -t {{your team name}} set-pipeline -c {{the full path to your pipeline.yml file}} -p {{your pipeline name}} -l {{the full path to the parameters file - supplied day of lab due to security reasons}}

- [] Open the LTRAPP3000 directory (The lab participant will have full access to this directory for lab work and git)
- [] Open the above directory via Terminal
- [] $cd LTRAPP3000
- [] Create a directory
- [] $mkdir lab-keys
- [] exist out of this directory to the root of LTRAPP300 dir
- [] $cd ../
- [] Clone git repo (instructor will provide repo link to participants)
- [] $git clone git@github.com:devops-ontap/sdwan.git --config core.sshCommand="ssh -i ~/.ssh/sconrod-tester"
- [] Checkout Branch - Instructor will assign you a pre-provisioned branch to work in
- [] $git checkout {{your branch name assigned by instructor}}
- [] logon to their automation pipeline.

#### Instructor will provide the logon creds day of lab ####
Example:
fly --target=prod-main login --concourse-url=http://prod-ci.devops-ontap.com:8080 -n {{participant team name}} --username={{username}} --password={{password}}

#### Participants pipeline will be in a paused state #### 
*****WAIT before un-pausing pipeline to verify code in your repo by Instructor*****


Important Note
=============
- [] The Instructor will demonstrate all  steps before Participants being steps. Please watch Instructor Demo and take notes if required before starting lab work.
- [] The Instructor will demonstrate a few techniques to verify via CLI that the Cloud Provider Underlay is functioning as desired
- [] The Instructor will perform a code review prior to you starting work.

Pipeline Phase 1
==================

The pipeline will deploy the cloud underlay, the vmanage, vbond, vsmart, vedge instances and configure them

GUI Steps - steps uncoupled from API and CLI
=============
- [] reset password on gui to admin1 - do this on all three vmanage machines
- [] Administration, Settings - select multi-tenancy and reboot - on the first vmanage machine only 
- [] Administration, Settings, SP Organization, vBond, Controller Certificate Authority(instructor will demo how to create this and update gui) - first vmanage machine only.
- [] Follow Steps in the Document devops-ontap/sdwan/enterprise-cert-steps.md 
- [] vmanage_2 and vmanage_3 repeat steps:

from vshell on vmanage_2 and vmanage_3:

- [] scp admin@{{vmanage_1 ip}}:/home/admin/ROOT-CA.pem .
- [] request root-cert-chain install /home/admin/ROOT-CA.pem

from vmanage_1 GUI
==========
-[] select Administration, Cluster Management, select the 3 dots beside vmanage_1, and select edit. From the vManage IP Address dropdown select the cluster ip
- [] services will restart on vmanage again. This can take 3-5 minutes.
- [] add vmanage_2 by the cluster IP, wait for services to restart
- [] add vmanage_3 by the cluster IP, wait for services to start

-[] select the three bars right top of Administration - Cluster Management to watch the progress of the cluster

-[] install the ROOT-CA.PEM and cert chain on vbonds and vsmarts as well as vedge.
-[] once the ROOT-CA.pem is installed on all instances along with the chain, add each of vbonds, vsmarts to vmanage gui.
-[] select on vmanage, Configuration, Devices, Controllers. Add in all vbonds, vsmarts
-[] a CSR is sent to each machine. SCP all CSRs to the vmanage and create the CRTs. Paste the CRTs into the GUI except for vedge, send the CRT 
back to vedge as we do not install the vedge cert in the GUI but rather via the requests command on the vedge device.

- [] run commands to verify cluster is sync'd
- [] in order to put the vbonds and vsmarts in vmanage mode you need to apply a template. A CLI template will work - 
- [] add Tenant, example aspadescisco - Select Administration, Tenant Management, Add Tenant. Complete the form and select both vSmarts

Learning Challenges #1 - 15 min
=======================
Preparation: Read the following article: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html
===========================

- [] There would not be a learning opportunity if the Participants simply started the automation code pipeline and the SDWAN deployed. 
- [] Therefore, the Instructor has introduced one failure into the Cloud Underlay Deployment. 
- [] Participants will be challenged to discover the issue, then there will be a short discussion as to the underlying cause of the issue and how to remediate the problem.
- [] Participants will be instructed to use an "Automation Task" to fix the issue.
- [] The Instructor will provide hints and demonstrate how to run an automation task, following Best Practices for Agile Rapid Software Development.
- [] Upon Completion of the challenge the AWS porition of the lab will be completed.

Learning Challenge #2 - 15 min
==============================
- [] Create a new task to deploy another vmanage server using the code via your pipeline - first use an automation task. When it succeeds, add it to your pipeline and deploy again.
- [] Add the new vmanage servers to your SDWAN (you may use GUI if your code doesn't work)

Learning Challenge #3 - 15 min
==============
- [] Setup an Enterprise CA and install on vmanage, all instances, then upload the license file.
- [] Read and Perform Follow Steps in the following doc in the repo(replacing values in {{}}:
- [] devops-ontap/sdwan/enterprise-cert-steps.md

Summary 
======
Instructor will provide a verbal recap of all steps completed.

Students will have successfully deployed and configured
=====

AWS VPC, route tables, security groups and rules, internet gateway, elastic ips, route tables, vbond, vmanage, smart and cat8kv device deployment to correct
subnets as well as secondary disk deployment and application of user data file, as well as Enterprise ROOT CA installation and have added at least one Tenant to their 
SDWAN multi-tenancy. Participants will also understand how to configure Cisco Software Licensing SMART and Virtual accounts for multi-tenancy.

Notes
========
vbond: vbond.devops-ontap.com
domain: devops-ontap.com
cluster id: cluster
org name: devops-ontap.com
sp name: devops-ontap.com

Import Step after Auto-Config in pipeline of all vmanage, there are settings in the GUI that are decoupled that must be repeated before adding all nodes to cluster
==========
- [] Before a vmanage node can be added to teh cluster, you must logon to the gui and set the
- [] org name, and the vbond dns name as well as install the cert and cert chain
- [] its only possible to add one node into the cluster at a time as each time you add a node the services restart and its the equivalent to a full restart
- [] make sure you are not logged on via SSH to the vmanage nodes when you add them to the cluster
- [] CSRs/Generation and CRTs need to be installed after all nodes are added to cluster. 

vbond, vsmart ENI indexes
==========================

It is critical to have a pipeline to test new AMIS which are the new versions of SDWAN.
From time to time, the indexes on the VHDs will change. 
Currently in this version, the vmanage index0 comes up on VPN0, however vbond, vsmart, vedge, all come up with vpn512/mgmt comes up on eth0 and the ge0/0 comes up 
on index_1 on the public subnet. Therefore, the only way to connect to these initially when using SSH the first time is on the 512 - so it needs to have an EIP
This is very interesting and unusual.

Grand Prize Challenge
===================

- [] After all cloud underlays and SDWANs are deployed, deploy a second vedge to any cloud and onboard it to a vmanage in a different cloud
- Use an automation task, push to your branch after you have it working

Bonus Points
========
#### If you are a rocket scientist and Devops Pro this one is for you! ####
- [] Deploy your second vedge in each cloud to different cloud sdwans.


Additional Resources and Learning Materials
========

SDWAN Smart Licensing 
=======
https://www.cisco.com/c/dam/en_us/services/downloads/SD-WAN_pnp_support_guide.pdf
