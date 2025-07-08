# Troubleshooting Guide for Microsoft Graph Authentication Issues

## Common Authentication Problems and Solutions

### 1. GUI Hangs During Authentication
**Problem**: When clicking Connect, the GUI becomes unresponsive and no browser window appears.

**Solutions**:
- **Run as Administrator**: Right-click on PowerShell and select "Run as Administrator"
- **Clear Credentials**: Click the "Clear Creds" button in the GUI, then try connecting again
- **Check PowerShell Execution Policy**: Run `Get-ExecutionPolicy` and if it's `Restricted`, run `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`
- **Try Device Code Flow**: The script now automatically falls back to device code authentication if interactive fails

### 2. Browser Authentication Window Doesn't Appear
**Problem**: The authentication process starts but no browser window opens.

**Solutions**:
- **Check Default Browser**: Ensure you have a default browser set in Windows
- **Firewall/Antivirus**: Temporarily disable firewall or antivirus software
- **Network Proxy**: If behind a corporate proxy, configure PowerShell to use it:
  ```powershell
  $proxy = New-Object System.Net.WebProxy("http://proxyserver:port")
  [System.Net.WebRequest]::DefaultWebProxy = $proxy
  ```
- **Use Device Code**: The script will show a device code that you can enter at https://microsoft.com/devicelogin

### 3. "Access Denied" or "Insufficient Privileges" Error
**Problem**: You get permission-related errors during authentication.

**Solutions**:
- **Contact Admin**: Your tenant administrator needs to grant you the following permissions:
  - `User.Read.All`
  - `Directory.Read.All`
- **Admin Consent**: Admin may need to provide admin consent for the application
- **Check User Role**: Ensure your account has at least "Directory Reader" role in Azure AD

### 4. Module Installation Issues
**Problem**: Microsoft Graph PowerShell module fails to install.

**Solutions**:
- **Run as Administrator**: PowerShell may need elevated privileges to install modules
- **Check PowerShell Gallery**: Verify you can access PowerShell Gallery:
  ```powershell
  Get-PSRepository
  ```
- **Manual Installation**: Download and install the module manually from PowerShell Gallery
- **Corporate Environment**: Your organization may have restrictions on module installation

### 5. Connection Timeouts
**Problem**: Authentication process times out before completion.

**Solutions**:
- **Network Issues**: Check internet connectivity and DNS resolution
- **Increase Timeout**: Add timeout parameter to connection command
- **VPN Issues**: If using VPN, try disconnecting and reconnecting
- **Regional Issues**: Some regions may have connectivity issues with Microsoft Graph

### 6. Cached Credential Issues
**Problem**: Authentication fails due to cached/expired credentials.

**Solutions**:
- **Clear Credentials**: Use the "Clear Creds" button in the GUI
- **Manual Clear**: Run these commands:
  ```powershell
  Disconnect-MgGraph
  Clear-MgContext
  ```
- **Windows Credential Manager**: Clear stored credentials in Windows Credential Manager
- **Azure CLI**: If you have Azure CLI installed, run `az logout`

### 7. Multi-Factor Authentication (MFA) Issues
**Problem**: MFA prompts don't appear or fail during authentication.

**Solutions**:
- **Browser MFA**: Ensure your browser supports MFA (modern browsers recommended)
- **App Passwords**: May need to generate app-specific passwords
- **Conditional Access**: Check if conditional access policies are blocking the connection
- **Device Registration**: Ensure your device is registered in Azure AD if required

### 8. PowerShell Version Compatibility
**Problem**: Authentication fails due to PowerShell version issues.

**Solutions**:
- **Update PowerShell**: Ensure you're using PowerShell 5.1 or later
- **PowerShell Core**: Consider using PowerShell Core (pwsh.exe) instead of Windows PowerShell
- **Module Compatibility**: Ensure Microsoft Graph module is compatible with your PowerShell version

## Step-by-Step Troubleshooting Process

1. **Check Prerequisites**:
   - PowerShell 5.1 or later
   - Internet connectivity
   - Proper permissions in Azure AD

2. **Clear Everything**:
   - Click "Clear Creds" button
   - Restart PowerShell
   - Run as Administrator

3. **Test Connection**:
   - Try connecting with minimal scopes first
   - Check if you can access other Microsoft services

4. **Check Logs**:
   - Look for error messages in PowerShell
   - Check Windows Event Viewer
   - Review Azure AD sign-in logs

5. **Alternative Methods**:
   - Try using Azure PowerShell module
   - Use Microsoft Graph Explorer (https://developer.microsoft.com/graph/graph-explorer)
   - Test with Azure CLI

## Getting Help

If you continue to experience issues:

1. **Check Microsoft Documentation**: Visit Microsoft Graph PowerShell documentation
2. **Azure AD Admin**: Contact your Azure AD administrator
3. **Microsoft Support**: Create a support ticket with Microsoft
4. **Community Forums**: Check PowerShell and Microsoft Graph communities

## Useful Commands for Diagnostics

```powershell
# Check PowerShell version
$PSVersionTable

# Check execution policy
Get-ExecutionPolicy

# Check installed modules
Get-Module -ListAvailable Microsoft.Graph*

# Check current context
Get-MgContext

# Test network connectivity
Test-NetConnection graph.microsoft.com -Port 443

# Check proxy settings
netsh winhttp show proxy
```

## Common Error Messages and Solutions

- **"Connect-MgGraph: The term 'Connect-MgGraph' is not recognized"**
  - Solution: Install Microsoft Graph PowerShell module

- **"Authentication needed. Please call Connect-MgGraph"**
  - Solution: Connection expired, reconnect to Microsoft Graph

- **"Insufficient privileges to complete the operation"**
  - Solution: Contact administrator for proper permissions

- **"The user or administrator has not consented to use the application"**
  - Solution: Admin consent required for the application

- **"AADSTS50020: User account is not supported"**
  - Solution: Account type not supported (e.g., personal Microsoft account in work context)

Remember: Most authentication issues can be resolved by running PowerShell as Administrator and clearing cached credentials.
