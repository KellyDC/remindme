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
4. The application will automatically add itself to startup
5. Navigate the program files location `C:\Program Files\RemindMe` and update the security allow users to have Modify access, so you can add/update the settings. 


## System Tray Usage

Once RemindMe is running, you'll see a notification icon in your system tray. Right-click the icon to access:

- **About**: Display About dialog of the application
- **Check Tasks Now**: Manually trigger a task check
- **Edit Settings**: Opens `settings.json` in Notepad for editing
- **Reload Settings**: Reloads the configuration after making changes
- **Quit**: Closes the application

After editing settings, use "Reload Settings" to apply changes without restarting the application.

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
