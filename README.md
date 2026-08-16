# Sleep Guardian

An Omarchy Quattro idle manager based on Pierre Berube's MIT-licensed
[Sandman](https://github.com/lgse/sandman). It retains Sandman's screen-saver,
display-off, auto-lock, and custom idle timers, and adds selectable systemd power
actions:

- Suspend
- Hibernate
- Suspend then hibernate
- Hybrid sleep

Unsupported actions are disabled after querying `systemd-logind`; an invalid or
manually corrupted action falls back to suspend. Idle inhibitors are respected,
configuration writes are atomic, unrelated Omarchy settings are preserved, and
the power timer is off by default.

## Install

```sh
omarchy plugin add https://github.com/thenailedone/omarchy-sleep-guardian-plugin.git --enable
```

Click the Sleep Guardian icon in the bar, choose an available power action, and
then choose a timeout. `Suspend → Hibernate` invokes systemd's native
`suspend-then-hibernate` mode. Its transition delay comes from the system-wide
`HibernateDelaySec=` setting; when no delay is configured systemd may use battery
information or its documented two-hour default.

State is stored in `~/.config/omarchy/sleep-guardian.json`. Screen-saver and lock
values are also merged into `~/.config/omarchy/shell.json`; display and power
timers remain plugin-owned.

## Validate

```sh
npm test
omarchy plugin validate .
qmllint -I "$OMARCHY_PATH/shell" BarWidget.qml Panel.qml Service.qml
```

The tests never invoke a sleep command.

## Remove

Set screen saver and lock to the values you want to retain, then run:

```sh
omarchy plugin remove thenailedone.sleep-guardian
```

Removing the plugin does not revert screen-saver or lock values already stored in
Omarchy's `shell.json`.

## License

MIT. The upstream Sandman copyright and license are retained in `LICENSE`.
