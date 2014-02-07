---
layout: post
title: Mac Book Air 設定手順
description: ""
category: "mac"
tags: [mac設定,brew,vim]
---
{% include JB/setup %}
Mac miniのサブマシンにMac Book Air買ってしまいました。  
年末の繁忙期のためか１日遅れで到着。  
以下、正月休みに行った設定メモ

##環境

- 11インチ：128GBフラッシュストレージ  
- 1.3GHzデュアルコアIntel Core i5 プロセッサ  
（Turbo Boost使用時最大2.6GHz）  
- Intel HD Graphics 5000  
- 8GBメモリ  
- OsX 10.9.1(Marverics)
- USキーボード

##OSXアップデート

アップデートのリストを確認  

	% sudo softwareupdate -l

アップデートをすべて適用  

	% sudo softwareupdate -ia

特定のアップデートだけを適用  

	% sudo softwareupdate -i PackageName

自動更新On  

	% softwareupdate --schedule on


再起動  

	% sudo reboot

##ログインShellをzshへ変更

	% cat /etc/shells
		/bin/bash
		/bin/csh
		/bin/ksh
		/bin/sh
		/bin/tcsh
		/bin/zsh
	% chsh -s /bin/zsh


##アプリインストール

###App Store購入済みアプリ

App Storeから手動でインストール

* [Simplenote](http://simplenote.com/)
* [Alfred](http://www.alfredapp.com/)
* [Marked](http://markedapp.com/)
* [Desktop Calendar Plus](http://3fl.jp/dcp/?lang=ja)
* [LINE](http://line.me/ja/)

###その他インストールしたもの
* [Google Chrome](http://www.google.co.jp/intl/ja/chrome/browser/)
* [Google 日本語入力](http://www.google.co.jp/ime/)
* [iTerm2](http://www.iterm2.com/#/section/home)
* [Dropbox](https://www.dropbox.com/)
* [KeyRemap4MacBook](https://pqrs.org/macosx/keyremap4macbook/index.html.ja)
	- [USキーボードで「英数」「かな」キーを使う方法](http://ameblo.jp/maverick-5/entry-11343030071.html)
	- [Vim以外でVimする | Vim Emuration](http://rcmdnk.github.io/blog/2013/06/10/computer-mac-keyremap4macbook-vim/)
* [BetterTouchTool](http://bettertouchtool.en.softonic.com/mac)
	- [超便利！「BetterTouchTool」を使いたおすための7つの設定。[Mac] | MacWin Ver.1.0](http://macwin.org/mac/bettertouchtool/)

##Dropboxで共有している.filesにシンボリックリンクを貼る。

	% ln -s ~/Dropbox/dotfiles/.zshrc ~/.zshrc
	% ln -s ~/Dropbox/dotfiles/.vimrc ~/.vimrc
	
##HomeBrewインストール

* [Xcode](https://developer.apple.com/jp/technologies/tools/)


	1. [App Store](https://itunes.apple.com/jp/app/xcode/id497799835?mt=12)からダウンロードしてインストール

	2. Command Line Toolsのインストール
		* [MavericksでCommand Line Tools for Xcodeをインストールする - Qiita [キータ]](http://qiita.com/3yatsu/items/47470091277d46f3fde2)

* [Homebrew](http://brew.sh/)


	インストール(公式HPの通り)

		% ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"

	エラーがないかチェック
	
		% brew doctor
	
	Warningが発生

		Warning: Unbrewed dylibs were found in /usr/local/lib.
		If you didn't put them there on purpose they could cause problems when
		building Homebrew formulae, and may need to be deleted.
		Unexpected dylibs:
		    /usr/local/lib/libusb-1.0.0.dylib

	消せと言われたけど、とりあえずゴミ箱へ移動してbrewでインストールする。

		% ls /usr/local/lib/libusb*
			/usr/local/lib/libusb-1.0.0.dylib* /usr/local/lib/libusb-1.0.dylib@
		% ls -l /usr/local/lib/libusb-1.0.dylib
				lrwxr-xr-x  1 root  admin  33 12 30 17:20 /usr/local/lib/libusb-1.0.dylib@ -> /usr/local/lib/libusb-1.0.0.dylib
		% mv /usr/local/lib/libusb* .Trash/


	再確認

		% brew doctor
			Your system is ready to brew.

	brewでlibusbを再インストール

		% brew install libusb
			==> Downloading http://downloads.sourceforge.net/project/libusb/libusb-1.0/libus
			#################################################################### 100.0%
			==> ./configure --prefix=/usr/local/Cellar/libusb/1.0.9
			==> make install
			🍺  /usr/local/Cellar/libusb/1.0.9: 11 files, 416K, built in 12 seconds

		% ls -l /usr/local/lib/libusb*
			lrwxr-xr-x  1 xxxx  admin  45 12 30 18:39 /usr/local/lib/libusb-1.0.0.dylib@ -> ../Cellar/libusb/1.0.9/lib/libusb-1.0.0.dylib
			lrwxr-xr-x  1 xxxx  admin  39 12 30 18:39 /usr/local/lib/libusb-1.0.a@ -> ../Cellar/libusb/1.0.9/lib/libusb-1.0.a
			lrwxr-xr-x  1 xxxx  admin  43 12 30 18:39 /usr/local/lib/libusb-1.0.dylib@ -> ../Cellar/libusb/1.0.9/lib/libusb-1.0.dylib

	バージョンが`1.0.0`から`1.0.9`になった。

	本体を最新に更新  
	
		% brew update

	brewからインストール

		% brew install w3m
		% brew install tree

##Gnuコマンドを利用できるように変更

	% brew install coreutils --default-names

問題が起きた場合は一旦切る

	% brew unlink coreutils

元に戻す

	% brew link coreutils

その他

	% brew install findutils --default-names
	% brew install binutls --default-names

##[zsh-completions](https://github.com/zsh-users/zsh-completions)

	% brew install zsh-completions

.zshrcへパスを追加

	fpath=(/usr/local/share/zsh-completions $fpath)
	autoload -U compinit
	compinit -u

zcompdumpをリビルド

	% rm -f ~/.zcompdump; compinit

##[zsh-syntax-highlight](https://github.com/zsh-users/zsh-syntax-highlighting)

	% mkdir ~/.zsh
	% git clone git://github.com/zsh-users/zsh-syntax-highlighting.git ~/.zsh/zsh-syntax-highlighting

.zshrcへ記載

	source ~/.zsh/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

##Vim用設定
###NeoBundleのインストール

	% mkdir -p ~/.vim/bundle
	% git clone git://github.com/Shougo/neobundle.vim ~/.vim/bundle/neobundle.vim

Vimから`:NeoBundleInstall`

##その他

* [font - プログラミング用フォントをMacに homebrew でインストール - Qiita [キータ]](http://qiita.com/hshimo/items/02b9882162b6f07cd85f)
* [Vim - powerlineをいつ使う？今でしょ！ - Qiita [キータ]](http://qiita.com/alpaca_taichou/items/ab70f914a6a577e25d70)
* [Mac の Vim (CUI) でクリップボードを有効化する - Qiita [キータ]](http://qiita.com/b4b4r07/items/6f0ac4c5ae3edc10ce3a)
* [MacOSX - vim で lua を有効にする - Qiita](http://qiita.com/zakkied/items/c46364c00e9fce7b55c7)
