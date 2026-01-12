1. Make sure that the virtio media iso is mounted to a drive 
2. Install with pnputil. 
```
pnputil /add-driver <DriveLetter>:\NetKVM\<OS-version>\amd64\*.inf /install
```

```ad-tip
**Use 'Get-Location' 'Path' property to get absolute path of current directory**
```

```ad-pscode
	$absolutePath = (Get-Location).Path
```

