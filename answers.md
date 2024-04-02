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
## Question 2 (Group Enumeration)

1. How many groups exist in the domain?
2. Please provide at least one group's SID?
3. (True | False) Group SIDs are unique for each group in a Active Directory Domain?
4. How many users are a member of the Domain Admins group?
5. (True | False) Domain Admins only have administrator privileges on domain controllers?

## Question 3 (Forest Enumeration)

1. What domain is considered the root domain?
2. (True | False) A domain does not have to exist inside a forest?

## Question 4 (Organizational Units Enumeration)

1. How many OUs exist in the domain?
2. (True | False) Organizational Units are a way to logically organize AD objects in a domain?

## Question 5 (Domain Enumeration)

1. How many computers in the domain are using Windows 10?
2. (True | False) A domain controller can use Windows 10?
3. How many computers in the domain are using Windows Server 2022?
4. How many computers in the domain are using Windows Server 2016?

## Question 6 (User Enumeration)

1. How many users exist in the domain?
2. (True | False) Only Domain Admin users get SIDs?

## Question 7 (Domain Group Policy Object Enumeration)

1. How many group policy objects exist in the domain?
2. Give one GPO name.

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

## Question 9 (Local Enumeration)
>
> **Hint** Use a interactive Powershell remoting session. Please exit the session after the question.

1. What are the local users on PC1?
2. What is the SID for the best-duck user?
3. By looking at the SID for the best-duck user what information can be established?

## Question 10 (Local Enumeration)
>
> **Hint** Use a interactive Powershell remoting session. Please exit the session after the question.

1. How many local groups exist on PC1?
2. What users are a member of the local Administrators group?
3. (True | False) The user best-duck has domain administrator privileges?

## Question 11 (Network Tools)

1. Ping PC1
2. What is the IP Address of PC1?
3. How many router hops is PC1 away?
4. Based on the router hops what can be concluded about PC1's subnet?

## Question 12 (Local Enumeration)

1. On PC1 what is the status of the WSearch service?
2. Please Restart the WSearch service

## Question 13 (Local Enumeration)

1. What is PID for the system process on PC1?

## Question 14 (Local Effects)

1. Create a scheduled task that opens calc.exe every minute. Please name the task 1minCalc\<YOUR NAME>
2. Wait for the scheduled task to perform its action
3. Delete the scheduled task

## Question 15 (Local Admin)

1. Create a process from the explorer.exe program
2. Kill that process using its PID
3. (True | False) A program can only have one running process?

## Question 15 (Local Enumeration)

1. On PC1 what is the value of the BEST_SHOW environment variable?

## Question 16 (Local Enumeration)

1. What is the absolute path of ntoskrnl.exe?
2. What is the absolute path of powershell.exe?

## Question 17 (Local Enumeration)

1. Find the OS's installer service (installservice)
2. What is the state/status and start type of the installservice?

## Question 18 (Registry)

> Use the following  registry path: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC

1. What service does W3SVC depend on?
2. What is the image path for this service?

## Question 19 (File ACLs)
>
> Use the following file share path \\\DC1\Share
>

1. How many files exist in this share? (Don't forget to include hidden files)
2. What is the contents of the The Raven.txt
3. Who has access to The Raven.txt file?

## Question 20 (File Attributes)

1. What are the file attributes of The Raven.txt
2. What are the file attribute of the nuke-codes.txt
3. (True | False) The file nuke-codes.txt is visible in the file explorer?