Altrantives: https://github.com/RedTeamPentesting/pretender pretender
- There are other options in the Responder configuration file, like `Specific IP Addresses to respond to (default = All)`, which can be helpful to configure in some cases where we only want to relay connections from a specific IP or group of IPs.

| flag | Usage                                                             |
| ---- | ----------------------------------------------------------------- |
| -A   | Analysis mode<br>Recon mode, where we only listen to the traffic. |

```
sudo python3 Responder.py -I ens192
```

