To display a list of available roles features that can be installed on Windows Server 2016. We can use ```Get-WindowsFeature``` cmdlet.

```
Get-WindowsFeature
```

If we want to display features specific only to Active Directory or Hyper-V we can filter the results
as demonstrated below.

```
Get-WindowsFeature -Name AD*
```

```
Get-WindowsFeature -Name Hyper*
```
