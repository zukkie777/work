## Name
　NMAP-SUPPORT-TOOL

## Overview
　これはNMAP利用時の支援ツールであり、その主なスクリプトとその概要は二つである。

　1.nmap.sh   
　　1)LAN内のNWアドレス自体の探索   
  NMAP実行サーバ自身の全IPアドレスから、NWアドレスの計算(ipcalc)   
　　2)1)結果に対しディスカバリ(nmap)を実行し、XMLファイルへ出力    
　2)MAC-ADDRESS-Renewal.sh   
　　1)IEEEのウェブサイトからMACアドレス一覧(oui.txt)の取得(curl)   
　　2)1)結果に対し、NMAPフォーマットへ変換後(make-mac-prefixes.pl)に更新   
　　　*make-mac-prefixes.plはnmapインストール時にnmapディレクトリ設置される   

## Description
　1.nmap.sh   
   1)nmap.sh実行開始時にsar.shがcallされ、リソース情報の収集を開始する。   
　 2)実行サーバ自体の全IPアドレスから、全NWアドレスを計算する。   
　　 複数インタフェイスを持つ場合は、全て計算する。   
　 3)2)実行結果をもとに全てのNWアドレスに対し、ディスカバリ(nmap)を実行し、   
　　 結果をXMLフォーマットに出力する。   
　 4)1)実行開始直後にcallされたsar.shのリソース情報へnmapによるディスカバリ開始・終了時刻を   
　　 マッピングする(sar-result_and_nmap-log.sh)   
2)MAC-ADDRESS-Renewal.sh   
  1)IEEEのWebサイトからMACアドレス一覧(oui.txt)の取得(curl)する。   
　　*IEEEのURLは適宜変更されることがあり、また応答ステータスコードが200以外の場合は、   
　　 正常取得不可となることがわかっているため、その場合は以降の全処理を停止させることとしている。   
　2)現在のnmapの持つMACアドレス一覧ファイル(nmap_mac_prefixes)のバックアップを取得する。   
　3)1)の実行結果(oui.txt)をnmapフォーマット(nmap_mac_prefixes)に変換する。   
　　*nmapインストール時に配置される：make-mac-prefixes.plを用いて実行する。   
  4)2)のバックアップと3)の上書いた実行結果の差分を取得して全処理終了   
　　*次回のnmap.sh実行時から、更新されたMACアドレスにて処理が実施される。   

## Demo

## VS. 

## Requirement

## Usage
　1.nmap.sh	  
　  # bash -x nmap.sh    

　　若しくはcrontabに任意の実行ユーザ、任意の曜日時刻に定期実行するよう記述する。   
   01 14 * * * /usr/local/etc/nmap-tool/nmap.sh   
  2.MAC-ADDRESS-Renewal.sh    
   # bash -x MAC-ADDRESS-Renewal.sh   
　　若しくはcrontabに任意の実行ユーザ、任意の曜日時刻に定期実行するよう記述する。   
   01 14 * * * /usr/local/etc/nmap-tool/MAC-ADDRESS-Renewal.sh   

## Install   
  # cd /usr/local/etc/   
  # tar xvzf nmap-tool.tgz   
    *必要な全ディレクトリ、全ファイルが展開される。   
  # vi auth.txt    
    sodoers上でnmapを許可したユーザのパスワードを記述する。   
    もしくはnmap.shのスクリプトをrootで実行するようにcatからsudo -Sまでの記述を削除する。   
　　cat auth.txt | sudo -S nmap -T4 -A -v -iL $NWADDR -oX /tmp/`date "+%Y%m%d%H%M%S"`.xml   
　　nmap -T4 -A -v -iL $NWADDR -oX /tmp/`date "+%Y%m%d%H%M%S"`.xml;   


## Contribution
  1. Fork it ( https://github.com/zukkie777/tool_for_nmap )   
  2. Create your feature branch (git checkout -b my-new-feature)   
  3. Commit your changes (git commit -am 'Add some feature’)   
  4. Push to the branch (git push origin my-new-feature)   
  5. Create new Pull Request   

## Licence 


## Author
https://github.com/zukkie777   
