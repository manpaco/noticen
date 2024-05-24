# noticen

I made this project to have control of notification sounds and popups in one place. You can also add more types of notifications in the "switch/case" of the main script.

In this project are several files in [resources](/resources) that you can take as an example.

Replace the `FDTSOUNDS` variable inside [sounds.import](sounds.import) file. Also configure your sound player and notification sender on [tools.import](tools.import) file.

If you place the project on a directory other than `$HOME/.local/noticen`, then change the `MYPATH` variable on [noticen](noticen), [sysound](sysound), and [bt-helper](bt-helper).

## sysound

Internally it works with a symlink to real sounds (stored in `FDTSOUNDS`). When you turn off sounds, deletes the link. When you turn on sounds, creates the link.

## bt-helper

Makes queries to `bluetoothctl` for 10 seconds (max) until it finds a connected device. If it finds any, it shows the notification.

## Udev rules

Always remember to call the script as follows:

    /bin/su -l youruser '/path/to/noticen -x y'
    /bin/su -l youruser 'noticen -x y' # if noticen is on PATH

Use the files named like '99-*.rules' inside [resources](/resources).

## Network Manager dispatcher

First enable the dispatcher service with systemd, and then place [network-notification](/resources/network-notification) file on `/etc/NetworkManager/dispatcher.d` directory.

Remember to replace `youruser` on that file.

## Gammastep hook

Place the [gamma-notification](/resources/gamma-notification) file on `$HOME/.config/gammastep/hooks` directory.

## Recorder

You can use the [clirecorder](https://github.com/manpaco/tools/blob/main/clirecorder) in my tools repo. Modify as necessary.

## Keymaps

Copy the content of [sway-keymaps](/resources/sway-keymaps) to sway config file, `$HOME/.config/sway`. Modify as necessary.

In this case, when `noticen` is executed on volume change or screenshot only reproduce sound (pop and shutter, respectively). Popups on screenshot are sent by `grimshot` (that's why the `--notify` appears).

## Troubleshooting

If you experience delayed playback (silence in the first milliseconds of the notification sound), you can configure pipewire with the following file (place it in `$HOME/.config/wireplumber/wireplumber.conf.d` directory):

    # 51-session-suspend.conf
    # ALSA suspend: node property override
    monitor.alsa.rules = [
      {
      matches = [
        {
        node.name = "~alsa_output.pci.*"
        }
      ]
        actions = {
          update-props = {
            session.suspend-timeout-seconds = 0
          }
        }
      }
    ]
