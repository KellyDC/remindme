# Remind Me - Never Miss a Work or Personal Reminder

Remind Me is a Golang application that reads scheduled notifications from a JSON configuration file and sends Windows toast notifications when tasks are due.
It is designed to help you stay on top of your tasks, whether they are work-related deadlines or personal reminders, with minimal effort.

Sometime we have a personal task that we do not want to show in our calendar, as it is really personal, but we still want to be reminded of it.

![Remind Me](/banner.jpg)

## Features

- 📅 **Schedule-based Notifications**: Automatically checks for due tasks at configurable intervals
- 🔔 **Windows Toast Notifications**: Native Windows 10/11 notifications with sound support
- 🖱️ **System Tray Integration**: Right-click menu in system tray for easy access to settings and controls
- ⚙️ **JSON Configuration**: Easy-to-edit settings file for tasks and application configuration
- 📝 **Comprehensive Logging**: Detailed logging with configurable log levels
- 🚀 **Background Operation**: Runs silently in the system tray
- 🎯 **Manual Check**: Option to check tasks immediately via system tray or command line
- � **Windows Installer**: Professional MSI installer for easy deployment
- �🛡️ **Error Handling**: Graceful handling of file errors, JSON parsing errors, and other exceptions

## Installation

### Prerequisites

- Windows 10 or Windows 11
- For building: Go 1.21 or later and WiX Toolset v3.11+

### Option 1: Windows MSI Installer (Recommended)

1. Download `RemindMe-Setup.msi` from the releases page
2. Double-click the MSI file to start the installation
3. Follow the installation wizard
4. The application will automatically start after installation and add itself to startup

### Option 2: Download Pre-built Executable

