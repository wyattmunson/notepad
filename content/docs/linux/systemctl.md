---
title: "Systemd"
description: ""
summary: "Use systemctl control the systemd system and service manager via the command line."
date: 2024-07-02T02:52:22-07:00
lastmod: 2024-07-02T02:52:22-07:00
draft: false
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

`systemd` is a process and service manager.

It can be used to start a webserver on boot, run a bash script, mount a drive automatically, and many more things.

`systemctl` is the CLI that fronts `systemd`. Unit files are used to find systemd processes.

## Systemctl

`systemctl` is the CLI to manage `systemd`.

```bash
sudo systemctl status docker
sudo systemctl status docker.service

# start a service
sudo systemctl start docker

# stop a service
sudo systemctl stop docker

# restart a service
sudo systemctl restart docker

# reload config without restarting
sudo systemctl reload docker

# start service on boot
sudo systemctl enable docker

# disable start on boot
sudo systemctl disabled docker

# reload daemon
sudo systemctl daemon-reload
```

## Unit Files

A `unit` is some resource that systemd knows how to operate and manage.

{{< callout note>}}
Typical location: `/etc/systemd/system/SERVICE_NAME.service`
{{< /callout >}}

### Types of Unit Files

The `.service` unit file is the most common, but there are a variety of types:

- `.service` - how to manage a service or application on the box. Includes how to start/stop service, when to start it, and dependency order
- `.socket` - network or IPC socket, or FIFO buffer that `systemd` uses for socket-based activation.
- `.device` - manages devces with `udev` or `sysfs` filesystem
- `.mount` - manages a mountpoint. Named after the mount path
- `.automount` - manages a mountpoint to be automatically mounted
- `.swap` - manages swap space on system
- `.path` - defines a path that can be used for path-based activation
- `.snapshot` - created by `systemctl snapshot` command. Allows for snapshot of system state when making changes. Snapshots do not persist between sessions and are used to rollback temporary states.
- `.slice` - related to Linux Control Group nodes
- `.scope` - created automatically from info recieved from bus interfaces

### Example Unit File

```txt { title="/etc/systemd/system/SERVICE_NAME.service" }
[Unit]
Description=Docker compose service
After=docker.service
Requires=docker.service

[Service]
User=USER_NAME
Group=USER_NAME
TimeoutStartSec=infinity
TimeoutStopSec=5min
Restart=always
RestartSec=3
WorkingDirectory=/path/to/Dockerfile

ExecStart=/usr/bin/docker compose -p STACK_NAME up
ExecStop=/usr/bin/docker compose -p STACK_NAME down

[Install]
WantedBy=multi-user.target
```

### Overriding a unit file

Override specific aspects of a unit file with a snippets in a subdirectory. For a unit named `SERVICE_NAME.service`, create a subdirectory `SERVICE_NAME.service.d`, and create a file within it ending in `.conf`.

### Unit File Syntax

```systemd { title="Unit file syntax definitions" }
[Section]
Directive1=value
Directive2=value2
```

- Section names are case sensitive

#### [Unit] Section

{{< callout note >}}
The `[Unit]` section is the beginning of the unit file and defines the metadata and relationship to other units.
{{< /callout >}}

- `Description=`: This directive can be used to describe the name and basic functionality of the unit. It is returned by various systemd tools, so it is good to set this to something short, specific, and informative.
- `Documentation=`: This directive provides a location for a list of URIs for documentation. These can be either internally available man pages or web accessible URLs. The systemctl status command will expose this information, allowing for easy discoverability.
- `Requires=`: This directive lists any units upon which this unit essentially depends. If the current unit is activated, the units listed here must successfully activate as well, else this unit will fail. These units are started in parallel with the current unit by default.
- `Wants=`: This directive is similar to Requires=, but less strict. Systemd will attempt to start any units listed here when this unit is activated. If these units are not found or fail to start, the current unit will continue to function. This is the recommended way to configure most dependency relationships. Again, this implies a parallel activation unless modified by other directives.
- `BindsTo=`: This directive is similar to Requires=, but also causes the current unit to stop when the associated unit terminates.
- `Before=`: The units listed in this directive will not be started until the current unit is marked as started if they are activated at the same time. This does not imply a dependency relationship and must be used in conjunction with one of the above directives if this is desired.
- `After=`: The units listed in this directive will be started before starting the current unit. This does not imply a dependency relationship and one must be established through the above directives if this is required.
- `Conflicts=`: This can be used to list units that cannot be run at the same time as the current unit. Starting a unit with this relationship will cause the other units to be stopped.
- `Condition...=`: There are a number of directives that start with Condition which allow the administrator to test certain conditions prior to starting the unit. This can be used to provide a generic unit file that will only be run when on appropriate systems. If the condition is not met, the unit is gracefully skipped.
- `Assert...=`: Similar to the directives that start with Condition, these directives check for different aspects of the running environment to decide whether the unit should activate. However, unlike the Condition directives, a negative result causes a failure with this directive.

