---
permalink: /issues/nvidia
---

# nvlddmkm cannot be found
Event viewer: 
```
The description for Event ID 153 from source nvlddmkm cannot be found. Either the component that raises this event is not installed on your local computer or the installation is corrupted. You can install or repair the component on the local computer.

If the event originated on another computer, the display information had to be saved with the event.

The following information was included with the event: 

\Device\Video3
Error occurred on GPUID: 100

The message resource is present but the message was not found in the message table
```

## Solutions
### Permission to nvlddmkm
```ps
$files = (Get-Childitem –Path 'C:\' -Include '*nvlddmkm*' -File -Recurse -ErrorAction SilentlyContinue)
$currentUser = '$env:USERNAME'

foreach ($file in $files) {
	takeown /F "$file"
    icacls "$file" /grant Administrators:F /C
	icacls "$file" /grant '$currentUser:F' /T /C
	icacls "$file" /setowner 'NT SERVICE\TrustedInstaller' /C
}
```


### Disable windows auto download
System -> Advanced system settings -> Hardware ->  Device installation settings -> No