# Lab Setup

## Environment

- Host system: Windows 10
- Victim system: Windows 10 Pro VM
- Attack type: RDP brute force simulation
- Log source: Windows Security Log

## Preparation Steps

1. Confirm the Windows 10-Test VM is running
2. Create a local test account:
   ```cmd
   net user testuser Password123! /add
   ```
3. Add the account to Remote Desktop Users:
   ```cmd
   net localgroup "Remote Desktop Users" testuser /add
   ```
4. Enable Remote Desktop on the victim VM
5. Enable logon auditing
6. Clear the Security log before testing
7. Start RDP from the host using `mstsc`
8. Enter incorrect passwords multiple times
9. Review Event ID 4625 in Event Viewer

## Security Log Path

```text
Event Viewer → Windows Logs → Security
```