#### [Install] Section

- `WantedBy=`: The WantedBy= directive is the most common way to specify how a unit should be enabled. This directive allows you to specify a dependency relationship in a similar way to the Wants= directive does in the [Unit] section. The difference is that this directive is included in the ancillary unit allowing the primary unit listed to remain relatively clean. When a unit with this directive is enabled, a directory will be created within /etc/systemd/system named after the specified unit with .wants appended to the end. Within this, a symbolic link to the current unit will be created, creating the dependency. For instance, if the current unit has WantedBy=multi-user.target, a directory called multi-user.target.wants will be created within /etc/systemd/system (if not already available) and a symbolic link to the current unit will be placed within. Disabling this unit removes the link and removes the dependency relationship.
- `RequiredBy=`: This directive is very similar to the WantedBy= directive, but instead specifies a required dependency that will cause the activation to fail if not met. When enabled, a unit with this directive will create a directory ending with .requires.
- `Alias=`: This directive allows the unit to be enabled under another name as well. Among other uses, this allows multiple providers of a function to be available, so that related units can look for any provider of the common aliased name.
- `Also=`: This directive allows units to be enabled or disabled as a set. Supporting units that should always be available when this unit is active can be listed here. They will be managed as a group for installation tasks.
- `DefaultInstance=`: For template units (covered later) which can produce unit instances with unpredictable names, this can be used as a fallback value for the name if an appropriate name is not provided.

#### Unit-specific Sections

**Service**

`Type=` Can be one of:

- **simple**: The main process of the service is specified in the start line. This is the default if the Type= and Busname= directives are not set, but the ExecStart= is set. Any communication should be handled outside of the unit through a second unit of the appropriate type (like through a .socket unit if this unit must communicate using sockets).
- **forking**: This service type is used when the service forks a child process, exiting the parent process almost immediately. This tells systemd that the process is still running even though the parent exited.
- **oneshot**: This type indicates that the process will be short-lived and that systemd should wait for the process to exit before continuing on with other units. This is the default Type= and ExecStart= are not set. It is used for one-off tasks.
- **dbus**: This indicates that unit will take a name on the D-Bus bus. When this happens, systemd will continue to process the next unit.
- **notify**: This indicates that the service will issue a notification when it has finished starting up. The systemd process will wait for this to happen before proceeding to other units.
- **idle**: This indicates that the service will not be run until all jobs are dispatched.

Additional Settings:

- `RemainAfterExit=`: This directive is commonly used with the oneshot type. It indicates that the service should be considered active even after the process exits.
- `PIDFile=`: If the service type is marked as “forking”, this directive is used to set the path of the file that should contain the process ID number of the main child that should be monitored.
- `BusName=`: This directive should be set to the D-Bus bus name that the service will attempt to acquire when using the “dbus” service type.
- `NotifyAccess=`: This specifies access to the socket that should be used to listen for notifications when the “notify” service type is selected This can be “none”, “main”, or "all. The default, “none”, ignores all status messages. The “main” option will listen to messages from the main process and the “all” option will cause all members of the service’s control group to be processed.

Manage Services:

- `ExecStart=`: This specifies the full path and the arguments of the command to be executed to start the process. This may only be specified once (except for “oneshot” services). If the path to the command is preceded by a dash “-” character, non-zero exit statuses will be accepted without marking the unit activation as failed.
- `ExecStartPre=`: This can be used to provide additional commands that should be executed before the main process is started. This can be used multiple times. Again, commands must specify a full path and they can be preceded by “-” to indicate that the failure of the command will be tolerated.
- `ExecStartPost=`: This has the same exact qualities as ExecStartPre= except that it specifies commands that will be run after the main process is started.
- `ExecReload=`: This optional directive indicates the command necessary to reload the configuration of the service if available.
- `ExecStop=`: This indicates the command needed to stop the service. If this is not given, the process will be killed immediately when the service is stopped.
- `ExecStopPost=`: This can be used to specify commands to execute following the stop command.
- `RestartSec=`: If automatically restarting the service is enabled, this specifies the amount of time to wait before attempting to restart the service.
- `Restart=`: This indicates the circumstances under which systemd will attempt to automatically restart the service. This can be set to values like “always”, “on-success”, “on-failure”, “on-abnormal”, “on-abort”, or “on-watchdog”. These will trigger a restart according to the way that the service was stopped.
- `TimeoutSec=`: This configures the amount of time that systemd will wait when stopping or stopping the service before marking it as failed or forcefully killing it. You can set separate timeouts with TimeoutStartSec= and TimeoutStopSec= as well.
