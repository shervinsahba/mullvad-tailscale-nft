These are nft rules to make tailscale and mullvad run simultaneously on Linux. See https://github.com/tailscale/tailscale/issues/925#issuecomment-1616354736

Move these files to your directory of choice (/opt was chosen as seen in the files). Then load into nftables.
```
mv mullvad-tailscale*.nft /opt
nft -f /opt/mullvad-tailscale.nft
```

The author, sdht0, comments the following:

The above rules apply independent of the mullvad rules and make almost everything work. However, to be able to connect to the exposed ports from docker, the mullvad rules need to be modified directly: `nft insert rule inet mullvad forward oifname "tailscale0" accept`

Consolidating into the systemd service:

```
systemctl edit tailscaled
```
```
[Service]
ExecStartPre=nft -f '/opt/mullvad-tailscale.nft'
ExecStartPre=nft insert rule inet mullvad forward ip daddr 100.64.0.0/10 accept
ExecStopPost=nft -f '/opt/mullvad-tailscale-cleanup.nft'
```
