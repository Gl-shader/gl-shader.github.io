---
title:  "DoS Wechat with an emoji"
date:   2019-05-14 17:22:33 +0800
categories: Hacking
classes:
  - landing
---

This DoS bug was reported to Tencent, but they decided not to fix because it's not critical.

## Description:
vcodec2_hls_filter in libvoipCodec_v7a.so in WeChat application for Android results in a DoS by replacing an emoji file (under the /sdcard/tencent/MicroMsg directory) with a crafted .wxgf file.

## Attack vector:
Crash-log is provided in poc.zip file at https://drive.google.com/open?id=1HFQtbD10awuUicdWoq3dKVKfv0wvxOKS

An malware app can crafts a malicious emoji file and overwrites the emoji files under /sdcard/tencent/MicroMsg/[User_ID]/emoji/[WXGF_ID]. Once the user opens any chat messages that contain an emoji, WeChat will instantly crash.

## POC:
Video at https://drive.google.com/open?id=1x1Z3hm4j8f4rhv_WUp4gW-bhdtZMezdU

User must have sent or received a GIF file in WeChat

Malware app must retrieve the phone's IMEI. For POC, we can use the below command
adb shell service call iphonesubinfo 1 | awk -F "'" '{print $2}' | sed '1 d' | tr -d '.' | awk '{print}' ORS=- 

Produce the malicious emoji file with the retrieved IMEI (use encrypt_wxgf.py in poc.zip) by running "python encrypt.py crash4.wxgf [SIZE_OF_EMOJI_ON_SDCARD]" to produce out.wxgf

Replace /sdcard/tencent/MicroMsg/[User_ID]/emoji/[WXGF_ID] with the padded out.wxgf.encrypted

WeChat will crash now if a message that contains the overwritten emoji file