1. Download the latest `RemindMe.exe` from the releases page
2. Place it in your desired directory (e.g., `C:\Program Files\RemindMe\`)
3. Create a `settings.json` file in the same directory
4. Run `RemindMe.exe`

### Option 3: Build from Source

1. Clone or download this repository
2. Open PowerShell in the project directory
3. Install dependencies:
   ```powershell
   go mod download
   ```
4. Build the executable:

   ```powershell

   # Build with icon embedded
   windres resource.rc -o resource.syso
   go build -ldflags="-H windowsgui" -o RemindMe.exe
   ```

Cleaning up the build artifacts...

```powershell
# Clean up previous builds
 Remove-Item .\RemindMe.exe -ErrorAction SilentlyContinue; go clean -cache -modcache
```

## System Tray Usage

Once RemindMe is running, you'll see a notification icon in your system tray. Right-click the icon to access:

- **Check Tasks Now**: Manually trigger a task check
- **Edit Settings**: Opens `settings.json` in Notepad for editing
- **Reload Settings**: Reloads the configuration after making changes
- **Quit**: Closes the application

After editing settings, use "Reload Settings" to apply changes without restarting the application.

## Building the Windows Installer

To create your own MSI installer package:

### Prerequisites for Building Installer

1. **WiX Toolset v3.11 or later**: Download from [https://wixtoolset.org/releases/](https://wixtoolset.org/releases/)
2. **Go 1.21 or later**: For building the application
3. **PowerShell or Command Prompt**: For running build scripts

### Build Steps

1. **Using PowerShell (Recommended)**:

   ```powershell
   .\build-installer.ps1
   ```

2. **Using Batch File**:

   ```cmd
   build-installer.bat
   ```

3. **Manual Build**:

   ```powershell
   # Build the Go application
   go build -ldflags "-H windowsgui" -o RemindMe.exe .

   # Compile WiX source (adjust path as needed)
   & "${env:ProgramFiles(x86)}\WiX Toolset v3.11\bin\candle.exe" -out dist\RemindMe.wixobj installer\RemindMe.wxs

   # Create MSI installer
   & "${env:ProgramFiles(x86)}\WiX Toolset v3.11\bin\light.exe" -out dist\RemindMe-Setup.msi dist\RemindMe.wixobj -ext WixUIExtension
   ```

4. **Updating media**:

   If you need to update the media files (icons, banners, etc.) used in the installer, make your changes in the `installer` directory and re-run the build process.

5. **Output**:
   After running the build script, the installer will be created in the `dist` directory.

   The installer will be created as `dist\RemindMe-Setup.msi` and includes:

- Main executable (`RemindMe.exe`)
- Default settings file (`settings.json`)
- Application icons (`icon.ico`, `icon.png`)
- Documentation files (`README.md`, `LICENSE`)
- Start Menu shortcuts
- Automatic startup configuration
- Uninstaller

## Task Types

RemindMe supports two types of tasks:

1. **Specific Date Tasks**: Tasks that trigger only on a specific date

   - Set both `due_date` (YYYY-MM-DD format) and `due_time` (HH:MM format)
   - Example: A project deadline on July 15, 2025 at 2:00 PM

2. **Recurring Daily Tasks**: Tasks that trigger every day at the same time
   - Set `due_date` to an empty string `""` and specify `due_time` (HH:MM format)
   - Example: Daily medication reminders, lunch breaks, or standup meetings

### Example Configuration with Both Task Types

```json
{
  "tasks": [
    {
      "id": "project-deadline",
      "name": "Project Submission Deadline",
      "description": "Submit the final project deliverables",
      "due_date": "2025-07-20",
      "due_time": "17:00",
      "sound": true,
      "enabled": true
    },
    {
      "id": "daily-water-reminder",
      "name": "Drink Water",
      "description": "Stay hydrated! Time for a glass of water",
      "due_date": "",
      "due_time": "10:00",
      "sound": false,
      "enabled": true
    }
  ]
}
```

## Configuration

Create a `settings.json` file with your task configuration:

```json
{
  "tasks": [
    {
      "id": "unique-task-id",
      "name": "Task Name",
      "description": "Detailed task description",
      "due_date": "2025-07-14",
      "due_time": "15:30",
      "sound": true,
      "enabled": true
    }
  ],
  "check_interval": "0 15,16,17,18 * * *",
  "log_level": "info",
  "sound_enabled": true
}
```

### Configuration Options

#### Tasks

- **id**: Unique identifier for the task
- **name**: Display name for the task (shown in notification title)
- **description**: Detailed description (shown in notification body)
- **due_date**: Date in YYYY-MM-DD format (leave empty `""` for recurring daily tasks)
- **due_time**: Time in HH:MM format (24-hour)
- **sound**: Enable/disable sound for this specific task
- **enabled**: Enable/disable the task

**Note**: Tasks with an empty `due_date` field are treated as recurring daily tasks that trigger every day at the specified `due_time`.

#### Global Settings

- **check_interval**: Cron expression for when to check tasks
  - Default: `"0 15,16,17,18 * * *"` (3PM, 4PM, 5PM, 6PM daily)
  - Examples:
    - `"0 * * * *"` - Every hour
    - `"*/5 * * * *"` - Every 5 minutes
    - `"0 9,12,15,18 * * *"` - 9AM, 12PM, 3PM, 6PM daily
- **log_level**: Logging verbosity (`debug`, `info`, `warn`, `error`)
- **sound_enabled**: Global sound toggle

### Cron Expression Format

The check_interval uses standard cron format:

```
* * * * *
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sunday = 0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

## Usage

### Running the Application

#### Background Mode (Recommended)

```powershell
# Run with default settings.json in current directory
.\RemindMe.exe

# Run with custom settings file
.\RemindMe.exe "C:\path\to\custom-settings.json"
```

#### Manual Check Mode

```powershell
# Check tasks once and exit
.\RemindMe.exe settings.json --check-now
```

### Running as a Windows Service

To run RemindMe as a Windows service, you can use tools like NSSM (Non-Sucking Service Manager):

1. Download NSSM from https://nssm.cc/
2. Install the service:
   ```powershell
   nssm install RemindMe "C:\path\to\RemindMe.exe" "C:\path\to\settings.json"
   ```
3. Start the service:
   ```powershell
   nssm start RemindMe
   ```

### Task Scheduler Integration

Alternatively, use Windows Task Scheduler:

1. Open Task Scheduler
2. Create Basic Task
3. Set trigger (e.g., "At startup")
4. Set action to start `RemindMe.exe` with your settings file path
5. Configure to run whether user is logged on or not

## Logging

The application creates detailed logs with the following information:

- Task check events
- Notification sending status
- Error messages and debugging information
- Application lifecycle events

Logs are output to stdout in JSON format. You can redirect to a file:

```powershell
.\RemindMe.exe settings.json > RemindMe.log 2>&1
```

## Example Scenarios

### Daily Work Reminders

```json
{
  "tasks": [
    {
      "id": "standup",
      "name": "Daily Standup",
      "description": "Join the team standup meeting",
      "due_date": "2025-07-14",
      "due_time": "09:00",
      "sound": true,
      "enabled": true
    },
    {
      "id": "lunch-break",
      "name": "Lunch Break",
      "description": "Time for a healthy lunch break!",
      "due_date": "",
      "due_time": "12:00",
      "sound": false,
      "enabled": true
    }
  ],
  "check_interval": "0 * * * *",
  "log_level": "info",
  "sound_enabled": true
}
```

### Medication Reminders (Recurring Daily Tasks)

```json
{
  "tasks": [
    {
      "id": "morning-meds",
      "name": "Morning Medication",
      "description": "Take your morning medications with breakfast",
      "due_date": "",
      "due_time": "08:00",
      "sound": true,
      "enabled": true
    },
    {
      "id": "evening-meds",
      "name": "Evening Medication",
      "description": "Take your evening medications with dinner",
      "due_date": "",
      "due_time": "19:00",
      "sound": true,
      "enabled": true
    }
  ],
  "check_interval": "*/15 * * * *",
  "log_level": "info",
  "sound_enabled": true
}
```

## Troubleshooting

### Common Issues

1. **Notifications not appearing**

   - Ensure Windows notifications are enabled for the application
   - Check Windows Focus Assist settings
   - Verify the application is running with proper permissions

2. **Settings file not found**

   - Verify the path to settings.json is correct
   - Use absolute paths when in doubt
   - Check file permissions

3. **JSON parsing errors**

   - Validate your JSON syntax using an online JSON validator
   - Ensure date format is YYYY-MM-DD
   - Ensure time format is HH:MM (24-hour)

4. **Tasks not triggering**
   - Check that the current date matches the due_date
   - Verify the check_interval includes the task's due_time
   - Ensure the task is enabled

### Enable Debug Logging

Set `log_level` to `"debug"` in your settings.json for more detailed output.

### Testing Notifications

Use the `--check-now` flag to test notifications immediately:

```powershell
.\RemindMe.exe settings.json --check-now
```

## Building from Source

### Dependencies

- [go-toast](https://github.com/go-toast/toast) - Windows toast notifications
- [cron](https://github.com/robfig/cron/v3) - Cron job scheduling
- [logrus](https://github.com/sirupsen/logrus) - Structured logging

### Build Commands

```powershell
# Download dependencies
go mod download

# Build executable
go build -o RemindMe.exe .

# Build with optimizations (smaller file size)
go build -ldflags="-s -w" -o RemindMe.exe .

# Cross-compile for different architectures
$env:GOOS="windows"; $env:GOARCH="amd64"; go build -o RemindMe-amd64.exe .
$env:GOOS="windows"; $env:GOARCH="386"; go build -o RemindMe-386.exe .
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Support

For issues, questions, or feature requests, please create an issue on the GitHub repository.
