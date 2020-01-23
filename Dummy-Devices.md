## Dummy-Devices
Dummy-Devices are a kind of „Software-Clone“ created from a real device: All
details *Adebar* usually gathers from your Android device via ADB are stored
into text files. This is mostly intended for development and troubleshooting
purposes – for example, if you have problems with *Adebar* and your device, you
can create an issue, send the „Dummy Device Data“ – and someone experienced can
look into it without having the real device.


### Creating a dummy device
For creation of dummy devices, you find a script named `mk_dummy` in the `tools/`
directory of Adebar. This script will do most of the work for you; just follow
these steps:

1. copy the script into a „working directory“ where the dummy shall be created
1. enable *USB Debugging* in your device and connect it to your computer
1. in the directory where you copied `mk_dummy` into, run `./mk_dummy <name>`
   (replace `<name>` with the name you want to give that device, e.g.
   `bqx5_90` for a *BQ Aquaris X5* running Android 9. Note that no spaces are
   allowed in the name.
1. watch the process: `mk_dummy` will create a new directory with the name you
   gave the device (`bqx5_90` in above example), collect the details from your
   „real device“, and store them into files located in that directory. This can
   take a little – but not much longer than a minute.

Now you've got your DummyDevice. If you want to send that to someone for
investigation, simply pack up the directory into an archive, like

* `zip -ro9 bqx5_90.zip bqx5_90/*` or
* `tar cjf bqx5_90.tar.bz2 bqx5_90/*` or
* `tar czf bqx5_90.tar.gz bqx5_90/*`

Now you can transfer the archive by mail, upload or whatever.


### Using a dummy device
If you want to make use of a dummy device yourself, just create a config for it
(see [[Configuration]] for details). The two important parameters here are:

* `DUMMY_BASE=$HOME/adebar/dummies` (where your dummy devices reside)
* `DUMMY=bqx5_90` (name of the dummy device you want to use)

Optionally you can also specify a different `STORAGE_BASE`. Everything else is
configured as done for a real device – though some things will be ignored by
Adebar here:

* things to be transferred *to* the device (this cannot be done for dummies)
* some app-specific details (like disabled components)
* files to be pulled (like configuration files, or XPrivacy data)


### Sending dummy device data
Before sending your DummyDevice data across the network, be aware that they
can contain personal data – so you want to sanitze them. For example, replace
email addresses in the `dumpsys_account` file by something like
`user@example.com`. Further, your device's IMEI might be present in
`dumpsys_iphonesubinfo` or `getprop`, `dumpsys_telephony.registry` might contain
phone numbers, and so on.

It might not be the best idea to place such a dummy device into the public. So
better send them in a safe way (e.g. a PGP encrypted mail). When unsure, ask.
