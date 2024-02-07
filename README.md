These are nft rules to make tailscale and mullvad run simultaneously on Linux. See https://github.com/tailscale/tailscale/issues/925#issuecomment-1616354736

Move these files to your directory of choice (/opt was chosen as seen in the files). Then load into nftables.
```
mv mullvad-tailscale*.nft /opt
nft -f /opt/mullvad-tailscale.nft
```

Consolidating into the systemd service:

```
systemctl edit tailscaled
```
```
[Service]
ExecStartPre=nft -f '/opt/mullvad-tailscale.nft'
ExecStopPost=nft -f '/opt/mullvad-tailscale-cleanup.nft'
```
