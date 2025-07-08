# Summary of Changes Made to Fix "Last Sign In" Field

## Problem
The "Last Sign In" text box was not being populated because the script was using the deprecated `LastSignInDateTime` property.

## Root Cause
Microsoft Graph API has deprecated the `LastSignInDateTime` property directly on the user object. The sign-in activity data is now available through the `SignInActivity` property.

## Changes Made

### 1. Updated Property Request (Line ~353)
**Before:**
```powershell
"JobTitle", "AccountEnabled", "CreatedDateTime", "LastSignInDateTime", 
```

**After:**
```powershell
"JobTitle", "AccountEnabled", "CreatedDateTime", "SignInActivity", 
```

### 2. Updated Display Logic (Line ~397-407)
**Before:**
```powershell
$lastSignInTextBox.Text = $User.LastSignInDateTime
```

**After:**
```powershell
# Handle LastSignInDateTime from SignInActivity property
if ($User.SignInActivity -and $User.SignInActivity.LastSignInDateTime) {
    try {
        $lastSignInDate = [DateTime]::Parse($User.SignInActivity.LastSignInDateTime)
        $lastSignInTextBox.Text = $lastSignInDate.ToString("yyyy-MM-dd HH:mm:ss")
    }
    catch {
        $lastSignInTextBox.Text = $User.SignInActivity.LastSignInDateTime
    }
} else {
    $lastSignInTextBox.Text = "Never signed in"
}
```

### 3. Updated Authentication Scopes (Lines ~77 and ~92)
**Before:**
```powershell
Connect-MgGraph -Scopes @("User.Read.All", "Directory.Read.All") -NoWelcome -ErrorAction Stop
```

**After:**
```powershell
Connect-MgGraph -Scopes @("User.Read.All", "Directory.Read.All", "AuditLog.Read.All") -NoWelcome -ErrorAction Stop
```

### 4. Improved Date Formatting
Added proper date parsing and formatting for both CreatedDateTime and LastSignInDateTime for better readability.

## Additional Permission Required
The script now requests `AuditLog.Read.All` permission which is required to access sign-in activity data. This may require admin consent in some organizations.

## Result
- The "Last Sign In" field will now properly display the user's last sign-in date and time
- If a user has never signed in, it will display "Never signed in"
- Dates are formatted consistently as "yyyy-MM-dd HH:mm:ss"
- Better error handling for date parsing

## Testing
To test the fix:
1. Run the script
2. Connect to Microsoft Graph (may need admin consent for new permission)
3. Search for a user who has signed in recently
4. Verify the "Last Sign In" field shows the correct date and time
