# [試して理解] Linuxの仕組み ～実験と図解で学ぶOSとハードウェアの基礎知識
武内 覚. 技術評論社, 2018.03.08

Finish reading at:

###### Purpose
To learn the basics and fundamentals of Linus OS in order to introduce Arch Linux to my VAIO laptop.

## Notes
https://github.com/satoru-takeuchi/linux-in-practice/

### 1章
- ユーザーモード: プロセス
- カーネルモード: デバイスドライバ->デバイス，プロセス管理システム，プロセススケジューラ，メモリ管理システム

カーネルモードで動作する(OSの核となる)処理をまとめたプログラム -> カーネル

プロセスはシステムコールを介してカーネルにカーネルの機能を依頼
