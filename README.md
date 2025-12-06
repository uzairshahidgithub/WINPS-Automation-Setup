# Windows PowerShell Automated ScreenFetch Setup with Update & Upgrade Commands for Daily Use
A simple and clean setup to make your PowerShell look cooler with **screenfetch**, while adding Linux-inspired commands like `update` & `upgrade` for Windows.

<img width="1118" height="627" alt="image" src="https://github.com/user-attachments/assets/c5afd22e-852f-45f7-8a26-6ed129b28316" />

## Requirements
* Windows PowerShell
* Internet connection (first-time install of update module)
  
## Step 1: Ensure ScreenFetch Works
If you can already type `screenfetch` and it runs normally, great move to Step 2. If screenfetch is **not installed**, install using:
```powershell
Install-Module -Name windows-screenfetch
```
Verification:
```powershell
# Check if installed
Get-Command screenfetch

# Test it
screenfetch
```

## Step 2: Create / Open PowerShell Profile
Check if your PowerShell profile already exists:

```powershell
Test-Path $PROFILE
```
If result: **False**, create one:

```powershell
New-Item -ItemType File -Path $PROFILE -Force
```

Open the profile:
```powershell
notepad $PROFILE
```

## Step 3: Paste the Code Below in Your $PROFILE
Paste this entire block:
```powershell
try { screenfetch } catch {}
$updateModuleLoaded = $false
function update {
    if (-not $updateModuleLoaded) {
        if (-not (Get-Module -ListAvailable PSWindowsUpdate)) {
            Install-Module PSWindowsUpdate -Force -Scope CurrentUser
        }
        Import-Module PSWindowsUpdate
        $updateModuleLoaded = $true
    }
    Get-WindowsUpdate
}
function upgrade {
    if ([Security.Principal.WindowsPrincipal]::new(
        [Security.Principal.WindowsIdentity]::GetCurrent()
    ).IsInRole([Security.Principal.WindowsBuiltInRole]::Administrator)) {
        update
        Install-WindowsUpdate -AcceptAll -AutoReboot
    } else {
        Write-Host "Run PowerShell as Administrator!" -ForegroundColor Red
    }
}
```
## Step 4: Install the module:
```powershell 
Install-Module PSWindowsUpdate -Force -Scope CurrentUser
```
## Step 5: Load your profile:
Reload profile to activate commands
```powershell
. $PROFILE
```
Save & Close then **restart PowerShell**.
## How To Use
| Command       | Purpose                              |
| ------------- | ------------------------------------ |
| `screenfetch` | Show system info with ASCII logo     |
| `update`      | Check for Windows updates            |
| `upgrade`     | Install updates & reboot if required |

⚠ upgrade must be run in **Administrator PowerShell**

## Summary
This setup adds:
* Automatic system info display
* Linux-style update commands
* Clean PowerShell customization

## Authors
- [Muhammad Uzair](https://www.github.com/uzairshahidgithub)

## Support
For support, email uzairrshahid@gmail.com or join our Discord Community.
