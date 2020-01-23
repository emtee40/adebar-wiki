## Tools shipped with Adebar
I decided to include some tools with *Adebar* – mainly some scripts I use in the
same area (i.e. device backups and documentation). You can find them in the
`tools/` sub directory. To give you a raw idea what they are about, they shall
be listed here:

* `ab2tar`: convert an ADB backup file (`*.ab`) into a
  [Tarball](https://en.wikipedia.org/wiki/Tar_(computing)) in order to
  investigate its contents. Called without parameters, it will reveal its
  syntax. Passed the file name of the backup, it will do its job.
* `abrestore`:  Fixing `adb restore` for devices with Android 7 (Nougat) and
  above. Some of them don't restore a backup if the app itself isn't yet
  installed. This script works around that by extracting and installing the APK
  before calling `adb restore`. Again, expects the backup file as only parameter.
* `getapk`: Grab APK files from your device via ADB. Which ones, depends on the
  parameter you pass to it: a single APK when given a package name, all user
  apps, all system apps – or even all apps. Call without parameters for details.
* `mk_dummy`: Script to create [[Dummy-Devices]].
* `ssnap`: a (serial) screenshot snapper. It will put your device into demo mode,
  set the clock to the specified value while hiding all your „real notifications“,
  optionally starting an app if you specified its package name, and then perform
  a screenshot (saved locally on your computer) whenever you hit the „s“ key.
  Once you hit the „q“ (for „quit“), it tells the device to exit demo mode (i.e.
  return it to „normal state“), and exists.  
  Call it with the `-h` parameter for more details.
