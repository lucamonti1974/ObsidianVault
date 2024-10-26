**How to use PowerShell script to unblock files**

1. Open an administrative PowerShell prompt.
2. Copy & Paste the below script.
3. Edit the variable to the desired directory
4. Execute script.
 
``` Powershell
#Edit $Path to be the directory you would like the script to run on.
# Examples would be "C:\Program Files\LANDesk\ManagementSuite" or "C:\Users\Admin\Downloads"

$Path = "C:\Program Files\LANDesk"
Get-ChildItem "PATH" -Recurse | Unblock-File

```
