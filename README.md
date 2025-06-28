### Device specific configuration to build AOSP Android 14 for Raspberry Pi 4.

***

NOTE: Raspberry Vanilla `android-14.0` branch is no longer maintained. Consider using newer AOSP versions.

### How to build (Ubuntu 22.04 LTS):

1. Establish [Android build environment](https://source.android.com/docs/setup/start/requirements).

2. Install additional packages:

```
sudo apt-get install bc coreutils dosfstools e2fsprogs fdisk kpartx mtools ninja-build pkg-config python3-pip rsync
sudo pip3 install meson mako jinja2 ply pyyaml dataclasses
```

3. Initialize repo:

```
repo init -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r67
curl -o .repo/local_manifests/manifest_brcm_rpi.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-14.0/manifest_brcm_rpi.xml --create-dirs
curl -o .repo/local_manifests/manifest_brcm_driveid.xml -L https://raw.githubusercontent.com/AAOSPersonalizedUsingFaceDetection/aaos-custom-manifest/main/manifest_brcm_driveid.xml --create-dirs
```

Or optionally, you can reduce download size by creating a shallow clone and removing unneeded projects:

```
repo init -u https://android.googlesource.com/platform/manifest -b android-14.0.0_r67 --depth=1
curl -o .repo/local_manifests/manifest_brcm_rpi.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-14.0/manifest_brcm_rpi.xml --create-dirs
curl -o .repo/local_manifests/remove_projects.xml -L https://raw.githubusercontent.com/raspberry-vanilla/android_local_manifest/android-14.0/remove_projects.xml
curl -o .repo/local_manifests/manifest_brcm_driveid.xml -L https://raw.githubusercontent.com/AAOSPersonalizedUsingFaceDetection/aaos-custom-manifest/main/manifest_brcm_driveid.xml --create-dirs
```

4. Sync source code:

```
repo sync
```

5. Setup Android build environment:

```
 build/envsetup.sh
```

6. Select the device (`rpi4`) and build target ( `car` for Android Automotive):


```
lunch driveid_car-ap2a-userdebug

```


7. Compile:

```
make bootimage systemimage vendorimage -j$(nproc)
```

8. Make flashable image for the device (`rpi4`):

```
./rpi4-mkimg.sh
```
