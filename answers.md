# Practical Review

> **Goal**: To prepare for the practical by using the madduck.local Active Directory domain. All questions are derived from the lab workbook.
>

## Getting Started

Please Remote Desktop Protocol **(RDP)** into either DC1 or DC2 to utilize the AD Powershell module. Sign in using your username and password given earlier.

## Question 1 (Domain Enumeration)

1. How many computers exist in the domain?
2. What are the names for all computers in the domain?
3. Please provide at least one computer's SID?
4. (True | False) Computer SIDs are unique for each computer in a Active Directory Domain?

> Solution
>
> 1. The are 4 computers.
>
> ```powershell  
>    $computers = Get-ADComputer -Filter *
>    $computers.Count   
>```
>
> 2. The computers names are **DC1, DC2, pc1**, and **pc2**
>
> ```powershell
>   Get-ADComputer -Filter * | Select-Object Name
>```
>
> 3. Here is the list of all the computers SIDs:
>
> - **DC1 SID:** S-1-5-21-73388235-3422076780-2126075944-1003
> - **DC2 SID:** S-1-5-21-73388235-3422076780-2126075944-1107
> - **pc1 SID:** S-1-5-21-73388235-3422076780-2126075944-2105
> - **pc2 SID:** S-1-5-21-73388235-3422076780-2126075944-1611
>
> ```powershell
>   Get-ADComputer -Filter * | Select-Object Name, SID
>```
>
> 4. **True.** Whenever a computer joins a domain it is assigned a unique SID.
>
## Question 2 (Group Enumeration)

1. How many groups exist in the domain?
2. Please provide at least one group's SID?
3. (True | False) Group SIDs are unique for each group in a Active Directory Domain?
4. How many users are a member of the Domain Admins group?
5. (True | False) Domain Admins only have administrator privileges on domain controllers?

> Solution
>
> 1. There are 48 groups in the domain.
>
> ```powershell
>   $groups = Get-ADGroup -Filter *
>   $groups.Count
> ```
>
> 2. An example SID is **S-1-5-21-73388235-3422076780-2126075944-1104** which is the SID for the DnsAdmins group
> 3. **True**
> 4. There are 10 members in the Domain Admins groups
>
> ```powershell
>   $adminGroup = Get-ADGroupMember -Identity "Domain Admins"
>   $adminGroup.Count
>```
>
> 5. **False.** Domain Admins have administartor privileges on all systems in the domain

## Question 3 (Forest Enumeration)

1. What domain is considered the root domain?
2. (True | False) A domain does not have to exist inside a forest?

> Solution
>
> 1. The root domain is madduck.local
>
> ```powershell
>   Get-ADForest | Select-Object RootDomain
>```
>
> 2. **False**. A domain must exist inside a forest.

## Question 4 (Organizational Units Enumeration)

1. How many OUs exist in the domain?
2. (True | False) Organizational Units are a way to logically organize AD objects in a domain?

> Solution
>
> 1. There are 4 organizational units in the domain
>
> ```powershell
>   $units = Get-ADOrganizationalUnit -Filter *
>   $units.Count
>```
>
> 2. **True**

## Question 5 (Domain Enumeration)

1. How many computers in the domain are using Windows 10?
2. (True | False) A domain controller can use Windows 10?
3. How many computers in the domain are using Windows Server 2022?
4. How many computers in the domain are using Windows Server 2016?

> Solution
>
> 1. There are 2 computers using windows 10. Both pc1 and pc2 are using windows 10.
>
> ```powershell
>  $windows10 = Get-ADComputer -Filter * -Properties * | Where-Object OperatingSystem  -Match "Windows 10"
>  $windows10.Count
>```
>
> 2. **False**. A domain controller must use Windows Server.
> 3. There are 2 computers using Windows Server 2022. Both DC1 and DC2 are using Windows Server 2022.
>
>  ```powershell
>   $server2022 = Get-ADComputer -Filter * -Properties * | Where-Object OperatingSystem  -Match "Windows Server 2022"
>   $server2022.Count
>```
>
> 4. There are 0 computers using Windows Server 2016.
>
> ```powershell
>  $server2016 = Get-ADComputer -Filter * -Properties * | Where-Object OperatingSystem  -Match "Windows Server 2016"
>  $server2016.Count
>```

