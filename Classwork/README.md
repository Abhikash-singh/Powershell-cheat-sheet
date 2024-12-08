
  Classwork 1 - PowerShell Basics

Summary of PowerShell Basics
In this classwork, I used PowerShell to navigate folders, manage files, and check system information. I learned how to use Get-Help for information on cmdlets and Get-Command to see all available cmdlets.



Classwork 2 - Using Get-Help to Learn PowerShell Cmdlets
Objective
I learned bout important PowerShell cmdlets using the Get-Help cmdlet to understand what they do, how to use them, and their syntax.

Summary
In this classwork, I used the Get-Help cmdlet to learn about 10 key PowerShell cmdlets. I found out what each cmdlet does, how to use it, and examples of how to use them in real life. Here are the cmdlets with their details:



Classwork 3- PowerShell Pipelines and Data Manipulation
Objective
I Learned how to use PowerShell pipelines and key commands like Import-CSV, Export-CSV, Get-Content, ConvertTo-HTML, and Out-File to manipulate and format data.

Summary
In this classwork, I focused on PowerShell pipelines. I learned how to handle CSV files, process services, read and export files, and stop processes. I also explored the differences between Get-Content and Import-CSV, and how to format data as HTML and CSV.



Classwork 4: PowerShell Commands Analysis
This document explains if the given PowerShell commands will work as intended and provides simple explanations for each.

Display services from all computers in the domain
Command:
Get-ADComputer –Filter * | Get-Service –Name *
Will it work? No
Why not?
Problem: Get-Service needs a computer name, but Get-ADComputer outputs objects, not names.
Solution: Extract computer names and pass them to Get-Service using ForEach-Object like this:
Get-ADComputer –Filter * | ForEach-Object { Get-Service -ComputerName $_.Name }
Display services from all computers in the domain
Command:
Get-ADComputer –Filter * | 
Select-Object –Property @{n='ComputerName';e={$_.Name}} | 
Get-Service –Name *
Will it work? No
Why not?
Problem: Select-Object creates a custom property ComputerName, but Get-Service does not recognize it.
Solution: Use ForEach-Object to extract Name directly and pass it to Get-Service:
Get-ADComputer –Filter * | 
Select-Object –ExpandProperty Name | 
ForEach-Object { Get-Service -ComputerName $_ }
Display WMI class from all computers in the domain
Command:
Get-ADComputer –Filter * | 
Select-Object –Property @{n='ComputerName';e={$_.Name}} | 
Get-WmiObject –Class Win32_BIOS
Will it work? No
Why not?
Problem: Get-WmiObject needs the -ComputerName parameter, but it’s missing here.
Solution: Extract the Name from Get-ADComputer and pass it to Get-WmiObject:
Get-ADComputer –Filter * | 
Select-Object –ExpandProperty Name | 
ForEach-Object { Get-WmiObject -Class Win32_BIOS -ComputerName $_ }
Display WMI class from computers in Computers.txt
Command:
Get-WmiObject –Class Win32_BIOS –ComputerName (Get-Content Computers.txt)
Will it work? Yes
Why it works:
Reason: Get-Content reads the file Computers.txt line by line and returns an array of computer names.
What happens: PowerShell passes this list to -ComputerName, so Get-WmiObject runs on each computer. No changes needed.
Display all processes from all computers in the domain
Command:
Get-Process –Name * -ComputerName (Get-ADComputer –Filter * | Select-Object –Expand Name)
Will it work?  No
Why not?
Problem: Get-Process doesn’t accept a list of computer names inside parentheses.
Solution: Use ForEach-Object to send each computer name one at a time:
Get-ADComputer –Filter * | 
Select-Object –ExpandProperty Name | 
ForEach-Object { Get-Process -Name * -ComputerName $_ }
Display Security event log from all computers in Computers.csv
Command:
Import-CSV Computers.CSV | Get-EventLog –LogName Security
Will it work?  No
Why not?
Problem: Get-EventLog needs the -ComputerName parameter, but it’s missing here.
Solution: Pass ComputerName from Import-CSV to Get-EventLog like this:
Import-CSV Computers.csv | 
ForEach-Object { Get-EventLog -LogName Security -ComputerName $_.ComputerName }



Classwork 5: PowerShell Remoting

Summary
In this assignment, I worked with PowerShell remoting to manage remote systems. I learned how to enable remoting, create persistent sessions, list modules, and run remote commands, such as retrieving event logs and managing network adapters. I also practiced closing sessions and using interactive remoting with Enter-PSSession.

Enable PSRemoting
Command:
Enable-PSRemoting -Force
What I did: Enabled remoting on all virtual machines (or host).
Using Invoke-Command for Event Logs
Command:
Invoke-Command -ComputerName 'VM1', 'VM2' -ScriptBlock { Get-EventLog -LogName Security -Newest 100 }
What I did: Retrieved the 100 most recent security events from remote machines.
Create Persistent Session
Command:
$remote = New-PSSession -ComputerName 'VM1'
What I did: Created a persistent session with VM1.
List Modules Remotely
Command:
Get-Module -ListAvailable -PSSession $remote
What I did: Listed available modules on VM1.
Import NetAdapter Module
Command:
Import-Module -PSSession $remote -Name NetAdapter
What I did: Imported the NetAdapter module from VM1.
Run Get-NetAdapter Remotely
Command:
Get-RemoteNetAdapter
What I did: Listed network adapters on VM1 remotely.
Close PSSessions
Command:
Get-PSSession | Remove-PSSession
What I did: Closed all active sessions.
Interactive Session with Enter-PSSession
Command:
Enter-PSSession -ComputerName 'VM1'
What I did: Started an interactive session with VM1 and then exited.



Classwork 6: CIM/WMI Cmdlets
Summary
In this lab, I used CIM/WMI cmdlets to gather system information:

List CIM Classes: I listed CIM classes in the Microsoft namespace with "Registration" in the name.
Show Methods: I displayed methods for the win32_networkadapterconfiguration CIM class.
Identify Network Adapters: I identified network adapters with DHCP enabled.
Query Installed Hotfixes: I queried installed hotfixes using CIM.
These tasks helped me understand how to query and manage system data using CIM/WMI cmdlets.



Classwork 7: Variables and Operators in PowerShell

Summary
In this classwork, I practiced working with variables, subexpressions, and string manipulation in PowerShell.

Display Defined Variables: I learned how to list all currently defined variables using Get-Variable.
Create and Assign Variables: I created a variable $x and assigned it a value (e.g., 100).
Remove Variables: I removed a variable using Remove-Variable to clean up.
Store Processes in a Variable: I stored the list of running processes in the $procs variable using Get-Process.
Display First Object in $procs: I displayed the first object in $procs.
Access First Object’s Property: I accessed the name of the first process object in $procs.
Create a Message: I created a message with a variable value using a subexpression.
String Replacement: I used the -replace operator to replace a string within a variable.
This classwork helped me understand variable management, string manipulation, and working with system processes in PowerShell.












