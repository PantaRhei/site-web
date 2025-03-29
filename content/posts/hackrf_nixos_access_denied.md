+++
title = 'Hackrf one Nixos access denied'
date = 2023-11-21T17:05:10+01:00
draft = false
+++

If you use a hackrf one on nixos, you may be have this message:

```
[segfault45@nixos:~]$ hackrf_info
hackrf_info version: 2024.02.1
libhackrf version: 2024.02.1 (0.9)
Found HackRF
Index: 0
hackrf_open() failed: Access denied (insufficient permissions) (-1000)
```

For solve this problem you need add user to
`plugdev` group, active hackrf hardware and
install pakage.

You have config exemple:
```nix
users.users.your_name = {
  isNormalUser = true;
  description = "your_name";
  extraGroups = [ "networkmanager" "wheel"  "plugdev"];#adding user to plugdev group
  packages = with pkgs; [
    #thunderbird
  ];
};

...

#Active hardware
hardware.hackrf.enable = true;
#install packages
environment.systemPackages = with pkgs; [
    hackrf
  ];
```

After do `nixos-rebuild switch`, you need reboot your computer.

And, it's work:
```
[segfault45@nixos:~]$ hackrf_info
hackrf_info version: 0000.13.32
libhackrf version: 0000.13.32 (0.9)
Found HackRF
Index: 0
Serial number: **
Board ID Number: 4 (HackRF One)
Firmware Version: 0000.13.32 (API:1.07)
Part ID Number: **
Hardware Revision: r9
Hardware appears to have been manufactured by Great Scott Gadgets.
Hardware supported by installed firmware:
    HackRF One
```

![meme](/images/meme1.jpg)