## Question 6 (User Enumeration)

1. How many users exist in the domain?
2. (True | False) Only Domain Admin users get SIDs?

> Solution
>
> 1. There are 12 users in the domain
>
> ```powershell
>$users = Get-ADUser -Filter *
>$users.Count
>```
>
> 2. **False.** All users get a SID.

## Question 7 (Domain Group Policy Object Enumeration)

1. How many group policy objects exist in the domain?
2. Give one GPO name.

> Solution
>
> 1. There are 3 group policy objects in the domain
>
> ```powershell
>   $gpos = Get-GPO -All
>   $gpos.Count
>```
>
>2. The names of each GPO are:
>
> - Default Domain Policy
> - RDP Connections
> - Default Domain Controllers Policy
>
> ```powershell
> Get-GPO -All | Select-Object DisplayName
>```

## Question 8 (Local Enumeration)
>
> **Hint** Use a interactive Powershell remoting session. Please exit the session after the question.

1. What is the IP Address of PC1?
2. Get the following computer info from PC1?
    1. Bios Caption
    2. Keyboard Layout
    3. OS Name
    4. OS Hot Fixes
3. What DNS server is PC1 using?
4. What is PC1's gateway IP Address?
5. What ports is PC1 listening on?

> Solution
>
> 1. The IP Address of PC1 is 10.0.0.6
>
> ```powershell
> Resolve-DnsName pc1
> ```
>
> 2. Here is the following info
>      1. Hyper-V UEFI Release v4.1
>      2. en-US
>      3. Microsoft Windows 10 Pro
>      4. KB5034466, KB5034468, KB5011048, KB5015684...
>
> ```powershell
> Enter-PSSession pc1
> Get-ComputerInfo | Select-Object BiosCaption, KeyboardLayout, OSName, OSHotfixes    
>```
>
> 3. Using DC1 for its DNS server (10.0.0.4)
>
>```powershell
>Get-NetIPConfiguration
>```
>
> 4. The Gateway IP Address is 10.0.0.1
> 5. Will vary depending how many people are logged in
>
> ```powershell
>Get-NetTCPConnection
>```

## Question 9 (Local Enumeration)
>
> **Hint** Use a interactive Powershell remoting session. Please exit the session after the question.

1. What are the local users on PC1?
2. What is the SID for the best-duck user?
3. By looking at the SID for the best-duck user what information can be established?

> Solution
>
> 1. The local users are **best-duck, DefaultAccount, Guest, and WDAGUtilityAccount.**
>
> ```powershell
> Get-LocalUser
> ```
>
> 2. The SID is **S-1-5-21-2033867447-2489785829-455501711-500**
>
> ```powershell
> Get-LocalUser -Name "best-duck" | Select-Object Name, SID
>```
>
> 3. The best-duck SID is of the form S-1-5-domain-500 therefore it can be concluded that the user is the local administrator. See page 25.

## Question 10 (Local Enumeration)
>
> **Hint** Use a interactive Powershell remoting session. Please exit the session after the question.

1. How many local groups exist on PC1?
2. What users are a member of the local Administrators group?
3. (True | False) The user best-duck has domain administrator privileges?

> Solution
>
> 1. There are 19 local groups on PC1.
>
> ```powershell
> $groups = Get-LocalGroup
> $groups.Count
>```
>
> 2. The user **best-duck** and the group **Domain Admins** are members of the local Administrators group.
>
> ```powershell
> Get-LocalGroupMember -Group "Administrators"
>```
>
> 3. **False.** The user best-duck only has administrator access on pc1.
>
## Question 11 (Network Tools)

1. Ping PC1
2. What is the IP Address of PC1?
3. How many router hops is PC1 away?
4. Based on the router hops what can be concluded about PC1's subnet?

> Solution
>
> ```powershell
> Test-NetConnection -ComputerName pc1
>```
>
> 2. Pinging PC1 gives the IP Address of 10.0.0.6
> 3. PC1 is only 1 hop away.
>
> ```powershell
> tracert.exe pc1
>```
>
> 4. PC1 and DC1 are in the same subnet. Moreover they share the same gateway router

## Question 12 (Local Enumeration)

