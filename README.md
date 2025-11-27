# Remind Me - Windows Task Reminder

**Version 1.2.0**

## What is Remind Me?

A lightweight Windows application that sends toast notifications for scheduled tasks. Features include flexible scheduling (dates, daily, weekdays), smart categories, custom sounds, and a visual task editor.

## Quick Start

1. **Install**: Run `RemindMe-Setup.msi`
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

## Example Tasks

### Daily Reminder

```json
{
  "category": "Personal",
  "name": "Drink Water",
  "due_date": "",
  "due_time": "*:00",
  "sound": true
}
```

### Weekday Meeting

```json
{
  "category": "Work",
  "name": "Team Standup",
  "due_date": "Monday,Wednesday,Friday",
  "due_time": "09:15",
  "sound": true
}
```

### Birthday Reminder

```json
{
  "category": "Personal",
  "name": "Mom's Birthday",
  "due_date": "2025-08-15",
  "due_time": "08:00",
  "sound": true
}
```

_Note: "Birthday" in name triggers birthday.png icon and birthday.wav sound_

## Configuration

Tasks stored in `settings.json`:

### Task Fields

- **id**: Auto-generated (YYYYMMDD-HHMMSS-random)
- **category**: General, Personal, Work, Special
- **name**: Title (birthday detection)
- **description**: Message
- **due_date**: `"2025-12-25"`, `""` (daily), or `"Monday,Friday"`
- **due_time**: `"HH:MM"`, `"*:00"` (hourly), or `"09:*"` (every minute)
- **sound**: Enable/disable per task
- **enabled**: Active/inactive

### Special Features

- **Birthday**: Tasks with "birthday" use `birthday.png` and `birthday.wav`
- **Work Category**: Skips weekends automatically
- **Icons**: Place `remindme.png` and `birthday.png` in app folder

## System Tray

Right-click icon:

- **Edit Settings**: Visual task editor
- **Check Tasks Now**: Manual trigger
- **Start with Windows**: Auto-start toggle
- **Reload Settings**: Apply changes
- **About**: Version info
- **Quit**: Exit

## GUI Editor

### Features

- Calendar picker for dates
- Time dropdown (hour/minute)
- Category dropdown
- Auto-generated IDs (hidden)
- Birthday icon/sound detection
- Auto-save to settings.json

### Usage

1. Right-click tray → Edit Settings
2. Select task or click New Task
3. Use pickers/dropdowns
4. Save - applies immediately

## Troubleshooting

**Notifications not showing?**

- Check Windows notification settings
- Verify Focus Assist is off
- Ensure task is enabled

**Wrong time?**

- Use 24-hour format: `"14:30"`
- Check weekday spelling

**Birthday icon/sound not working?**

- Include "birthday" in task name (case-insensitive)
- Place `birthday.png` and `birthday.wav` in app folder

**GUI editor not opening?**

- Restart application
- Check taskbar for window

## Requirements

- Windows 10 or 11
- `remindme.png` (default icon)
- `birthday.png` (birthday tasks)
- `sound.wav` (default sound)
- `birthday.wav` (birthday tasks)

## License

MIT License

## Support

Create issue on GitHub for questions or feature requests.
