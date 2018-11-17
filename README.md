# OctoPrint-BedLevelVisualizer - FORK that works with SMOOTHIEWARE M375.1

This plugin utilizes [Plotly](https://plot.ly/plotly-js-scientific-d3-charting-library/) js library to render a 3D surface of the bed's reported mesh on a tab within OctoPrint. It converts this

```
Send: M375.1
Recv:   140.0000|   23.0109     0.9860     0.5512     0.6398     0.6475     0.6553    -7.7516
Recv:   116.6667|    0.2795     0.3742     0.4596     0.5155     0.5512     0.5870     0.5839
Recv:    93.3333|    0.3245     0.3804     0.4255     0.4286     0.4658     0.5745     0.5776
Recv:    70.0000|    0.2811     0.3602     0.4099     0.5078     0.5637     0.5683     0.6118
Recv:    46.6667|    0.2174     0.3494     0.4037     0.4239     0.4907     0.5745     0.6134
Recv:    23.3333|    0.2143     0.3043     0.3820     0.4146     0.4394     0.4705     0.4006
Recv:     0.0000|    0.0186     0.2345     0.3121     0.3447     0.3665     0.4301     0.6040
Recv:            -----+----------+----------+----------+----------+----------+----------+-----
Recv:            0.0000 23.3333 46.6667 70.0000 93.3333 116.6667 140.0000
Recv: ok
```
into this

![screenshot](screenshot.png)

## Known Issues
  - Since version 0.1.3 there is a python dependency on numpy.  As a result; if you don't already have numpy the install can take in excess of 30 minutes to complete on a pi. Just be patient and let it run and eventually the plugin install will finish.
  - If you have Marlin's Auto Temperature Reporting Feature enabled you will want to have M155 S30 and M155 S3 surrounding your G29 command, see settings screenshot, otherwise the collected data will be tainted.
  - ~~Currently there is a conflict with the TempsGraph plugin.  If you have this plugin installed you will receive an error that Plotyle.react is not a function.  There is a version update pending on that plugin to resolve this issue, just waiting on the author to release.~~ Resolved with TempsGraph release [0.3.3](https://github.com/1r0b1n0/OctoPrint-Tempsgraph/releases/tag/0.3.3).

## Settings

![screenshot](settings_general.png)

![screenshot](settings_stored_mesh.png)

## Tips
  - If your leveling method requires homing first make sure to enter that as well in the GCODE Commands setting.
  - If you have Marlin's Auto Temperature Reporting feature enabled you will want to have M155 S30 and M155 S3 surrounding your reporting GCODE command, otherwise the collected data will be tainted with temperature information.
  - If you end up requiring multiple commands it is recommended to enter `@BEDLEVELVISUALIZER` just prior to the reporting command.

    ~~~
	G28	
    M155 S30	
    @BEDLEVELVISUALIZER	
    G29 T	
    M155 S3
	~~~

## Setup

Install via the bundled [Plugin Manager](https://github.com/foosel/OctoPrint/wiki/Plugin:-Plugin-Manager)
or manually using this URL:

    https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/archive/master.zip

## Changelog

**[0.1.6]** (09/06/2018)

**Added**
  - Repetier firmware support thanks to @gztproject.

**[0.1.5]** (09/03/2018)

**Added**
  - Option to make center of bed the origin point per request.  Helpful when using a fixed center leveling system as described [here](https://github.com/PrusaOwners/prusaowners/wiki/Bed_Leveling_without_Wave_Springs).
  - Option to make measured offsets relative to origin position, related to above addition but could be useful elsewhere.
  
**Changed**
  - X/Y axis calculations to resolve bug discovered during above changes where if your leveling grid was based on an odd number of probe points the maximum perimeters were getting dropped due to rounding errors.
  
**[0.1.4]** (08/06/2018)

**Fixed**
  - Issue introduced with previous update that was causing some leveling reports to not be identified correctly.

**[0.1.3]** (08/05/2018)

**Added**
  - Recognition of older bed reports to work for some users.  Should now work with reports that return lines like `Bed x: 40.00 y: 20.00 z: 7.31`.

**[0.1.2]** (06/09/2018)

**Added**
  - Display of last mesh timestamp above Update button.

**[0.1.1]** (06/01/2018)

**Changed**
  - Mobile detection in plotly js library to also detect iPad as webgl device.

**[0.1.0]** (05/02/2018)

**Added**
  - Flip X/Y settings added to allow changing the orientation of the surface displayed.
  - Added Remove Row Labels in order to shift the mesh data to account for some reports that return an index at the beginning of the line.
  - Added bootstrap tooltips to info icons.
  - Wizard added on install to explain/enter the GCODE commands and demonstrate how to use the `@BEDLEVELVISUALIZER` command.

**Changed**
  - Improved graph display by reading build volume from printer profile settings.
  - Fixed Z range on graph between -2mm and 2mm.
  - Updated settings dialog for new options and moved stored data to it's own tab.
  - Simiplified data extraction regular expression.
  - Data collection now triggered based on sending the GCODE command `@BEDLEVELVISUALIZER`.
  
**Removed**
  - Prusa Firmware mode no longer necessary due to added options listed above.

**0.0.9** (skipped)

**[0.0.8]** (04/22/2018)

**Added**
  - Pop-up notification on error.

**Changed**
  - Removed placeholder attribute on  Data Collector Flag to make it more obvious there's nothing typed in it.

**Fixed**
  - Delta printers not collecting mesh data points.

**[0.0.7]** (04/20/218)

**Changed**
  - Description in Plugin Manager.

**[0.0.6]** (04/19/2018)

**Added**
  - Timestamp stored with saved mesh data.  Displays on mouse hover of info graphic on update button.
  
**Changed**
  - Screenshots updated to make settings a little clearer.
  - Visibility bindings fixed for issues related to new installs and no stored data.

**[0.0.5]** (04/18/2018)

**Added**
  - Prusa Firmware Mode setting, to handle G81 responses correctly

**Changed**
  - Graph is now always visible in OctoPrint as long as you have saving enabled, there is a mesh stored, and the user is logged in.

**Fixed**
  - Remove duplicated tabs from Prusa firmware's G81 response.

**[0.0.4]** (04/17/2018)

**Changed**
  - "Data Collector Flag" is now a text field in lieu of select list to allow full customization of the text that flags for mesh data collection to handle the numerous available options that apparently seem to be out there.

**[0.0.3]** (04/17/2018)

**Added**
  - Capture data option for identifying correct response to start storing mesh data
  - Stored data displayed in settings
  - Non-UBL support

**[0.0.2]** (04/15/2018)

**Added**
  - Settings dialog to update the GCODE command used to report bed topology.
  - Improved performance with option for storing the mesh in config.yaml (default).

**Fixed**
  - Reversed point order to fix graphing misorientation.
	
**[0.0.1]** (04/14/2018)

**Initial Release**

[0.1.6]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.1.6
[0.1.5]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.1.5
[0.1.4]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.1.4
[0.1.3]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.1.3
[0.1.2]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.1.2
[0.1.1]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.1.1
[0.1.0]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.1.0
[0.0.9]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.9
[0.0.8]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.8
[0.0.7]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.7
[0.0.6]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.6
[0.0.5]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.5
[0.0.4]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.4
[0.0.3]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.3
[0.0.2]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.2
[0.0.1]: https://github.com/jneilliii/OctoPrint-BedLevelVisualizer/tree/0.0.1

## To-Do
- [ ] Pause standard OctoPrint temperature polling or squash the responses until processing is completed.
- [X] ~~Orientation testing to verify axes are in correct direction.~~ added settings to allow controlling the orientation.
- [X] Calculate bed dimensions and apply to probe points for display on graph, #28.

## Support My Efforts
I, jneilliii, programmed this plugin for fun and do my best effort to support those that have issues with it, please return the favor and leave me a tip if you find this plugin helpful.

[![paypal](https://jneilliii.github.io/images/paypal-with-text.png)](https://paypal.me/jneilliii)

<small>No paypal.me? Send funds via PayPal to jneilliii&#64;gmail&#46;com</small>
