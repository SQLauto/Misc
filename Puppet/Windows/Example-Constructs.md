#Example Windows Constructs


## Exec Commands

### Unless Command
This will only run if the path is not mounted. Unless exit 0 means it exists in this case, so command not run
```
exec { 'Mount Network Z Drive if it Does Not Exist':
  provider  => powershell,
  command => '$(C:\Windows\System32\net.exe use Z: "\\san\shared-drive"'),
  unless  => 'if (test-path z:) { exit 0 }',
}
```

COMPETING EXAMPLE!!!! ABOVE SEEMS BROKEN
```
  exec { 'Install ServerSpec RubyGem': 
    command => 'c:\\Ruby24\\bin\\gem.cmd install serverspec', 
    unless => '$result = c:\\Ruby24\\bin\\gem.cmd list | findstr serverspec; 
        if ($result -neq $null) { Exit 1 } else { Exit 0 }',
    provider => powershell, 
  }
```

### Powershell Running a 'cmd' Command
Run a local bat file for example
```
exec { 'Run bat file':
    provider  => powershell,
    cwd       => "C:\\Test",
    command   => ".\\DoSomething.bat BOTH",
    logoutput => true,
}
```

### Is machine in a domain?
```
notify {"DOMAIN IS - ${domain}":}
if $domain != undef {
  notify {"Machine IN domain":}
} else {
  notify {"Machine NOT in domain":}
}

```