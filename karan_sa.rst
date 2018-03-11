***********************
Karan Configuration SA
***********************


Overview
*********

In the lab participants will deploy a Windows 2012 Server Guest VM and provision it with Karan which is used as a proxy for Calm automation.

Estimated time to complete: **30mins**

**Product Feature Resource(s)**

- slack: #calm
- PdM:  Jasnoor Gill
- Solutions: Mark Lavi, Andy Schmid


Configure Guest VM
******************
Using Windows with Calm

In order for Calm to work with Windows virtual machines, an additional step is required. After deploying Prism Central and completing the configuration of Calm itself, an additional virtual machine must be deployed that runs the ‘Karan’ service.

What is Karan? Put simply, Karan is a component that allows the Calm services to run PowerShell scripts on Windows virtual machines. Think of Karan as a “translation” layer between Calm and the Windows VMs.

.. note:: The official reference for deploying Karan is located in the nutanix_documentation_. This guide is not intended as a replacement for the official documentation.


Configure Karan
******************

Deploying Karan

The steps for deploying Karan are as follows.

- Download the Karan Installer. The link referenced is correct as at January 2018.
- Deploy a Windows virtual machine running Windows 2012 R2 or Windows 2016 RTM
- Enable PowerShell remote execution on the Karan Windows VM:
.. note:: enable-psremoting
- Set the PowerShell Execution Policy:
.. note:: set-executionpolicy remotesigned
- Set all target machines as trusted machines on the Karan host:
.. note:: set-item wsman:\localhost\Client\TrustedHosts -Value *
- Run the Karan installer and, when prompted, populate the fields as follows.
- Select http or https as required
- Set the port to 8090 (note that this must not be changed and port 8090 must be allowed through the Windows firewall on both host and client VMs)
- Set the number of Karan instances (leave as 1 for demo environments)
- Enter the hostname or IP address of the Karan instance. This hostname or IP address must be accessible from the Calm/Prism Central VM!
- Set the gateway UUID to:
.. note:: 2067b70d-bd3f-4b3d-9d82-3add93f30a0a
- Enter the Prism Central VM IP hostname or IP address, as follows:
.. note:: http://<prism_central_hostname_or_ip_address>:8090
.. note:: Don't forget to specify the port, as per the example above! 
- Click Next
- Specify the account information (for demo environments, the Karan VM’s local administrator account is OK)
- Complete the wizard until Karan is installed
- After installation, start the Karan service from the Windows Services application:
.. note:: services.msc

Configuring Windows target VMs

For Karan to have access to the Windows target/client VMs, the following commands must be run. In most cases, these commands would be run as part of preparing a Windows image for use with Sysprep.
.. note:: enable-psremoting      |       set-executionpolicy remotesigned

Using Karan

Karan itself isn’t ‘used’ in the traditional sense i.e. there’s no Karan ‘application’. By installing Karan and having it available for Calm itself to use, PowerShell scripts will be automatically ‘proxied’ through the Karan instance, when required.

.. note:: When deploying or working with Windows VMs from Calm, the only change that is required is to set the operating system to Windows, as opposed to Linux (the default).  


Takeaways
*********

Congratulations you have successfully configured a guest VM and Karan! 


_nutanix_documentation:: https://portal.nutanix.com/#/page/docs/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v10:nuc-installing-karan-service-t.html
