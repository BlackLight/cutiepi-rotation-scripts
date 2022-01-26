# cutiepi-rotation-scripts

A set of scripts and services to manage display rotation on the CutiePi tablet.

It interacts with the CutiePi gyroscope over `iio-sensor-proxy` to detect the
orientation of the device, and it manipulates the display rotation using some
simple `xrandr` and `xinput` scripting.

## Installation

1. Ensure that the `iio-sensor-proxy` package is installed:

```shell
$ sudo apt install iio-sensor-proxy
```

2. Clone the repo:

```shell
$ git clone https://github.com/BlackLight/cutiepi-rotation-scripts
$ cd cutiepi-rotation-scripts
```

3. Simply add the `bin` folder to your `PATH`, or copy/symlink its content
   somewhere in your `PATH`

4. If you want to run the `autorotate` script as a startup service:

```shell
$ mkdir -p ~/.config/systemd/user
$ cp systemd/autorotate.service ~/.config/systemd/user
$ systemctl --user daemon-reload
# Start the service
$ systemctl --user start autorotate
# Enable the service at startup
$ systemctl --user enable autorotate
```

## Usage

As shown in the previous example, the `autorotate` script can be enabled as a
startup service and it can automatically handle the rotation of the display by
polling gyroscope sensor data.

There are also two auxiliary scripts (`screen-rotate-left` and
`screen-rotate-right`) that can be programmatically called in your scripts or
launched from the panel to manually force display orientation.

## Notes

The `autorotate` script disables the HDMI input when started. The reason is
that the calculation of the coordinate transformation matrices for the touchpad
is much more complex when working with extended displays. If your tablet is
physically connected to a screen, however, the chances that you may want to
leverage auto-rotation are quite low anyway, so a workaround could be a simple
script that turns on the HDMI input and stops the `autorotate` service when an
HDMI input is plugged, and restarts it when it's unplugged.

If a strong use case exists for handling auto-rotation on extended displays,
feel free to open an issue.

