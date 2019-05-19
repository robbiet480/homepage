---
title: Airmail 3 GPG Issue
date: 2019-03-19T10:47:00+01:00
author: Djordje Atlialp
draft: false
toc: false
cover: d7e60867901a4500b0eb7362de1a525e.png
tags:
---

[Airmail 3](https://airmailapp.com/) is an alternative email client for macOS. However, the GPG plugin (to be found [here](https://help.airmailapp.com/en-us/article/plugins-airmail-for-macos-lenvq3/)) has been having great difficulty being recognised by Airmail 3 for some time now. On GitHub, there is already a long discussion about this: https://github.com/Airmail/AirmailPlugIn-Framework/issues/34.

A [comment](https://github.com/Airmail/AirmailPlugIn-Framework/issues/34#issuecomment-442918633) from [@y3sh](https://github.com/y3sh) was especially helpful because it describes how to solve the problem locally.

```bash
# Libmacgpg
git clone --recursive https://github.com/GPGTools/Libmacgpg.git
pushd Libmacgpg
make
popd

# AirmailPlugin Framework
git clone https://github.com/Airmail/AirmailPlugIn-Framework.git
open AirmailPlugin-Framework/AMPGpg/AMPGpg.xcodeproj

# In Xcode we are now looking for the framework `Libmacgpg.framework`.
# If this is included, delete it now.

# Now include the just compiled Libmacgpg.framework.
# `Build`.
```

Xcode stores the binaries created in this way under the following path: `~/Library/Developer/Xcode/DerivedData/AMPGpg-*/Build/Products/Debug/AMPGpg.bundle`.

If the folder `DerivedData` contains several projects with the name `AMPGpg-*`, you can delete these folders and rebuild the project.

Now copy the `AMPGpg.bundle` Into the Plugin folder of Airmail 3 and restart the app. After that, the plugin should be recognised and work.
