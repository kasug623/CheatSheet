# TOC<!-- omit in toc -->
- [Tag Naming Convention Guide](#tag-naming-convention-guide)
  - [🎯 Tag Categories](#-tag-categories)
- [Folder Structure Guide for Security Knowledge Base](#folder-structure-guide-for-security-knowledge-base)
  - [📁 Top-Level Structure (by Purpose)](#-top-level-structure-by-purpose)
- [🔖 Tagging + Folders = Complete System](#-tagging--folders--complete-system)

# Tag Naming Convention Guide
This section defines the tag prefixes and examples used to organize your security knowledge base.  

## 🎯 Tag Categories

| Category             | Prefix      | Example Tags                                            | Notes                              |
|----------------------|-------------|---------------------------------------------------------|-------------------------------------|
| OS                   | `os/`       | `os/windows`, `os/linux`, `os/macos`                   | General OS categories               |
| Distribution         | `distro/`   | `distro/ubuntu`, `distro/centos`, `distro/debian`      | Specific Linux distributions        |
| Programming Language | `lang/`     | `lang/python`, `lang/shell`, `lang/powershell`         | Includes scripting and shell types |
| Team Role            | `team/`     | `team/blue`, `team/red`                                | Role-based categorization           |
| Activity Type        | `type/`     | `type/forensics`, `type/pentest`, `type/malware-analysis` | Field or activity                   |
| Task Type            | `task/`     | `task/scan`, `task/triage`, `task/reverse`             | Specific operations/tasks           |
| Technical Area       | `tech/`     | `tech/memory`, `tech/network`, `tech/log`, `tech/pe`   | Domain-specific technical focus     |
| Tool                 | `tool/`     | `tool/volatility`, `tool/nmap`, `tool/procmon`         | Tool-based classification           |


# Folder Structure Guide for Security Knowledge Base
This section presents an example of folder structure.  

## 📁 Top-Level Structure (by Purpose)
```
CheatSheet/
├── Purpose/
│   ├── Blue/
│   │   ├── LiveResponse/
│   │   ├── EDR/
│   │   │   ├── Falcon/
│   │   │   │   ├── Monitoring.md
│   │   │   │   ├── Incident-Response.md
│   │   │   │   ├── API.md
│   │   │   │   └── Config.md
│   │   │   └── SentinelOne/
│   │   │       ├── Monitoring.md
│   │   │       ├── Incident-Response.md
│   │   │       ├── API.md
│   │   │       └── Config.md
│   │   └── Monitoring/
│   ├── Forensic/
│   │   ├── Memory-Analysis/
│   │   ├── Disk-Analysis/
│   │   └── Timeline-Analysis/
│   ├── Malware-Analysis/
│   │   ├── Static-Analysis/
│   │   ├── Dynamic-Analysis/
│   │   └── Sandboxing/
│   ├── Red/
│   │   ├── PenTest/
│   │   └── C2/
│   └── Common/
├── Tools/
│   ├── PowerShell/
│   ├── Shell/
│   │   ├── Common_Commands.md
│   │   ├── Scripting_Tips.md
│   │   ├── Bash_Specific.md
│   │   └── Zsh_Specific.md
│   ├── Python/
│   ├── CSharp/
│   ├── Wireshark/
│   ├── Nmap/
│   └── Git/
├── OS/
│   ├── Windows/
│   ├── Linux/
│   │   ├── Ubuntu/
│   │   └── CentOS/
│   └── MacOS/
└── Common/
```

# 🔖 Tagging + Folders = Complete System
- **Folders** provide intuitive navigation.
- **Tags** allow cross-sectional filtering, searching, and indexing.
