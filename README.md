# Device configuration for Moto MAXX (Quark)

Copyright 2015 to Today - Felipe Leon Project :sunglasses:<br/>
Copyright 2015 to 2016 - The CyanogenMod Project<br/>
Copyright 2017 - 2018 - The LineageOS Project

## I use this tree to build in Oreo lineage-16.x base sources

This tree works prefect on [ResurrectionRemix Pie](https://github.com/ResurrectionRemix/platform_manifest/tree/pie) source also in [LineageOS 16.0](https://github.com/LineageOS/android/tree/lineage-16.0) but I don't run or build LineageOS regularly so it may not build, but the fixes are usually very simple if you can't fix it **mention** @fgl27 on one of the XDA threads links in [device_motorola_quark/wiki](https://github.com/fgl27/device_motorola_quark/wiki)

### How to build this...

Pull the below repos creating a file **"/home/user/source_folder/.repo/local_manifests/roomservice.xml"** and pasting the bellow

	<?xml version="1.0" encoding="UTF-8"?>
	<manifest>
	
	  <!-- Strings for LineageActions app-->
	  <project name="LineageOS/android_packages_resources_devicesettings" path="packages/resources/devicesettings" remote="github" revision="lineage-16.0" />

	  <!-- Radio ralated lib-->
	  <project name="LineageOS/android_system_qcom" path="system/qcom" remote="github" revision="lineage-16.0" />
	
	  <!-- Device/kernel/vendor-->
	  <project name="fgl27/device_motorola_quark" path="device/motorola/quark" remote="github" revision="P" />
	  <project name="fgl27/BHB27Kernel" path="kernel/motorola/apq8084" remote="github" revision="P" />
	  <project name="fgl27/proprietary_vendor_motorola" path="vendor/motorola" remote="github" revision="P" />

	</manifest>

If yours source file **"/home/user/source_folder/.repo/manifests/default.xml"** doesn't have the remote github add the below under **<manifest\>** line in previously created file **"/home/user/source_folder/.repo/local_manifests/roomservice.xml"**.

	  <remote  name="github"
	           fetch="https://github.com/" />

### Fix the source to build for Quark

From source main folder do

### This will remove wifi logs spams, technically not need but improve log reading and background performance

	cd system/connectivity/wificond/
	git fetch https://github.com/fgl27/system_connectivity_wificond/ Pie && git cherry-pick f695a663f751814ab35e30791693d784649fad4e^..31b7bd81e031bbe9505c82bc15670e4281b00d34
	cd -

	cd frameworks/opt/net/wifi/
	git fetch https://github.com/fgl27/android_frameworks_opt_net_wifi/ lineage-16.0 && git cherry-pick d9810f5343c626cfd4223c71aa37980a23a34256^..9a61195fd803c43ad1924ee513d73873f57c1918
	cd -

### This is only needed in LineageOS,they will fix wifi display support

	cd frameworks/av

	git fetch https://github.com/LineageOS/android_frameworks_av refs/changes/27/238927/2 && git cherry-pick FETCH_HEAD
	git fetch https://github.com/LineageOS/android_frameworks_av refs/changes/28/238928/2 && git cherry-pick FETCH_HEAD
	git fetch https://github.com/LineageOS/android_frameworks_av refs/changes/29/238929/2 && git cherry-pick FETCH_HEAD
	git fetch https://github.com/LineageOS/android_frameworks_av refs/changes/31/238931/2 && git cherry-pick FETCH_HEAD
	git fetch https://github.com/LineageOS/android_frameworks_av refs/changes/32/238932/2 && git cherry-pick FETCH_HEAD
	git fetch https://github.com/LineageOS/android_frameworks_av refs/changes/42/239642/1 && git cherry-pick FETCH_HEAD
	cd -

	cd device/qcom/sepolicy-legacy
	git fetch https://github.com/LineageOS/android_device_qcom_sepolicy-legacy refs/changes/41/239741/3 && git cherry-pick FETCH_HEAD
	cd -

## Building after repo sync and fixing the source (fixing the source is always necessary to redo after a "repo sync"):

	export WITH_SU=true
	. build/envsetup.sh 
	make clean

### Lunch the device in LineageOS

	lunch lineage_quark-userdebug

### Lunch the device in ResurrectionRemix

	lunch rr_quark-userdebug

### Start the build

	time mka bacon -j8 2>&1 | tee quark.txt

Were the **first number** after **-j** is the number of cores you wanna use for this task and **2>&1 | tee quark.txt** will export the build "output" to a file **quark.txt**, read it in case the build fails searching for the reason of the fail.

This link ([Setup_new_build_machine](https://github.com/fgl27/scripts/blob/master/etc/new_machine.md#for-general-android-app-build-machine--adb-shell-and-fastboot-for-debugging)) may help to setup a build machine in case you don't know how to, but that is very personalized for me so carefully read it.

The Motorola Moto Maxx (codenamed _"quark"_) is a high-end smartphone from Motorola mobility.<br/>
It was announced on November 2014.

Basic   | Spec Sheet
-------:|:-------------------------
CPU     | Quad-core 2.7 GHz Krait 450
Chipset | Qualcomm Snapdragon 805
GPU     | Adreno 420
Memory  | 3GB RAM
Shipped Android Version | 4.4.4
Storage | 64 GB
MicroSD | No
Battery | Non-removable Li-Po 3900 mAh battery
Display | 1440 x 2560 pixels, 5.2 inches (~565 ppi pixel density)
Camera  | 21 MP (5248 x 3936), auto focus, dual-LED flash


![MOTO MAXX](https://raw.githubusercontent.com/fgl27/scripts/f45458e4bc40dcc6d71ed933d49dad01a3b63f4b/etc/images/moto-maxx.jpg "MOTO MAXX")
