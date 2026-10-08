# PowerShell Scripts

Windows administration tooling written to take the repetition out of Active Directory work on a service desk. Everything here is GUI-driven Windows Forms on top of the `ActiveDirectory` module, so first-line staff can use it without learning the cmdlets.

## Scripts

| Script | What it does | Requires |
|---|---|---|
| [`AD Tool`](./AD%20Tool) | **"Quick AD Tools"**: a tabbed GUI for the tasks that generated the most tickets. Mirror one account's group memberships onto another; add many groups to many users at once; update user attributes such as surname; un-archive accounts; assign Microsoft Teams phone numbers; strip a user from every group at offboarding; delete stale computer objects. Every action is logged to a file for troubleshooting. | `ActiveDirectory` and `MicrosoftTeams` modules, AD/Teams admin rights, .NET Framework |
| [`Pull User Details in Active Directory`](./Pull%20User%20Details%20in%20Active%20Directory) | Small lookup GUI: enter a `firstname.lastname` SAM account name and get display name, email, enabled state, last logon and full group membership in one window. Built for "why can't this user get into X" calls. | `ActiveDirectory` module |

## Running

```powershell
# RSAT / AD module on a workstation
Add-WindowsCapability -Online -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0
Install-Module MicrosoftTeams -Scope CurrentUser     # AD Tool only

Set-ExecutionPolicy -Scope Process Bypass
.\'Pull User Details in Active Directory'
.\'AD Tool'
```

Run with an account that has the rights for the operation you are performing. Test in a non-production domain first; several AD Tool actions (remove from all groups, delete computer object) are not reversible.

## Design notes

- **GUI over cmdlets on purpose.** The audience was service-desk staff, not administrators. A form with a text box and a button is faster to hand over than a cmdlet with six parameters.
- **Log everything.** The AD Tool writes each operation and its result to a log file so a bad bulk change can be traced and reversed.
- **Confirm destructive actions.** Group-stripping and object deletion prompt before running.

## Related

- [Network Optimisation & Security Enhancement](https://github.com/Dstanfield-Creator/projects/tree/master/infrastructure/network-security-enhancement) and [Cloud Services & VM Management](https://github.com/Dstanfield-Creator/projects/tree/master/infrastructure/cloud-vm-management), the roles this tooling came out of.
- [general-it](https://github.com/Dstanfield-Creator/general-it) for the onboarding/offboarding checklists these scripts support.

## Disclaimer

These scripts are provided for educational and informational purposes. Use them at your own risk; the author accepts no liability for loss or damage resulting from their use. Review and test in a safe environment before running against production systems.

---

**Author:** Danny Stanfield · Perth, WA  
**License:** MIT
