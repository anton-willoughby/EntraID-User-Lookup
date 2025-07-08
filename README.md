# EntraID User Lookup Tool

A simple, self-contained PowerShell GUI application for looking up user information from Microsoft Entra ID (Azure Active Directory).

## Features

- **Simple GUI Interface**: Easy-to-use Windows Forms interface
- **Automatic Module Installation**: Checks for and installs required Microsoft.Graph modules if not present
- **Secure Authentication**: Uses Microsoft Graph authentication with proper scopes
- **Single User Lookup**: Search for users by display name, email, or UPN
- **Detailed User Information**: Displays comprehensive user details including:
  - Display Name
  - User Principal Name
  - Email
  - Department
  - Job Title
  - Manager
  - Account Status (Enabled/Disabled)
  - Creation Date
  - Last Sign-in Date
- **Export Options**: Export user data to CSV format
- **Connection Management**: Easy connect/disconnect with status indicators
- **Error Handling**: Comprehensive error handling and user-friendly messages

## Prerequisites

1. **PowerShell 5.1 or later** (Windows PowerShell or PowerShell Core)
2. **Microsoft Graph PowerShell SDK** (automatically installed by the script)
3. **Appropriate permissions** in your Entra ID tenant:
   - `User.Read.All` - To read user profiles
## Quick Start

### Option 1: Using Batch Files (Recommended)
- **Standard Mode**: Double-click `run-gui.bat`
- **Administrator Mode**: Double-click `run-gui-admin.bat` (if elevated permissions are needed)

### Option 2: PowerShell Direct
```powershell
.\simple-gui-lookup.ps1
```

## File Structure

```
EntraID/
├── simple-gui-lookup.ps1     # Main GUI application (self-contained)
├── run-gui.bat              # Standard launcher
├── run-gui-admin.bat        # Administrator launcher
├── README.md                # This documentation
├── TROUBLESHOOTING.md       # Troubleshooting guide
└── backup-20250708-152900/  # Backup of removed legacy files
```

## Authentication & Permissions

## Example Output

```
=== Microsoft Graph - Entra ID User Retrieval Script ===
Starting script execution...
1. **First run**: The tool will prompt you to sign in to your Microsoft 365/Azure account
2. **Module installation**: If required modules aren't installed, the tool will offer to install them automatically
3. **Search for users**: Enter display name, email, or UPN in the search field
4. **View details**: Select a user from the results to see detailed information
5. **Export data**: Use the export button to save user data to CSV

## Required Permissions

The application requires the following Microsoft Graph permissions:
- `User.Read.All` - To read user profiles
- `Directory.Read.All` - To read directory data

These permissions are requested automatically during authentication.

## Troubleshooting

For detailed troubleshooting information, see `TROUBLESHOOTING.md`.

**Common Issues:**
1. **Module Installation**: If automatic installation fails, run as administrator
2. **Authentication**: Ensure you have appropriate permissions in your Entra ID tenant
3. **Execution Policy**: Use the provided batch files or set execution policy:
   ```powershell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
   ```
3. **Permission Errors**: Contact your tenant administrator to grant the required Graph permissions

## Security Considerations

- The script uses device code authentication flow for secure login
- No credentials are stored in the script
- Connection is properly disconnected after execution
- Consider running in a secure environment for production use

## Customization

You can extend this script to:
- Filter users by specific criteria
- Include additional user properties
## Notes

- This is a self-contained application - no additional configuration files needed
- All removed legacy files are backed up in the `backup-20250708-152900/` directory
- The tool automatically handles Microsoft Graph module installation and updates

## Support

For issues or questions:
1. Check `TROUBLESHOOTING.md` for common solutions
2. Verify your Microsoft Graph permissions
3. Ensure you're running with appropriate PowerShell execution policy
