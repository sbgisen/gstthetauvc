# gstthetauvc - Gstreamer THETA UVC plugin

## Build and Install

### Prerequisites

Gstreamer development package (on Ubuntu, libgstreamer1.0-dev) and [libuvc with UVC1.5/H.264 support](https://github.com/ricohapi/libuvc-theta) are required.

- If libuvc is installed to non-standard directory, you may need to set location of the libuvc.pc file to `PKG_CONFIG_PATH` environment variable.

### Build

Just run `make` at gsttehtauvc/thetauvc.

### Install

Copy gstthetauvc.so into the gstreamer plugin directory or wherever you like.

- If you copied into directory other than the gstreamer plugin directory, you need to set the directory path to the `GST_PLUGIN_PATH` environment variable.

## Plugin properties

    mode   : Video mode to playback
             flags: readable, writable
             Enum "GstThetauvcMode" Default: 0, "2K"
                (0): 2K               - 1920x960(THETA V/Z1)/1920x1080(THETA S)
                (1): 4K               - 3840x1920(THETA V/Z1)
    serial : The serial number of the THETA to use.
             Useful if multiple THETAs are connected to the system.

For other properties, run `gst-inspect-1.0 thetauvcsrc`.

## Example

### View 4K streaming on the display

    gst-launch-1.0 thetauvcsrc mode=4K ! queue ! h264parse ! decodebin ! queue ! autovideosink sync=false

### Read into OpenCV

    VideoCapture cap("thetauvcsrc ! decodebin ! autovideoconvert ! video/x-raw,format=BGRx ! queue ! videoconvert ! video/x-raw,format=BGR ! queue ! appsink");

- Note:
  - OpenCV should be build with gstreamer backend enabled.
  - You may need to replace autoplugins (e.g. deocdebin/autovideoconvert/autovideosink) with platform specific plugins.
    - On Jetson platform, nvv4l2decoder/nvvidconv/nv3dsink respectively.