1. On PC1 what is the status of the WSearch service?
2. Please Restart the WSearch service

> Solution
>
> 1. The WSearch service is running
>
> ```powershell
> Get-Service -Name WSearch
>```
>
> To restart the WSearch service use the following command
>
> ```powershell
> Restart-Service -Name WSearch
> ```

## Question 13 (Local Enumeration)

1. What is PID for the system process on PC1?

> Solution
>
> 1. The PID for the system process is 4
>
> ```powershell
> Get-Process -Name System
>```
>
## Question 14 (Local Effects)

1. Create a scheduled task that opens calc.exe every minute. Please name the task 1minCalc\<YOUR NAME>
2. Wait for the scheduled task to perform its action
3. Delete the scheduled task

> Solution
> The following commands will create the scheduled task
>
> ```powershell
> $action = New-ScheduledTaskAction -Execute "calc.exe"
> $trigger = New-ScheduledTaskTrigger -Once -At (Get-Date) -RepetitionInterval (New-TimeSpan -Minutes 1)
> $task = New-ScheduledTask -Action $action -Trigger $trigger
> Register-ScheduledTask -TaskName "1minCalc$env:USERNAME" -InputObject $task
> Start-ScheduledTask -TaskName "1minCalc$env:USERNAME"
> ```
>
> 2. The calculator program should appear
> 3. The following command will delete the task
>
> ```powershell
> Unregister-ScheduledTask -TaskName "1minCalc$env:USERNAME"
>```

## Question 15 (Local Admin)

1. Create a process from the explorer.exe program
2. Kill that process using its PID
3. (True | False) A program can only have one running process?

> Solution
>
> 1. The following command will start the explorer program
>
> ```powershell
> Start-Process explorer.exe
>```
>
> 3. **False**. A program can create many running processes.
>
## Question 15 (Local Enumeration)

1. On PC1 what is the value of the BEST_SHOW environment variable?

> Solution
>
> 1. The value of the BEST_SHOW environment variable is Rick and Morty.
>
> ```powershell
> Invoke-Command -ComputerName PC1 -ScriptBlock {$env:BEST_SHOW}
>```
>
## Question 16 (Local Enumeration)

1. What is the absolute path of ntoskrnl.exe?
2. What is the absolute path of powershell.exe?

> Solution
>
> 1. The absolute path is C:\Windows\System32\ntoskrnl.exe
>
> ```powershell
> Get-Command ntoskrnl.exe
>```
>
> 2. The absolute path is C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
>
> ```powershell
>  Get-Command powershell.exe
>```
>
## Question 17 (Local Enumeration)

1. Find the OS's installer service (installservice)
2. What is the state/status and start type of the installservice?

> Solution
> The service is stopped. The start type is manual.
>
> ```powershell
> Get-Service -Name installservice | Select-Object Name, Status, StartType
>```
>
## Question 18 (Registry)

> Use the following  registry path: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC

1. What service does W3SVC depend on?
2. What is the image path for this service?

> Solution
> Use the regedit.exe GUI to view details
>
> 1. Depends on the WAS HTTP service.
> 2. The image path is %windir%\system32\svchost.exe -k iissvcs

## Question 19 (File ACLs)
>
> Use the following file share path \\\DC1\Share
>

1. How many files exist in this share? (Don't forget to include hidden files)
2. What is the contents of the The Raven.txt
3. Who has access to The Raven.txt file?

> Solution
>
> 1. There are 2 files on the share.
> 2. The Raven by Edgar Allan Poe
> 3. The following command will show who has access
>
> ```powershell
> Get-Acl '.\The Raven.txt' | Select-Object -ExpandProperty Access
>```
>
## Question 20 (File Attributes)

1. What are the file attributes of The Raven.txt
2. What are the file attribute of the nuke-codes.txt
3. (True | False) The file nuke-codes.txt is visible in the file explorer?

> Solution
>
> 1. A - Archive
>
> ```powershell
> attrib.exe '.\The Raven.txt'
> ```
>
> 2. A - Archive and H - Hidden
>
> ```powershell
> attrib.exe '.\nuke-codes.txt'
> ```
>
> 3. **False.** Since the file is hidden it will not appear in the file explorer by default.
