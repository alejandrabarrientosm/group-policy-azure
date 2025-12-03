<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory - Group Policy and Managing Accounts in Azure (Virtual Machine)</h1>

This tutorial we are going to be dealing with accounts, enable and unlocking in client-1 (Virtual Machine)<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Dealing with Account Lockouts
- Configuring Active Directory using Group Policy
  
<h2>Dealing with Account Lockouts</h2>

Get logged into dc-1
<p align left>
<img width="302" height="185" alt="image" src="https://github.com/user-attachments/assets/70390a6e-8dec-48d3-89ee-650ef4184ea9" />
<img width="336" height="323" alt="image" src="https://github.com/user-attachments/assets/709ea71d-2618-4ed3-8889-f515695c805f" />
<img width="287" height="300" alt="image" src="https://github.com/user-attachments/assets/5671c9f6-fb87-4175-b79f-2153091195c4" />

Pick a random user account you created previously. Attempt to log in with it 10 times with a bad password, after 10 attempts the account should be locked but because Group Policy was not setup in dc-1, we were able to log in using the correct password. 
<p align left>
<img width="331" height="364" alt="image" src="https://github.com/user-attachments/assets/64fc3e34-f1a5-4fe3-9f26-d9ec597a2f85" />

<h2>Configuring Active Directory using Group Policy</h2>

Configure Group Policy to Lockout the account after 5 attempts. Open the Group Policy Management Console (GPMC)
Log in to a machine with Group Policy Management Console installed (typically, a Domain Controller).
Click Start, and type gpmc.msc in the search box, then press Enter. This opens the Group Policy Management Console.
<p align left>
<img width="579" height="411" alt="image" src="https://github.com/user-attachments/assets/4043a618-852b-4a80-8dd2-22162ccb57c4" />

Create or Edit a Group Policy Object (GPO)
In the GPMC, navigate to the Group Policy Objects section.
Right-click Group Policy Objects and select New to create a new GPO, or right-click an existing GPO and select Edit to modify it.
Give the new GPO a descriptive name if you're creating a new one, like "Account Lockout Policy".
<p align left>
<img width="1438" height="860" alt="image" src="https://github.com/user-attachments/assets/8f3c8cb6-b787-4f39-b173-5e1c7968a3c8" />

Click on Edit,
<p align left>
<img width="1177" height="785" alt="image" src="https://github.com/user-attachments/assets/ee7eb959-a19b-432c-9c57-6bc5dc53c4ad" />

Navigate to the Account Lockout Policy Settings
In the Group Policy Management Editor, expand the following:
Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.
<p align left>
<img width="1490" height="673" alt="image" src="https://github.com/user-attachments/assets/daccb7b1-f199-47ec-9264-509bc731825f" />


Configure Account Lockout Policy Settings
You will see three primary settings that you need to configure:
Account Lockout Duration:
Definition: The time in minutes that an account remains locked before it is automatically unlocked.
Configuration: Double-click on this setting, select Define this policy setting, and then set the duration (e.g., 30 minutes).
Account Lockout Threshold:
Definition: The number of failed logon attempts that will trigger an account lockout.
Configuration: Double-click on this setting, select Define this policy setting, and then set the threshold (e.g., 3 invalid attempts).
Reset Account Lockout Counter After:
Definition: The time in minutes after which the failed logon attempts counter is reset to 0, assuming there are no additional failed logon attempts.
Configuration: Double-click on this setting, select Define this policy setting, and then set the time (e.g., 15 minutes).
<p align left>
<img width="421" height="364" alt="Screenshot 2025-12-01 164206" src="https://github.com/user-attachments/assets/ebd99332-b929-49fb-b4a5-3dd9d897de9a" />
<img width="894" height="297" alt="image" src="https://github.com/user-attachments/assets/4eaaa591-ff23-431c-94c1-99ab601ce9ab" />


