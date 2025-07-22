# Installing the Agent
| Package Manager | Install Command | Uninstall Command |
| ----- | ---- | ----- | 
| MSI | `msiexec /i velociraptor.msi` |	`msiexec /x velociraptor.msi` |
| DEB | `sudo dpkg -i velociraptor.deb` | `sudo dpkg -r velociraptor.deb` |
| RPM | `sudo rpm -i velociraptor.rpm` | `sudo rpm -e velociraptor.rpm` |
| RPM-MUSL	| ` sudo rpm -i velociraptor-musl.rpm` | `sudo rpm -e velociraptor-musl.rpm` |


# Checking the Service
## Systemd
* Check the Service

      systemctl status velociraptor_client

* Enable the Service (to autostart on boot)

      systemctl enable velociraptor_client

* Manually start Service (to autostart on boot)

      systemctl start velociraptor_client

  
# Troubleshooting
## Running the Agent in Verbose
  **Linux**
  
    /usr/local/bin/velociraptor_client --config /etc/velociraptor/client.config.yaml client -v 2>&1 | tee /tmp/$HOSTNAME-$(date '+%Y-%m-%d_T_%H-%M-%S')-$(whoami)-troubleshooting.log
  *This will save the output to a file in `/tmp` that can be sent to you for analysis if needed and triggers the agent in verbose mode*

### Reviewing the logs
1. Search for evidence the agent timed out and failed to connect by: `grep -i 'timed out' <<log_file>>`

## Manually Modifying the client configuration file post installation
  **Linux**
  * The file is located at `/etc/velociraptor/client.config.yaml` and the binary is located at `/usr/local/bin/velociraptor_client` by default
