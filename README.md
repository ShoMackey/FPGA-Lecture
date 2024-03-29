# FPGA-Lecture

## FPGAセットアップ

### ソフトウェア一覧
・Vitis_hls  
・Vivado  
・http://www.pynq.io/board.html にてUltra96v2のv2.7のOS  
・balena etcher https://www.balena.io/etcher/  
・Tera Tarm https://ttssh2.osdn.jp/  
### ハードウェア一覧
・Ultra96v2 FPGAボード  
・Windows PC or Ubuntu PC  
・micro SDカード16G以上  
・micro USBケーブル  
・Ultra96v2用ACアダプタ  
・LAN to USB Type A 変換ケーブル (あれば)  
###  セットアップ手順
・balena etcherでFPGA用osをSDカードにフォーマット  
・micro USB, LAN, ACアダプタをFPGAに接続  
・FPGAのsw4を押す  
・Tera Term 等でシリアル通信開始  
・ifconfigで確認したip addressにアクセス  

## 大まかな流れ
Step1: vitis_hlsにてcppでコード記述、ip化  
Step2: vivadoにてStep1で作成したipとプロセッサとの接続, bitstream化  
Step3: Step2にて作成した.bit, .hwhファイルをFPGAに転送, pynq os上のpythonでip操作  
### Step1 (Vitis HLSにて作業)
・cppにて高速化したい関数、その関数の正しさを証明するためのテストベンチを作成  
・C Simulation  
・C Synthesis  
・C/RTL Simulation (可能であれば)  
・Export RTL  
### Step2 (Vivadoにて作業)
・Step1にて作成したIPをV読み込む  
・Create Block Design  
・プロセッサ(zynq Ultrascale+)と以前に読み込んだIPをロード  
・プロセッサ側のmasterとslaveピンの数を調整  
・プロセッサと自作IPの配線接続  
・Validate Design  
・Create HDL Wrapper  
・Generate Bitstream  
### Step3  (FPGAボード上のpynq osでの作業)
・Step2にてgenerate bitstreamで作成した、"デザイン名+wrapper.bit"と"デザイン名.hwh"ファイルをFPGAに転送  
・.bitファイルの拡張子前の名前は、.hwhと揃えておく  
・pynqライブラリを使用し、overlayとして読み込む  
・配列データやctrl信号をipに送り込み、実行する  

## レクチャー内容
### Lチカ
・https://valinux.hatenablog.com/entry/20211125 参照  
### 行列積(matmul)の高速化
#### とりあえず動かす
・cppによる行列積関数の作成 (Vitis HLS)  
・配線用のセッティングhls interfaceの挿入  
・Step1まで実行  
・Step2を実行  
・Step3を実行、ip_check_on_pynq.pyを参照  
#### 最適化をしてみる１
・先ほど作った行列積関数にpragma(unroll, pipeline, array_partition等)を追加して、ベースラインからの高速化の度合いを確認  
・pragmaの詳細はhttps://japan.xilinx.com/htmldocs/xilinx2019_1/sdaccel_doc/hls-pragmas-okr1504034364623.html 参照  
・配列は2ポートのBRAMに固定  
#### 最適化をしてみる２ 
・先ほどのセクションでの問題点を考えてみる、リソースとレイテンシの関係など  
・行列積の処理フロー変更による高速化を試す  
### CNNの実装
#### CNNのハードウェア実装流れ
・pythonにてcnnモデル構築、学習  
・学習後の重み、モデルからの入力、出力データの抽出 (cppで使えるように)  
・cppにてcnn関数、テストベンチの実装    
・Step2, Step3の実装  
#### 例
・https://www.acri.c.titech.ac.jp/wordpress/archives/5181 参照  
・https://qiita.com/HirokiNakahara/items/4e6bad539fe9cce340c4 参照 



