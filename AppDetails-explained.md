## App Details explained
Looking at the app details in either `sysApps.md` or `userApps.md`, not all
fields are self-explaining – at least if you're not a developer. So I'll try to
shed some light here, going by an example entry and field-by-field, giving you
an idea what each line might be about. First, the example entry – taken from an
app on an *Android Go* device running Android 8.1 (more precisely: a
[Wiko Sunny 3](https://www.gsmarena.com/wiko_sunny3-9733.php)):

> + [Smart Assist - Clean & Boost & Security](https://www.appbrain.com/app/com.ape.cleanassist)
    + first installed: 2008-12-31 17:00:00
    + last updated: 2008-12-31 17:00:00
    + installed version: 9.0.10.14
    + IDs:     userId=10108
    + CodePath: `/system/vital-app/SmartAssist_CA`
    + App data: `/data/user/0/com.ape.cleanassist`
    + Primary CPU ABI: `armeabi`
    + supported screen sizes: [small, medium, large, xlarge, resizeable, anyDensity]
    + flags: [ HAS_CODE ALLOW_CLEAR_USER_DATA ]
    + privateFlags: [ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_UNRESIZEABLE ]
    + User 0: ceDataInode=-4294966108 installed=true hidden=false suspended=false stopped=true notLaunched=true enabled=0 instant=false virtual=false

The header should be clear: It's the app name (if available, otherwise the app's
package name) – if possible linked to a place where it can be obtained from.
Where the link goes to depends on where the app was installed from – and which
URL you have defined for that source (see `APP_MARKET_URL` in [[Configuration]]).

* **first installed:** when the app was initially installed. For apps that came
  pre-installed with the device, this often is a fictive date, as in this
  example.
* **last updated:** when the app was last updated. Again the example gives us
  a „special case“ – which unfortunately is rather a rule than an exception:
  not only do pre-installed apps [often come with quite serious security
  issues](https://www.wired.com/story/146-bugs-preinstalled-android-phones/);
  Heise also [cites a study](https://heise.de/-4630516) saying 91% of them are
  never updated. In this example, the app was removed from Play Store in March
  2018 – but came pre-installed on this device which was obtained in mid-2019…
* **last used:** some apps also state when they were last used – i.e. when the
  user actively opened its interface the last time. Obviously empty if that
  didn't happen at all.
* **installed version:** version of the app. More fun with pre-installed apps:
  the last version available on Play Store was 8.0.30.18 here. But we got
  9.0.10.14 here. No idea where from…
* **IDs:** any IDs associated with this app. Here it's *userId* – which is the
  ID used on OS level for access permissions etc. On a shell, if this app is
  running, you should be able to grab its process(es) via the `ps` command.
* **CodePath:** where the `*.apk` of the app can be found. Though this app is
  listed as user app on the device, this path tells us it came pre-installed
  (apps installed by the user end up below `/data`, not `/system`)
* **App data:** path where the app stores its (local) data. In general, only the
  app itself (and of course the Android system / the `root` user) has access
  to this.
* **CPU ABI:** [Application Binary
  Interface](https://developer.android.com/ndk/guides/abis) – basically the
  architecture the app was written for. An app can support multiple ABIs (you
  might additionally see a „Secondary CPU ABI“ here), and some ABIs are
  „compatible with others“ (if you e.g. have an ARM64-v8 device, it can also
  run apps using armeabi-v7a or armeabi).
* **supported screen sizes:** as the name says. Some apps are intended for
  large screens only, in which case they won't show the full list here.
* **flags, privateFlags:** the meaning of each flag you can find in the source
  code of `ApplicationInfo.Java`, which can e.g. for Android 8 (Oreo) be [found
  here](http://androidxref.com/8.0.0_r4/xref/frameworks/base/core/java/android/content/pm/ApplicationInfo.java).
  The `PRIVILEGED` privateFlag e.g. means an app has access to privileged
  permissions.
* **mtkFlags:** These are flags specific to MTK devices.
* **User X:** the status of this app for each user on the device. If you never
  created any user other than the first, only „User 0“ (the „device owner“) will
  show up here.

