<img src="https://image.freepik.com/free-icon/white-house-building_318-37808.jpg" align="left" height="48" width="48">

# RightScale Policy CAT Files
## Overview

You can use Cloud Application Templates (CATs) in RightScale Self Service to automate policies. We have created sample policy CATs that you can use as a starting point. These are provided solely as samples under an Apache 2.0 open source license with no warranties.

**Important: You should test these CATs to ensure they work for your needs.**

## Sample Policy CATs

### Start / Stop Scheduler
This CAT is a scheduler process that will start or stop VM instances based on a schedule managed within Self-Service and a special tag on the VM instance.  The tag on the VM instance is 'instance:schedule=<schedule name>'; e.g. 'instance:schedule=Business Hours'.  The scheduler will check to see if the current time is inside the schedule window, and if so, attempt to start instances that are not running.  If the current time is outside the schedule window, the scheduler will attempt to stop instances that are running.

Unlike the schedules attached to a VM launched by RightScale, this process will work for instances created outside of RightScale without RightLink.

To exclude an instance from the scheduler, the scheduler will look for exclude tags.  The examples show an exclude tag of 'instance:scheduler_exclude=true', but truly any tag could be used to exclude.  For example, you could exclude all tags for a specific environment as in: 'ec2:env=QA'.

Finally, the Scheduler itself runs within the context of the user's timezone. So, the 'Business Hours' schedule would be in the context of the timezone of the person that launched the scheduler.  If that is not appropriate, the timezone can be overruled with a parameter in the launch; i.e. a European timezone could be chosen to override US-NewYork.

Detailed logs are written to the Scheduler's Deployment audit log to verify that instances have been considered for starting or stopping and in fact the start / stop command was executed against specific instances within the context of a schedule.
### Unattached Volume Finder
**What it does**

This policy CAT will search all your cloud accounts that are connected to the RightScale account where you are running the CAT. It will find unattached volumes that are have been unattached for longer than a specified number of days.

You can choose to **Alert only** or **Alert and Delete** the volumes. **_Warning: Deleted volumes cannot be recovered_**.  We strongly recommend that you **start with Alert only** so that you can review the list and ensure that the volumes should actually be deleted. You can specify multiple email addresses that should be alerted and each email will receive a report with a list of unattached volumes.

<img src="https://github.com/rs-services/policy-cats/blob/master/readme_images/volume_email_screenshot.png" width="600">

The emails in the sample CAT are sent using a shared RightScale account of a free email service (mailgun). We have used a proxy, however, you may want to modify the CAT to use your own email service.

**Scheduling when the policy runs**

To control the frequency that the policy CAT runs, you should [create a schedule and associate it with the CAT](http://docs.rightscale.com/ss/guides/ss_creating_schedules.html) in RightScale Self-Service.

Specify the days of the week that you want the CAT to run. For example, if you want the policy CAT to run once a week on Monday, specify a schedule of only Monday. For the hours you should specify approximately a 30 minute time window for the policy CAT to complete. (It should take less than 15 minutes to run).

<img src="https://github.com/rs-services/policy-cats/blob/master/readme_images/create_a_new_schedule.png">

**Cost**

This policy CAT does not launch any instances, and so does not incur any cloud costs.



### Instance Runtime Policy

**What it does**

This policy CAT will search all your cloud accounts that are connected to the RightScale account where you are running the CAT. It will find instances that have been running for longer than a specified number of days.

This policy might be useful in demo, training, development, or test accounts where instances should not be running for a long period of time.

You can choose to **Alert only** or **Alert and Terminate** the instances. We strongly recommend that you start with **Alert only** so that you can review the list and ensure that the instances should actually be terminated. **_Warning: Terminated instances cannot be recovered._** You can specify multiple email addresses that should be alerted and each email will receive a report with a list of long-running instances.


**Notifications (Email)**

The emails in the sample CAT are sent using a shared RightScale account of a free email service (mailgun). We have used a proxy, however, you may want to modify the CAT to use your own email service.

<img src="https://github.com/rs-services/policy-cats/blob/master/readme_images/long_running_instance_screenshot.png" width="600">

**Scheduling when the policy runs**

To control the frequency that the policy CAT runs, you should [create a schedule and associate it with the CAT](http://docs.rightscale.com/ss/guides/ss_creating_schedules.html) in RightScale Self-Service.

Specify the days of the week that you want the CAT to run. For example, if you want the policy CAT to run once a week on Monday, specify a schedule of only Monday. For the hours you should specify approximately a 30 minute time window for the policy CAT to complete. (It should take less than 15 minutes to run).

<img src="https://github.com/rs-services/policy-cats/blob/master/readme_images/create_a_new_schedule.png">

**Cost**

This policy CAT does not launch any instances, and so does not incur any cloud costs.


### How to Use these CATs

1. [Download the policy CAT file from GitHub.](https://github.com/rs-services/policy-cats)
1. Make any desired changes to the policy CAT.
3. Upload and test the policy CAT. Use the Alert only option during testing. **Do not choose Alert and Delete until you are confident you know what will be deleted.**
4. [Create a schedule and associate it with the CAT.](http://docs.rightscale.com/ss/guides/ss_creating_schedules.html)
5. Launch the CAT in Test mode from the Designer screen of RightScale Self-Service using your desired schedule. Running in Test mode from the Designer screen means that only other users with the Designer role can see or run the policy CATs and it will not show in the Self-Service Catalog for other users. **Do not choose Alert and Delete until you are confident you know what will be deleted.**
