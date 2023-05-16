#### AWS SDWAN Deployment ####



#### Each Participant will Complete the Below Steps ####
- [] deploy thethe AWS Cloud Underlay and SDWAN instances with the base configuration via automation pipeline.
- [] learn how to perform required manual steps in the SDWAN Vmanage GUI that have been decoupled from the CLI.
- [] learn how to test/verify traffic flow 
- [] learn how to generate and install an Enterprise CA and certificate
- [] Configure Cisco SMART Licensing for the Smart Account and Virtual Accounts for multi-tenancy and virtual accounts and onboard CAT8KV



#### Students will perform the following steps upon entering lab: ####

- [] Open Intellij IDE and Terminal
- [] Open the LTRAPP3000 directory (The lab participant will have full access to this directory for lab work and git)
- [] Open the above directory via Terminal
$cd LTRAPP3000
- [] Create a directory
$mkdir lab-keys
- [] exist out of this directory to the root of LTRAPP300 dir
$cd ../
- [] Clone git repo (instructor will provide repo link to participants)
$git clone git@github.com:devops-ontap/sdwan.git --config core.sshCommand="ssh -i ~/.ssh/sconrod-tester"
- [] Checkout Branch - Instructor will assign you a pre-provisioned branch to work in
$git checkout {{your branch name assigned by instructor}}

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

Learning Challenges #1 - 15 min
=======================
- [] There would not be a learning opportunity if the Participants simply started the automation code pipeline and the SDWAN deployed. 
- [] Therefore, the Instructor has introduced one failure into the Cloud Underlay Deployment. 
- [] Participants will be challenged to discover the issue, then there will be a short discussion as to the underlying cause of the issue and how to remediate the problem.
- [] Participants will be instructed to use an "Automation Task" to fix the issue.
- [] The Instructor will provide hints and demonstrate how to run an automation task, following Best Practices for Agile Rapid Software Development.
- [] Upon Completion of the challenge the AWS porition of the lab will be completed.

Learning Challenge #2 - 15 min
==============================
- [] Create a new task to deploy another vmanage server using the code via your pipeline - first use an automation task. When it succeeds, add it to your pipeline and deploy again.


Summary 
======
Instructor will provide a verbal recap of all steps completed.

Students will have successfully deployed and configured
=====

AWS VPC, route tables, security groups and rules, internet gateway, elastic ips, route tables, vbond, vmanage, smart and cat8kv device deployment to correct
subnets as well as secondary disk deployment and application of user data file, as well as Enterprise ROOT CA installation and have added at least one Tenant to their 
SDWAN multi-tenancy. Participants will also understand how to configure Cisco Software Licensing SMART and Virtual accounts for multi-tenancy.




