## App Details explained
### Standard Apps
This block lists a group of „app types“ expected to be present on any Android
device – such as Calendar, Contacts, Homescreen etc. For each of these
categories, there's usually an app pre-installed with the shipped device/ROM.
Should there be multiple apps installed to perform a given action (e.g. multiple
camera apps are not that unusual), one is usually prompted at each corresponding
action to choose an app – to be used always, or just now. Does one choose
„always“, the app becomes standard and the user is not asked again (until this
standard is reset in settings), otherwise no standard app is set and the user
asked again next time.

In this block, an asterisk (`*`) indicates no standard app was found here, but
the app shown was the first one listed for the action – which usually means it
was the last one used.


### Installed Apps
#### Organization
First level of organization is pretty clear: System Apps and User Apps are
listed in separate files (by default, `sysApps.md` and `userApps.md`). Inside
each of these files, apps are grouped by the source they came from. At least
that's the theory; the next paragraph will explain why reality differs a little.

To determine the source, *Adebar* uses the package's `installer` attribute. This
is set by the managing app that installed it – usually the apps of Google Play,
F-Droid etc. There are three exceptions where *Adebar* has no chance to determine
the „real source“:

* if you used `adb install` from your computer
* if you used `pm install` from the shell on your Android device
* if you downloaded/copied an `.apk` to your device and invoked the package
  installer directly by „tapping“ the `.apk` file in your favorite on-device
  file manager – or the app you've used to install the new app deferred the
  installation itself to the package manager (e.g. F-Droid running as user
  app without the help of its „privileged extension“ as system app).

In those three cases, the installer property will either be `none` or
`com.android.packageinstaller` (the package installer itself) – and the generated
report will group the apps under those headings.

If you uninstalled some apps (`adb shell pm uninstall --user 0 <packageName>`)
to get rid of unwanted bloatware, and have not disabled the corresponding section
in your *Adebar* config for the device (`MK_UNINSTAPPS=0`), there will be a third
section listing those apps. Pointer: if you're looking for help on debloating,
[Universal Android Debloater](https://gitlab.com/W1nst0n/universal-android-debloater)
might come in handy. It helps you identifying bloatware on your Android device,
from generic ones (Google-provided) over manuafacturer specific (Samsung, Xiaomi,
Huawei & co) up to carrier specific ones, generating the uninstall commands as
well as the ones to restore them in case you want them back (`adb shell pm
install-existing <packageName>`).


#### Per-App Details
Looking at the app details in either `sysApps.md` or `userApps.md`, not all
fields are self-explaining – at least if you're not a developer. So I'll try to
shed some light here, going by an example entry and field-by-field, giving you
an idea what each line might be about. First, the example entry – taken from an
app on an *Android Go* device running Android 8.1 (more precisely: a
[Wiko Sunny 3](https://www.gsmarena.com/wiko_sunny3-9733.php)):

> + [Smart Assist - Clean & Boost & Security](https://www.appbrain.com/app/com.ape.cleanassist)
>     + storage used (app/data/cache): 30 MiB / (unknown) / 14 KiB
>     + first installed: 2008-12-31 17:00:00
>     + last updated: 2008-12-31 17:00:00
>     + installed version: 9.0.10.14 (901014)
>     + signature scheme: 1
>     + IDs:     userId=10108
>     + CodePath: `/system/vital-app/SmartAssist_CA`
>     + App data: `/data/user/0/com.ape.cleanassist`
>     + Primary CPU ABI: `armeabi`
>     + supported screen sizes: [small, medium, large, xlarge, resizeable, anyDensity]
>     + flags: [ HAS_CODE ALLOW_CLEAR_USER_DATA ]
>     + privateFlags: [ PRIVATE_FLAG_ACTIVITIES_RESIZE_MODE_UNRESIZEABLE ]
>     + User 0: ceDataInode=-4294966108 installed=true hidden=false suspended=false stopped=true notLaunched=true enabled=0 instant=false virtual=false

The header should be clear: It's the app name (if available, otherwise the app's
package name) – if possible linked to a place where it can be obtained from.
Where the link goes to depends on where the app was installed from – and which
URL you have defined for that source (see `APP_MARKET_URL` in [[Configuration]]).

* **category:** the optional attribute `appCategory` was added with Oreo
  (Android 8.0), see [developer reference](https://developer.android.com/reference/android/R.attr.html#appCategory).
  They seem not to show up on all Android 8+ devices, though. Categories are described as:
    * game: Apps which are primarily games
    * audio: Apps which primarily work with audio or music, such as music players
    * video: Apps which primarily work with video or movies, such as streaming video apps
    * image: Apps which primarily work with images or photos, such as camera or gallery apps
    * social: Apps which are primarily social apps, such as messaging, communication, email, or social network apps
    * news: Apps which are primarily news apps, such as newspapers, magazines, or sports apps
    * maps: Apps which are primarily maps apps, such as navigation apps
    * productivity: Apps which are primarily productivity apps, such as cloud storage or workplace apps
* **storage used (app/data/cache):** how much storage space an app consumes.
  These details are not available before Android 7 (Nougat), and are incomplete
  until Android 8 (Oreo) (on *Android Go* even with Oreo it seems) – so you might
  either miss them completely or see `(unknown)` entries for app data here on
  older Android versions.
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
* **installed version:** version of the app. In (parenthesis) you will see the
  „versionCode“: this is an internal number Android uses to tell versions apart.
  A higher versionCode always means a newer version (and Android usually refuses
  to „update“ an app to an older version, going by this number). Whereas the
  other part (called „versionName“) is what is shown to the user, and has no
  „technical relevance“; two different versions could show the same versionName
  – and the newer version could even show a name that „looks older“ (though such
  cases are pretty rare).  
  More fun with pre-installed apps: the last version available on Play Store was
  8.0.30.18 for this app. But we got 9.0.10.14 here. No idea where from.
* **signature scheme:** refers to the [APK
  signing](https://source.android.com/security/apksigning) process – which is to
  ensure nobody tampers with an APK once its developer has released it, and thus
  to prevent e.g. malware injection. The higher the number here, the harder the
  protection should be to circumvent. Signing scheme v2 was introduced with
  Android 7 (aka Nougat), and thus not understood by Android 6 and below; v3 was
  introduced with Android 9 (Pie). To ensure maximum protection while still
  maintaining compatibilty with older Android versions, developers can apply
  multiple signing schemes concurrently; each Android version then would rely on
  the highest one found that it understands (e.g. a device running Nougat would
  ignore v3 and v1 and pick v2, even if all 3 are available – but accept v1 if
  no v2 signature were present).  
  Another score against pre-installed apps here: though this device runs Android
  8.1 (Oreo) and thus would easily understand a v2 signature, several (luckily
  not all) of its pre-installed apps are only using v1 signatures.
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
  show up here. In the example we see the app was installed for user0 (`installed=true`)
  who never started it at all (`notLaunched=true`), and some more.
* **disabled components:** which parts of an app are disabled (if any). Disabled
  components often correspond to functionality you deactivated via the app's
  settings – or, on a rooted device, some advanced auto-start manager.
