
<h1 align="center">Telegram Media Downloader</h1>

<p align="center">
<a href="https://github.com/tangyoha/telegram_media_downloader/actions"><img alt="Unittest" src="https://github.com/tangyoha/telegram_media_downloader/workflows/Unittest/badge.svg"></a>
<a href="https://codecov.io/gh/tangyoha/telegram_media_downloader"><img alt="Coverage Status" src="https://codecov.io/gh/tangyoha/telegram_media_downloader/branch/master/graph/badge.svg"></a>
<a href="https://github.com/tangyoha/telegram_media_downloader/blob/master/LICENSE"><img alt="License: MIT" src="https://black.readthedocs.io/en/stable/_static/license.svg"></a>
<a href="https://github.com/python/black"><img alt="Code style: black" src="https://img.shields.io/badge/code%20style-black-000000.svg"></a>
<img alt="Code style: black" src="https://img.shields.io/github/downloads/tangyoha/telegram_media_downloader/total">

</p>

<h3 align="center">
  <a href="./README_CN.md">中文</a><span> · </span>
  <a href="https://github.com/tangyoha/telegram_media_downloader/discussions/categories/ideas">Feature request</a>
  <span> · </span>
  <a href="https://github.com/tangyoha/telegram_media_downloader/issues">Report a bug</a>
  <span> · </span>
  Support: <a href="https://github.com/tangyoha/telegram_media_downloader/discussions">Discussions</a>
  <span> & </span>
  <a href="https://t.me/TeegramMediaDownload">Telegram Community</a>
</h3>

### Overview

Download all media files from a conversation or a channel that you are a part of from telegram.
A meta of last read/downloaded message is stored in the config file so that in such a way it won't download the same media file again.

### UI

![web](./screenshot/web_ui.gif)

After running, open the browser to visit `localhost:5000`

### Support

| Category             | Support                                          |
| -------------------- | ------------------------------------------------ |
| Language             | `Python 3.7` and above                           |
| Download media types | audio, document, photo, video, video_note, voice |

### ToDo

- Add support for multiple channels/chats.

### Installation

For *nix os distributions with `make` availability

```sh
git clone https://github.com/tangyoha/telegram_media_downloader.git
cd telegram_media_downloader
make install
```

For Windows which doesn't have `make` inbuilt

```sh
git clone https://github.com/tangyoha/telegram_media_downloader.git
cd telegram_media_downloader
pip3 install -r requirements.txt
```

## Upgrade installation

```sh
cd telegram_media_downloader
pip3 install -r requirements.txt
```

## Configuration

All the configurations are  passed to the Telegram Media Downloader via `config.yaml` file.

**Getting your API Keys:**
The very first step requires you to obtain a valid Telegram API key (API id/hash pair):

