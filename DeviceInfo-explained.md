## DeviceInfo explained
The DeviceInfo page is divided into multiple sections, which will be explained here.

### Device and ROM properties
**Product Info** gives you a general overview over the device, like its
manufacturer, the device name, and CPU architecture.

**Sensors** lists up which sensors (like Gyroscope, Accelerometer, Proximity
sensor) are available in your device – usually together with the corresponding
manufacturer. If you are not interested in this section, you can rutn it off
by setting `MK_DEVICEINFO_SENSORS=0` in your config.

**OS details** lists some information about the Android system running on your
device – line Android version, Kernel and build. And while you might be
disappointed finding out your device is not running the latest Android version,
almost more important is to check the **Security Patch Level**. This better
shows a date not too far in the past.


### Device Features
This part might be interesting to developers – but will look rather cryptic and
„useless“ to non-developers. If you belong to the latter group and want to get
rid of this section, you can do so setting `MK_DEVICEINFO_PMLISTFEATURES=0` in
your configuration.

A raw hint though: apps can „require features“. If your device doesn't meet
those, such an app would show up as being incompatible with your device, and you
cannot install it. If you are interested in cross-checking, you'll need the APK
file of the app and run `aapt d badging MyApp.apk | grep uses-feature`. Results
showing up as `uses-feature-not-required:` are optional and will not cause
incompatibility when not met – but `uses-feature:` entries are mandatory.


### Device Status
This section can be toggled by the `MK_DEVICEINFO_STATUS` config setting.

**Battery Status** should be pretty much self-explaining – telling you whether
the device is currently charged (and if so, by what source), giving details on
charging level, voltage, temperature and more. It also lets you know what type
of battery your device is using, e.g. [Li-ion](https://en.wikipedia.org/wiki/Li-ion
"Wikipedia: Lithium-ion battery") or [Li-Po](https://en.wikipedia.org/wiki/Li-Po
"Wikipedia: Lithium polymer battery").

**Radio Status** gives you details on your device's mobile radio. The
[Baseband Processor](https://en.wikipedia.org/wiki/Baseband_processor) is what
enables your device to communicate with the mobile network (think of it as a
kind of „network card“) – and it has its own software on a separate partition
(which in turn you can think of as the corresponding „network driver“). The RIL,
or spelled out completely [Radio Interface
Layer](https://en.wikipedia.org/wiki/Radio_Interface_Layer), is what lets the
Baseband Processor (or rather its software) communicate with the Android kernel.

Following the general Baseband information are details on each SIM slot available
in your device: Signal Strength (linked to an article on Android.SE explaining
it), Service State (which network the SIM is connected to, whether the network
was selected manually or automatically, details on connectivity. Again, entries
should hopefully be understood without much explanation.

**Provider Info** tells you what network provider the SIM was obtained from –
including information on its country. The *provider code* is a combination of its
[MCC](https://en.wikipedia.org/wiki/Mobile_country_code "Wikipedia: Mobile country
code") (first 3 digits) and the providers MNC (Mobile Network Code, last 2 digits).

**Networking details** hopefully gives you some details on your device's WiFi.
Some devices are a bit reluctant to hand them out it seems.


#### Storage details
This section gives you some information on your device's storage, including
some disk statistics. If your configuration has `MK_PARTINFO=1`, you will also
find details on your device's **partitions** here:

Depending on manufacturer, device, ROM and possible other modifications, Android
devices offers different ways to identify partitions – with varying relyability
and details. By default, *Adebar* tries to figure the best available source
automatically; alternatively you can specify the source to use via the
configuration variable `PARTITION_SRC` (e.g. in the rare case I didn't yet
encounter where *Adebar* made a sub-optimal choice). In the order of reliability
and best details provided, possible values for this variable are:

* `auto`: let *Adebar* detect the best source itself (default)
* `dumchar`: use details from `/proc/dumchar_info`. This is only available on
  MediaTek (MTK) devices.
* `mtd`: use details from `/proc/mtd`, augmented by `/proc/mounts` when needed
  and possible.
* `emmc`: use details from `/proc/emmc`, augmented by `/proc/mounts` when needed
  and possible.
* `emmc`: use details from `/proc/emmc`
* `byname`: use details from `/dev/block/platform/*/by-name`, matched against
  `/proc/partitions` and `/proc/mounts` for names
* `parts`: use details from `/proc/partitions` and cross-check with `/proc/mounts`
  for names

The generated DeviceInfo document will tell you which source was used.

Finally, storage details will show you some **Disk statistics:** how much space
does your device offer, how much is used and by what. Basically what you can find
on-device in *Settings › Storage.*

#### SafetyNet
This section tries to sum up some [SafetyNet](https://www.lineageos.org/Safetynet/
LineageOS.Org: What is SafetyNet, and how it affects you") related data. It's by
far not complete (as it's not easy for a Shell program like *Adebar* to test that
in depth), but at least gives you some basic indicators on whether *SafetyNet*
might be triggered on your device – e.g. because you unlocked your bootloader.

General implication: with *SafetyNet* triggered, some applications will refuse to
work. Just like that. Some of them you shouldn't use on a smartphone anyway (like
banking apps, *Google Pay* or other financial applications – not at last because
of their „tracking features“, but also for general security reasons in case your
device gets lost/stolen). Others include DRM related apps like *Netflix* – or
several games. I haven't yet heard about any FOSS app caring about this, though.
So think what you want if this really is about safety – and if so, whose.

Want further details? HowtoGeek might help you out with [SafetyNet Explained:
Why Android Pay and Other Apps Don’t Work on Rooted
Devices](https://www.howtogeek.com/241012/safetynet-explained-why-android-pay-and-other-apps-dont-work-on-rooted-devices/).
For details on what is checked (and not covered by *Adebar's* simple checks),
see e.g. [Inside SafetyNet](https://koz.io/inside-safetynet/) (caution, it's
behind Cloudflare).


### Configured Accounts
This will give you a list of accounts on your device. Usually this includes your
Google Account (unless you run Google-free – congrats in that case!); but also
Threema or big GDPR-violators like WhatsApp/Facebook (are you nuts?) will show
up here when configured.


### Google Cloud Backup
Last in this list: the local setup of Google Cloud Backup. Even there on devices
running pure LineageOS with no GApps installed (though then usually the
introduction will be „Backup Manager is disabled“. This section will tell you
whether backup/restore is enabled, what backup destinations are defined, when
the last backup was run – and which of your installed apps are supporting it
(count them against the number of installed apps; I'd be surprised if they make
more than 20%).
