# Active Directory Home Lab

A fully functional Active Directory domain built using Samba AD DC on Ubuntu 24.04,
running on UTM 4.7.5 on Apple Silicon (macOS Tahoe). Windows 11 ARM64 is joined
to the domain as a workstation client.

## Lab Environment

| Component | Details |
|---|---|
| Hypervisor | UTM 4.7.5 on Apple Silicon |
| Domain Controller | Ubuntu 24.04 LTS ARM64 + Samba |
| Workstation | Windows 11 ARM64 |
| Domain name | lab.local |
| DC hostname | dc01.lab.local |
| DC static IP | 192.168.64.10 |

## Technologies Demonstrated

- Samba Active Directory Domain Controller
- DNS (Samba internal DNS backend)
- Kerberos authentication
- Organizational Units (OUs)
- Domain users and security groups
- Group Policy Objects (GPOs)
- Windows 11 domain join
- PowerShell AD automation scripts

## Repository Structure
active-directory-lab/
├── README.md
├── docs/
│   ├── setup-notes.md       # Build notes and troubleshooting
│   ├── ou-structure.md      # OU design and user layout
│   └── gpo-table.md         # GPO inventory
├── screenshots/             # Evidence of working configurations
└── scripts/                 # PowerShell and Bash automation scripts

## Lab Phases

| Phase | Topic | Status |
|---|---|---|
| 1 | UTM networking & Ubuntu Server install | ✅ Complete |
| 2 | Samba AD DC provisioning | ✅ Complete |
| 3 | OUs, users & groups | ✅ Complete |
| 4 | Windows 11 domain join | 🔄 In Progress |
| 5 | Group Policy | ⏳ Upcoming |
| 6 | PowerShell automation | ⏳ Upcoming |

## How to Use This Repo

Each phase is documented in `docs/setup-notes.md`. Screenshots showing
verified configurations are in `screenshots/`. All automation scripts
are in `scripts/` with inline comments explaining each step.