1. Visit  [https://my.telegram.org/apps](https://my.telegram.org/apps)  and log in with your Telegram Account.
2. Fill out the form to register a new Telegram application.
3. Done! The API key consists of two parts:  **api_id**  and  **api_hash**.

**Getting chat id:**

**1. Using web telegram:**

1. Open <https://web.telegram.org/?legacy=1#/im>

2. Now go to the chat/channel and you will see the URL as something like
   - `https://web.telegram.org/?legacy=1#/im?p=u853521067_2449618633394` here `853521067` is the chat id.
   - `https://web.telegram.org/?legacy=1#/im?p=@somename` here `somename` is the chat id.
   - `https://web.telegram.org/?legacy=1#/im?p=s1301254321_6925449697188775560` here take `1301254321` and add `-100` to the start of the id => `-1001301254321`.
   - `https://web.telegram.org/?legacy=1#/im?p=c1301254321_6925449697188775560` here take `1301254321` and add `-100` to the start of the id => `-1001301254321`.

**2. Using bot:**

1. Use [@username_to_id_bot](https://t.me/username_to_id_bot) to get the chat_id of
    - almost any telegram user: send username to the bot or just forward their message to the bot
    - any chat: send chat username or copy and send its joinchat link to the bot
    - public or private channel: same as chats, just copy and send to the bot
    - id of any telegram bot

### config.yaml

```yaml
api_hash: your_api_hash
api_id: your_api_id
chat_id: telegram_chat_id
last_read_message_id: 0
# note we remove ids_to_retry to data.yaml
ids_to_retry: []
media_types:
- audio
- document
- photo
- video
- voice
file_formats:
  audio:
  - all
  document:
  - pdf
  - epub
  video:
  - mp4
save_path: D:\telegram_media_downloader
file_path_prefix:
- chat_title
- media_datetime
disable_syslog:
- INFO
upload_drive:
  # required
  enable_upload_file: true
  # required
  remote_dir: drive:/telegram
  # required
  upload_adapter: rclone
  # option,when config upload_adapter rclone then this config are required
  rclone_path: D:\rclone\rclone.exe
  # option
  before_upload_file_zip: True
  # option
  after_upload_file_delete: True
hide_file_name: true
file_name_prefix:
- message_id
- file_name
file_name_prefix_split: ' - '
max_concurrent_transmissions: 1
web_host: 127.0.0.1
web_port: 5000
```

- **api_hash**  - The api_hash you got from telegram apps
- **api_id** - The api_id you got from telegram apps
- **chat_id** -  The id of the chat/channel you want to download media. Which you get from the above-mentioned steps.
- **last_read_message_id** - If it is the first time you are going to read the channel let it be `0` or if you have already used this script to download media it will have some numbers which are auto-updated after the scripts successful execution. Don't change it.
- **ids_to_retry** - `Leave it as it is.` This is used by the downloader script to keep track of all skipped downloads so that it can be downloaded during the next execution of the script.
- **media_types** - Type of media to download, you can update which type of media you want to download it can be one or any of the available types.
- **file_formats** - File types to download for supported media types which are `audio`, `document` and `video`. Default format is `all`, downloads all files.
- **save_path** - The root directory where you want to store downloaded files.
- **file_path_prefix** - Store file subfolders, the order of the list is not fixed, can be randomly combined.
  - `chat_title`      - channel or group title, it will be chat id if not exist title.
  - `media_datetime`  - media date, also see pyrogram.types.Message.date.strftime("%Y_%m").
  - `media_type`      - media type, also see `media_types`.
- **disable_syslog** - You can choose which types of logs to disable,see `logging._nameToLevel`.
- **upload_drive** - You can upload file to cloud drive.
  - `enable_upload_file` - Enable upload file, default `false`.
  - `remote_dir` - Where you upload, like `drive_id/drive_name`.
  - `upload_adapter` - Upload file adapter, which can be `rclone`, `aligo`. If it is `rclone`, it supports all `rclone` servers that support uploading. If it is `aligo`, it supports uploading `Ali cloud disk`.
  - `rclone_path` - RClone exe path, see wiki[how to use rclone](https://github.com/tangyoha/telegram_media_downloader/wiki#how-to-use-rclone)
  - `before_upload_file_zip` - Zip file before upload, default `false`.
  - `after_upload_file_delete` - Delete file after upload success, default `false`.
- **file_name_prefix** - custom file name, use the same as **file_path_prefix**
  - `message_id` - message id
  - `file_name` - file name (may be empty)
  - `caption` - the title of the message (may be empty)
- **file_name_prefix_split** - custom file name prefix symbol, the default is `-`
- **max_concurrent_transmissions** - Set the maximum amount of concurrent transmissions (uploads & downloads). A value that is too high may result in network related issues. Defaults to 1.
- **hide_file_name** - whether to hide the web interface file name, default `false`
- **web_host** - web host
- **web_port** - web port

## Execution

```sh
python3 media_downloader.py
```

All downloaded media will be stored at the root of `save_path`.
The specific location reference is as follows:

The complete directory of video download is: `save_path`/`chat_title`/`media_datetime`/`media_type`.
The order of the list is not fixed and can be randomly combined.
If the configuration is empty, all files are saved under `save_path`.

## Proxy

`socks4, socks5, http` proxies are supported in this project currently. To use it, add the following to the bottom of your `config.yaml` file

```yaml
proxy:
  scheme: socks5
  hostname: 127.0.0.1
  port: 1234
  username: your_username(delete the line if none)
  password: your_password(delete the line if none)
```

If your proxy doesn’t require authorization you can omit username and password. Then the proxy will automatically be enabled.

## Contributing

### Contributing Guidelines

Read through our [contributing guidelines](https://github.com/tangyoha/telegram_media_downloader/blob/master/CONTRIBUTING.md) to learn about our submission process, coding rules and more.

### Want to Help?

Want to file a bug, contribute some code, or improve documentation? Excellent! Read up on our guidelines for [contributing](https://github.com/tangyoha/telegram_media_downloader/blob/master/CONTRIBUTING.md).

### Code of Conduct

Help us keep Telegram Media Downloader open and inclusive. Please read and follow our [Code of Conduct](https://github.com/tangyoha/telegram_media_downloader/blob/master/CODE_OF_CONDUCT.md).
