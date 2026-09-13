Here i will be troubleshooting services through the windows VM!

<ins>1. Lab Information</ins>

Date: 07.09.26

VM: Windows

Windows Version: Windows 10 Pro

Service: Printer Spooler

<ins>2. Objective</ins>

To understand the service dependencies and troubleshoot the services that fail to start.

<ins>3. Initial State</ins>

Service name: Spooler

Display name: Print Spooler

Current status: Running

Startup type: Automatic

Log On As: Local System

Dependencies

Services this service depends on:

 - Fax

<img width="1023" height="778" alt="image" src="https://github.com/user-attachments/assets/dfd2326a-7094-4cda-a8ad-9c187033e5eb" />

Services that depend on this service:

- Remote Procedure Call
- Hyper Text Transfer Protocol (HTTP)

<img width="1023" height="780" alt="image" src="https://github.com/user-attachments/assets/a3d4f5e9-5509-459e-8ada-7e91f5df24c5" />

Verification

Get-Service <ServiceName> | Format-List *

<img width="1023" height="779" alt="image" src="https://github.com/user-attachments/assets/c0ded5f1-530d-49f3-85ce-c49c118edf5f" />

Result:

Name : Spooler 

DisplayName : Print Spooler 

Status : Running 

StartType : Automatic 

ServicesDependedOn : {RPCSS} {HTTP}

DependentServices : {FAX} 

CanStop : True 

CanPauseAndContinue : False

<img width="1023" height="779" alt="image" src="https://github.com/user-attachments/assets/ea18a4b2-5c73-4443-9dd9-7d50e0e8b533" />

4. Break / Fault Introduced

What did I change?

I stopped a service from running preventing its operations and operations of those reliant on it.

Command/action used:

Stop-Service Spooler

<img width="1022" height="780" alt="image" src="https://github.com/user-attachments/assets/f387a623-2a9a-4c79-94ef-52f927c0d697" />

Expected impact:

By stopping this service i expected that the windows OS will be prevented from carrying out printer services

5. Symptoms

- Printer services stop running
- OS unable to print
- Service shows as stopped

Error message:

<img width="1016" height="820" alt="image" src="https://github.com/user-attachments/assets/4d8b6bad-88c1-4401-be11-661c7835b65d" />

<img width="1023" height="777" alt="image" src="https://github.com/user-attachments/assets/ca403ea5-f37f-429a-bb27-08753bf73ffa" />

Observed service status:

<img width="1022" height="780" alt="image" src="https://github.com/user-attachments/assets/4c332811-ea49-47d0-9d0a-59ace2905748" />

Result:

6. Investigation

Step 1 — Check service status

Get-Service Spooler

<img width="1023" height="782" alt="image" src="https://github.com/user-attachments/assets/05572590-741c-4966-99e6-28b183761e24" />

Finding:

Service has stopped running.

Step 2 — Check dependencies

Get-Service -Name Spooler -RequiredServices

<img width="1024" height="782" alt="image" src="https://github.com/user-attachments/assets/fc78180f-f5be-4c05-a73f-da9e10e5c9c8" />

Finding:

Required services are still up as they where not affected by the print spooler being stopped.

Step 3 — Check dependent services

Get-Service -Name Spooler -DependentServices

<img width="1023" height="781" alt="image" src="https://github.com/user-attachments/assets/6ad82add-843f-4fd4-878b-c6caabdb66b5" />

Finding:

Fax Service was stopped due to being dependant on the print spooler being able to run.

Step 4 — Check Windows events

Relevant event/error:

<img width="1023" height="817" alt="image" src="https://github.com/user-attachments/assets/5440d623-f008-4a66-a9cf-47d71f2fdd37" />

7. Root Cause

What caused the failure?

The failure was cause by the printer spooler service from being stopped, meaning that the OS could not use printer or fax services.
I confirmed this by using the below command to see that the service was stopped.

Get-Service | Where-Object {$_.Status -eq "Stopped"} | Select-Object -First 125

Evidence supporting the conclusion:

<img width="1025" height="781" alt="image" src="https://github.com/user-attachments/assets/5f6fa5b6-6a71-46a5-87cf-5dfe84b8bcaa" />

8. Fix

Action taken:

Started the service back up

Start-Service Spooler

<img width="1023" height="818" alt="image" src="https://github.com/user-attachments/assets/d2b0b57d-0cdc-4d00-a58c-c55662a0f24c" />

Why this fixes the problem:

This will Reboot the Service and will allow the OS to begin normal printer operations.

9. Verification

Service status after repair:

Get-Service Spooler

Result:

Printer Operations back up and running on the OS.

10. Final State

Service status: Running
Startup type: Automatic
Dependencies healthy: Yes
Functionality restored: Yes

11. Lessons Learned

What did I learn?

Using the CMD prompt for most things rather than searching for them and going through the more "GUI" route is quicker!

Stopping services can have a detrimental affect to an organisations day to day taskings, therefore quick and efficient outcomes need to be remedied

What would I check first next time?

Use the command:

Get-Service | Where-Object {$_.Status -eq "Stopped"} | Select-Object -First 125

When finding and error and see if the service related to the error has stopped running. Doing this can also speed up time rather than finding it in the Services document.
