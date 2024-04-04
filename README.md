# PowerShell Automation for User Account Creation in Active Directory

Welcome to this step-by-step tutorial where you will learn to automate the creation of user accounts in Active Directory using PowerShell. This method is efficient, reliable, and perfect for large-scale user management tasks.

## Step 1: Launch PowerShell ISE with Admin Rights

Start by opening the PowerShell Integrated Scripting Environment (ISE) as an administrator. This provides the necessary permissions to run scripts that interact with Active Directory.

![PowerShell ISE Launch](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_01.png?raw=true)

## Step 2: Script Preparation

In the PowerShell ISE, draft or open your pre-written script designed for mass user account creation. Before running, ensure the script logic is correct to prevent any accidental errors.

![Script Preparation](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_02.png?raw=true)

## Step 3: Execute the Creation Script

With your script loaded in the ISE, initiate the execution process. Observe the output carefully for successful execution or to catch and troubleshoot any issues that arise.
(Script below automates 1000 user accounts, simply change the number in the second line of the script to suit your needs.)
```powershell
$PASSWORD_FOR_USERS = "123456789Abc!"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 1000
# ------------------------------------------------------ #

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""
    
    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        } else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }
    
    return $name
}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $firstName = generate-random-name
    $lastName = generate-random-name
    $username = $firstName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]``"").distinguishedName)" `
               -Enabled $true
    
    $count++
}

```
![Script Execution](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_03.png?raw=true)

## Step 4: Confirm User Accounts Creation

Once the script completes, verify in the Active Directory that all user accounts have been created and choose a random account to log into. This step not only confirms the script's effectiveness but also demonstrates the login process.

![User Creation Confirmation](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_04.png?raw=true)

## Step 5: Log into the Newly Created Account

Demonstrate the process of logging into the newly created user account ('fur.wax') to ensure that user can log in successfully.

![Logging into Account](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_05.png?raw=true)

Confirm the active session by logging into the Virtual Machine as the new user and verify that the correct user profile is loaded and operational.

![Active Session Confirmation](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_06.png?raw=true)

## Step 6: Execute Commands as the New User

Once logged in as 'fur.wax', verify you can perform tasks like pinging other systems or running commands that indicate the user environment is correctly set up.

![Command Prompt Verification](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_07.png?raw=true)

## Step 7: Final Verification and Configuration

Perform a final check to confirm that all user settings are correctly configured, which might involve pinging network resources or accessing shared drives to ensure network connectivity and proper permissions are in place.

![Final User Configuration](https://github.com/KLavallais/KLavallais/blob/main/images/AutomatedRandomUsers_08.png?raw=true)


By following this tutorial, you’ve efficiently created multiple user accounts with minimal effort. Automating such processes not only saves time but also introduces a high level of accuracy and consistency in administrative tasks.

