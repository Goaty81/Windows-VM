Here i will be troubleshooting services through the windows VM!

1. Lab Information

Date: 07.09.26

VM: Windows

Windows Version: Windows 10 Pro

Service: Printer Spooler

2. Objective

To understand the service dependencies and troubleshoot the services that fail to start.

3. Initial State

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

Get-Service <ServiceName> | Format *

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

<img width="1023" height="817" alt="image" src="https://github.com/user-attachments/assets/3dd8bdc3-63a3-48f7-9308-7c38a61c3580" />


Observed service status:



Result:

6. Investigation
Step 1 — Check service status
Get-Service <ServiceName>

Finding:

Step 2 — Check dependencies
Get-Service <ServiceName> -RequiredServices

Finding:

Step 3 — Check dependent services
Get-Service -DependentServices <ServiceName>

Finding:

Step 4 — Check Windows events

Relevant event/error:

<img width="1023" height="817" alt="image" src="https://github.com/user-attachments/assets/5440d623-f008-4a66-a9cf-47d71f2fdd37" />


7. Root Cause

What caused the failure?




Evidence supporting the conclusion:




8. Fix

Action taken:

# Fix command here

Why this fixes the problem:




9. Verification

Service status after repair:

Get-Service <ServiceName>

Result:

Application/functionality tested:

Result:

10. Final State

Service status:
Startup type:
Dependencies healthy: Yes / No
Functionality restored: Yes / No

11. Lessons Learned

What did I learn?








What would I check first next time?




Difficulty: Easy / Medium / Hard

Time to diagnose:
Time to fix:
