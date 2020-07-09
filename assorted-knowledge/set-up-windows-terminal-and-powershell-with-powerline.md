---
description: This applies to Powershell and not Windows Powershel
---

# Set up windows terminal & Powershell with Powerline

`Install-Module posh-git -Scope CurrentUser` 

`Install-Module oh-my-posh -Scope CurrentUser`  


`notepad $PROFILE`  
add:

```text
Import-Module posh-git
Import-Module oh-my-posh
Set-Theme Paradox
```

In settings.json add this to the right profile  
`"fontFace": "Cascadia Code PL",`

`choco install cascadiafonts` with windows powershell administrator

