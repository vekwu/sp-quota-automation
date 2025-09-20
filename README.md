# sp-quota-automation
# SharePoint Online Storage Quota Automation

## 📌 Project Overview
This project demonstrates how to **automate storage quota enforcement** across thousands of SharePoint Online site collections using PowerShell.

An enterprise customer with **10,000+ SharePoint sites** needed to enforce a **10GB storage quota** per site to optimize storage usage and improve operational efficiency.  
Since the SharePoint Online GUI does not scale well for such a task, I developed a **PowerShell script** that automated this process in minutes.

---

## 🚀 Problem Statement
- Large enterprise customer with **10,000+ SharePoint sites**.
- IT required each site to be limited to **10GB** with a **9GB warning threshold**.
- Manual/GUI configuration was **time-consuming, error-prone, and not scalable**.

---

## 🔧 Solution Approach
1. Researched SharePoint Online quota documentation.  https://learn.microsoft.com/en-us/powershell/module/microsoft.online.sharepoint.powershell/set-sposite?view=sharepoint-ps
 
2. Wrote a PowerShell script leveraging the **SharePoint Online Management Shell**.  
3. Tested in a sandbox environment before deploying in production.  
4. Automated quota enforcement across all sites within **minutes**.

---

## 💻 Script

```powershell
# Import the SharePoint Online module
Import-Module Microsoft.Online.SharePoint.PowerShell -DisableNameChecking

# Set SharePoint Online admin URL and credentials
$adminUrl = "https://yourtenant-admin.sharepoint.com/"
$credential = Get-Credential

# Connect to SharePoint Online
Connect-SPOService -Url $adminUrl -Credential $credential

# Define storage limits (in MB)
$storageLimitMB = 10000
$storageWarningMB = 9000

# Get all site collections
$siteCollections = Get-SPOSite -Limit All

# Apply quota settings to each site
foreach ($site in $siteCollections) {
    Set-SPOSite -Identity $site.Url -StorageQuota $storageLimitMB -StorageQuotaWarningLevel $storageWarningMB
    Write-Host "Quota set: $($site.Url)"
}

# Disconnect from SharePoint Online
Disconnect-SPOService
