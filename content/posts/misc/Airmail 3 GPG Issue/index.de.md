---
title: Airmail 3 GPG Issue
date: 2019-03-19T10:47:00+01:00
author: Djordje Atlialp
draft: false
toc: false
cover: d7e60867901a4500b0eb7362de1a525e.png
tags:
---

[Airmail 3](https://airmailapp.com/) ist ein alternativer E-Mail Client für macOS. Das GPG Plugin (zu finden [hier](https://help.airmailapp.com/en-us/article/plugins-airmail-for-macos-lenvq3/)) hat jedoch seit einiger Zeit große Schwierigkeiten, von Airmail 3 erkannt zu werden. Auf GitHub gibt es auch bereits eine längere Diskussion dazu: https://github.com/Airmail/AirmailPlugIn-Framework/issues/34.

Ein [Kommentar](https://github.com/Airmail/AirmailPlugIn-Framework/issues/34#issuecomment-442918633) von [@y3sh](https://github.com/y3sh) war da besonders hilfreich, da dieser beschreibt, wie das Problem zumindest lokal zu lösen ist.

```bash
# Libmacgpg
git clone --recursive https://github.com/GPGTools/Libmacgpg.git
pushd Libmacgpg
make
popd

# AirmailPlugin Framework
git clone https://github.com/Airmail/AirmailPlugIn-Framework.git
open AirmailPlugin-Framework/AMPGpg/AMPGpg.xcodeproj

# In Xcode suchen wir jetzt nach dem Framework `Libmacgpg.framework`.
# Sollte dieses eingebunden sein, jetzt löschen.

# Im Anschluss jetzt das eben kompilierte Libmacgpg.framework einbinden.
# `Build`.
```

Die so erstellten Binaries legt Xcode unter dem folgenden Pfad ab: `~/Library/Developer/Xcode/DerivedData/AMPGpg-{viele Buchstaben}/Build/Products/Debug/AMPGpg.bundle`.

Sollten im Ordner `DerivedData` mehrere Projekte mit dem Namen `AMPGpg-*` liegen, kann man diese Ordner beruhigt löschen und dann das Projekt neu bauen.

Nun kopiert man noch das `AMPGpg.bundle` in den Plugin Ordner von Airmail 3 und startet die App neu. Danach sollte das Plugin erkannt werden und auch funktionieren.
