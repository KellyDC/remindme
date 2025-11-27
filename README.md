# Remind Me - Windows Task Reminder

**Version 1.2.0**

## What is Remind Me?

A lightweight Windows application that sends toast notifications for scheduled tasks. Features include flexible scheduling (dates, daily, weekdays), smart categories, custom sounds, and a visual task editor.

## Quick Start

1. **Install**: Run `RemindMe-Setup.msi` - the app launches automatically after installation
2. **Configure**: Right-click tray icon → Edit Settings
3. **Add Tasks**: Use GUI editor with calendar/time pickers
4. **Done**: Tasks trigger automatically

## Key Features

- 📅 **Flexible Scheduling**: Specific dates, daily, or weekday patterns
- 🎨 **Category Dropdown**: General, Personal, Work, Special
- 🎂 **Birthday Detection**: Auto birthday icon/sound for tasks with "birthday" in name
- 🔔 **Toast Notifications**: Native Windows 10/11 notifications
- 🖥️ **Visual Editor**: Calendar picker, time dropdown, auto-generated IDs
- 🎵 **Smart Sounds**: Per-task control, automatic birthday.wav
- 🖱️ **System Tray**: Easy access and control
- 🚀 **Auto-Start**: Launches after installation and starts with Windows
- 💾 **Auto-Save**: Changes apply immediately

## Task Examples

**Daily Reminder**: "Drink Water" every hour at :00  
**Weekday Meeting**: "Team Standup" on Monday, Wednesday, Friday at 9:15 AM  
**Birthday**: "Mom's Birthday" on specific date with automatic birthday icon/sound

_Tip: Tasks with "birthday" in the name automatically use birthday icon and sound_

## Task Features

- **Auto-Generated IDs**: System creates unique identifiers automatically
- **Smart Categories**: Work tasks skip weekends, birthday tasks get special icons
- **Flexible Scheduling**: Specific dates, daily reminders, or weekday patterns
- **Custom Sounds**: Enable/disable per task, automatic birthday sound detection
- **Visual Editing**: No JSON editing needed - use the GUI editor

## System Tray Menu

Right-click the tray icon to access:

- **Edit Settings**: Open visual task editor
- **Check Tasks Now**: Manually trigger task check
- **Start with Windows**: Toggle auto-start on login
- **Reload Settings**: Apply any changes
- **About**: View version and app info
- **Quit**: Exit application

## Visual Task Editor

The GUI editor makes task management simple:

1. Right-click tray icon → **Edit Settings**
2. Browse existing tasks or click **New Task**
3. Fill in the form:
   - **Name** and **Description**
   - **Category** dropdown (General, Personal, Work, Special)
   - **Date picker** with full calendar
   - **Time picker** with hour/minute selection
   - **Sound** toggle
4. Click **Save** - changes apply immediately

_Tasks are automatically saved to your settings file_

## Installation Details

- **Installer**: Professional MSI installer with auto-start configuration
- **Launch Options**: Choose to launch immediately after installation
- **Startup Integration**: Automatically adds to Windows startup folder
- **System Requirements**: Windows 10 or Windows 11
- **Icons & Sounds**: All assets included with installation

## Troubleshooting

**Notifications not showing?**

- Check Windows notification settings for "Remind Me"
- Verify Focus Assist is off
- Ensure task is enabled in the editor

**Birthday icon/sound not working?**

- Include "birthday" in task name (case-insensitive)
- Icons and sounds are included with installation

**GUI editor not opening?**

- Check taskbar for the editor window
- Restart the application from system tray

## License

MIT License

## Support

Create issue on GitHub for questions or feature requests.
