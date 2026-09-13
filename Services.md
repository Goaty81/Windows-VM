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

Startup type: Manual

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

Get-Service <ServiceName>

<img width="1022" height="817" alt="image" src="https://github.com/user-attachments/assets/4e5fb1aa-1165-4cbf-bc7e-655919f61554" />

Result:

The service is running and ready to use

4. Break / Fault Introduced

What did I change?

I stopped a service from running preventing its operations and operations of those reliant on it.

Command/action used:

# Command used here

Expected impact:

5. Symptoms

What stopped working?

Error message:

Paste error here

Observed service status:

Get-Service <ServiceName>

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
Get-WinEvent -LogName System -MaxEvents 30

Relevant event/error:

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