Update Group Policy
You can wait for the Group Policy to propagate automatically, or you can force an update immediately.
On a client machine or server, open Command Prompt and type gpupdate /force, then press Enter.
Log into client-1 using jane_admin to force the policy.
<p align left>
<img width="652" height="413" alt="image" src="https://github.com/user-attachments/assets/398faa7c-5267-4282-98d7-98fde9ee8ed7" />

After setting Group Policy in client-1, log back in with your random user and password, this time try 7 times with a bad password. 
<p align left>
<img width="322" height="266" alt="image" src="https://github.com/user-attachments/assets/ffed4654-6fd8-4d0d-a312-0c8dbeab1e1b" />

After 7 attemps failed you should get this message. Which means that our policy worked
<p align left>
<img width="414" height="113" alt="image" src="https://github.com/user-attachments/assets/6febf93a-dafc-4c7a-ba9d-5084f9a3c78c" />

Go back to dc-1 and if for some reason you forget the name of the account just right click on mydomain.com>click on find> type the username and you'll be able to find the correct account
<p align left>
<img width="1246" height="656" alt="image" src="https://github.com/user-attachments/assets/4669d4cb-675d-4618-84ed-7723d1f862e5" />

Click on Account, then click on Unlock account. This account is currently locked oput on this Active Directory Domain Controller, click apply and ok. Then try again to log in client-1 with the correct password and it should be unlocked.
<p align left>
<img width="287" height="376" alt="Screenshot 2025-12-01 170911" src="https://github.com/user-attachments/assets/efe592b3-090e-42dd-9d73-f54aeaec59bf" />
<img width="337" height="237" alt="image" src="https://github.com/user-attachments/assets/66bce95e-a53f-425d-ab4b-3616fa4219df" />


Enabling and Disabling Accounts
Disable the same account in Active Directory, rigth click on the user account and choose Disable Account
<p align left>
<img width="770" height="792" alt="image" src="https://github.com/user-attachments/assets/ec9c9c6d-34c9-4015-9542-cada2b14daa4" />

Attempt to login with it, observe the error message
<p align left>
<img width="333" height="321" alt="image" src="https://github.com/user-attachments/assets/b6bcd114-d3ab-4700-90e1-a48b8a871711" />
<img width="415" height="100" alt="image" src="https://github.com/user-attachments/assets/90e362a8-2dad-4e97-ac9f-22fb2601e336" />


Re-enable the account in dc-1 and attempt to login with it (right click on it and enable account)
<p align left>
<img width="758" height="790" alt="image" src="https://github.com/user-attachments/assets/182053c2-5d43-4d70-901b-698ea0325514" />
<img width="331" height="319" alt="image" src="https://github.com/user-attachments/assets/e90ab776-313b-4840-b061-8c1f467213f2" />
<img width="288" height="301" alt="image" src="https://github.com/user-attachments/assets/cb685451-1d0a-426e-9b2a-ff9e72585222" />

Observing Logs
Observe the logs in the Domain Controller
<p align left>
<img width="361" height="93" alt="image" src="https://github.com/user-attachments/assets/7ed065a8-a27d-4fcf-a05f-6a9bfe0b6179" />
<img width="1558" height="949" alt="image" src="https://github.com/user-attachments/assets/45d08b2a-79ec-4e7b-b498-24f2a22aba48" />


Observe the logs on the client Machine. In this case the access was denied due to dog.tunap is not an administrator
<p align left>
<img width="1419" height="484" alt="image" src="https://github.com/user-attachments/assets/da514521-3d8c-4b31-9fdc-e20559890d1a" />

To view this as an administrator you can, type eventvwr.msc and run it as an adminstrator, and if your know the credentials of the administrator you will be able to see the logs
<p align left>
<<img width="338" height="352" alt="image" src="https://github.com/user-attachments/assets/007cb3dc-798c-4b68-8298-fa604324eb17" />
<<img width="1565" height="968" alt="image" src="https://github.com/user-attachments/assets/b615b4b5-f919-40b2-9f1d-67252cc070b0" />

And this conclude our lab!

















