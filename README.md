# sys-screen-capture-uploader

`sys-screen-capture-uploader` automatically sends screen captures (both images and videos) taken on your Nintendo Switch to a Telegram chat so that you can more easily access and share them.

This project was forked from [bakatrouble/sys-screenuploader](https://github.com/bakatrouble/sys-screenuploader). It differs from the original project because no intermediate server is needed. The screen captures are directly uploaded to the Telegram API. However, creating a Telegram bot is necessary (see [Setup](#setup) below).

## Requirements

You need a Nintendo Switch running [Atmosphere](https://github.com/Atmosphere-NX/Atmosphere), a custom firmware which will allow you to run homebrew. Check [this guide](https://switch.homebrew.guide/index.html) if you don't know how to hack your Switch.

## Setup

To use this homebrew, you must create your own Telegram bot. It will send your Switch screen captures to you (and only to you) on a private chat.

1. [Create a Telegram bot](https://core.telegram.org/bots#3-how-do-i-create-a-bot). Choose any name and username of your preference. Note down its authorization token.
2. Send any message to your bot from the Telegram user that should receive the screenshots.
3. Open `https://api.telegram.org/bot<your-bot-token>/getUpdates` on your browser. You should see a single result with your message. Note down the chat ID (`.result[0].message.chat.id` in [`jq`](https://stedolan.github.io/jq/) filter notation).

After getting these two values, you are ready to configure the homebrew and install it to your Switch:

1. Download [the latest release](https://github.com/musse/sys-screen-capture-uploader/releases/latest) and extract it somewhere.
2. Open `config/sys-screen-capture-uploader/config.ini` and set `telegram_bot_token` and `telegram_chat_id` to the values you have noted down.
3. Copy the release contents to the root of your SD card.

## Development

### Dependencies

You can install [`devkitpro-pacman`](https://github.com/devkitPro/pacman/releases/latest) and then install the needed dependencies with:

```
sudo dkp-pacman -Syu
sudo dkp-pacman -S devkitA64 switch-dev switch-curl switch-zlib switch-mbedtls
```

Check the following links for additional details on how to set up your environment for Switch homebrew development:

- [Homebrew app development](https://switch.homebrew.guide/homebrew_dev/app_dev.html)
- [Setting up Development Environment](https://switchbrew.org/wiki/Setting_up_Development_Environment)

### Build

To build the project, you also need [`CMake`](https://cmake.org/). Create a `build` directory on the project root, `cd` into it and run:

```
cmake .. -DCMAKE_TOOLCHAIN_FILE=../devkita64-libnx.cmake
```

You should now have a `Makefile` inside the `build` directory and can simply run `make` to build the project. The relevant binary file is `sys-screen-capture-uploader.nsp`.

After building the project, you can generate a release by running `scripts/release.sh` from the repository root. This will create the correct directory structure that should be copied to the root of your SD card and also a zip file containing all these files.

If you have an FTP server running on your Switch (such as [sys-ftpd-light](https://github.com/cathery/sys-ftpd-light)), you can run `scripts/upload.sh` from the repository root to update the binary over the network so that development is faster. Make sure to set the `SWITCH_HOSTNAME` env var before running this script.

## Credits

- [bakatrouble/sys-screenuploader](https://github.com/bakatrouble/sys-screenuploader): project from which this project was forked;
- [vbe0201/libnx-template](https://github.com/vbe0201/libnx-template/): homebrew template project;
- [SunTheCourier/SwitchPresence-Rewritten](https://github.com/SunTheCourier/SwitchPresence-Rewritten): initial sysmodule code;
- [lordelan](https://github.com/lordelan): icon.
