# Systemd Timer Cookie Cutter Template

This Cookie Cutter template creates systemd timer and service files for scheduled task execution.

## What is a Systemd Timer?

Systemd timers are systemd's alternative to cron jobs. They offer several advantages:

- Better logging and error handling through systemd's journal
- Accurate tracking of last execution time
- Ability to run jobs that missed their time slot (e.g., if the system was powered off)
- Can trigger on a variety of time-based and system events
- Integration with systemd's dependency system

A systemd timer setup consists of two units:
1. A `.service` file that defines what to execute
2. A `.timer` file that controls when the service is triggered

Common timer patterns include:
- `OnBootSec`: Run after system boot with a specified delay
- `OnUnitActiveSec`: Run at a fixed interval
- `OnCalendar`: Run at specific times (e.g., "Mon,Wed *-*-* 02:00:00")

## Usage

1. Install cookiecutter:
```bash
pipx install cookiecutter
```

or

```bash
uvx install cookiecutter
```

etc.

1. Generate the template:
```bash
cookiecutter gh:path-to-your-repo
```

## Variables

- `project_name`: Name of your service (will be used for both .service and .timer files)
- `service_description`: Description of what your service does
- `working_directory`: Full path to your project's working directory
- `exec_command`: The command to execute (full path recommended)
- `boot_delay`: Delay after boot before first execution (e.g., "10s", "1min")
- `interval`: Time between executions (e.g., "30min", "1h", "1d")

## Installation

After generating the files:

1. Copy the files to systemd's system directory:
```bash
sudo cp *.service *.timer /etc/systemd/system/
```

2. Reload systemd daemon:
```bash
sudo systemctl daemon-reload
```

3. Enable and start the timer:
```bash
sudo systemctl enable your-service.timer
sudo systemctl start your-service.timer
```

## Checking Status

Check timer status:
```bash
systemctl status your-service.timer
```

Check service status:
```bash
systemctl status your-service.service
```

List all timers:
```bash
systemctl list-timers
```

## Advanced Timer Examples

Here are some examples of more advanced timer configurations:

```ini
# Run at midnight every day
OnCalendar=*-*-* 00:00:00

# Run every Monday at 2am
OnCalendar=Mon *-*-* 02:00:00

# Run on the first day of every month
OnCalendar=*-*-01 00:00:00

# Run every 2 hours
OnUnitActiveSec=2h

# Run 1 hour after boot, then every 24 hours
OnBootSec=1h
OnUnitActiveSec=24h
```

You can modify the generated .timer file to use any of these patterns based on your needs.
