# ndb_master_creation
日本の診療報酬、薬価改定に対応したマスター作成プロジェクト  
Master creation with dictionaly style supporting revisions over time in Japan.  

本研究は、日本の医療経済研究において、ある時点での薬価、診療報酬が利用しづらいという問題を解決するために実施されました。  
This research was conducted to solve the problem that it is difficult to use drug prices and medical fees at a certain point in medical economics research in Japan.  

支払基金よりマスターを再帰的にダウンロードし、日にちに応じた薬価、診療報酬を与える辞書型の構造として再構成しています。  
I recursively downloaded the master from the payment fund. We are rebuilding it as a dictionary-type structure to obtain drug prices and medical fees according to the date.  

なお、マスターの改定が反映されるのは翌月2日、というルールに従って再構成しています。  
In addition, it is reconfigured according to the rule that the revision of the master will be reflected on the 2nd of the following month.  


## Step.1
Recurrative download from kikin.    
Download is done to D\:kikin/  
If needed, Please change the dl path.

Details in wget option is bellow.  
https://4thsight.xyz/427  

```
wget -rkp -np -linf -P /mnt/d/kikin/ https://www.ssk.or.jp/seikyushiharai/tensuhyo/index.html > ../log.txt 2>&1 
echo $?":exit status" >> ../log.txt
```


## Step.2

Creation of a list of file and metadata.  
Time stump and checksum were added

## Step.3

IY master creation.

drug code -> date (string, such as "20230405") -> drugcost (point)
master requirement:

This master should give you the yakka that the drug the day.
1. When the drug code appears in the time before first degital iy master, this should return the old value defined in the master with the "bdm", before degital master tag.
1. When the drug code appears in past from drug being available, this should return the first value defined in the future with the "bml", before market launch tag.
2. When the drug code appears in sale, this should return the value define in master, with the "ws", within sale tag.
2. When the drug code appears in the future from the drug being unavailable, this should return the last value defined the past with "aes",after end of sale tag.

This master should give you several additional info  
"nplcode" :NHI Price Listed Drug Code, 薬価基準収載医薬品コード  
"drugtype":  
"is_generic":  

Details in 
master_1_20220930.pdf
from kikin's HP.


## Step.4

SI master creation.

The price of si code is mainly described by point, not yen.  
So the master created is have code of code type.  
Users have to check the code type to use the price.


## Step.5

TO master creation.

The price of to code is mainly described by yen.  
But the master created is have code of code type.  
Users have to check the code type to use the price.

## Step.6

Custom dictionary with range info.

Reading whole masters requres you large memory.
It may be good to have range info in master to compress master size.
One of example of usage is writed.

## Step.7

Custom dictionary with range info.

Ver Julia.

## output memo


Process note books in note directory.  
Products are in output directory.  

## usage

git clone https://github.com/fk506cni/ndb_master_creation



## Process environment
Ubuntu 22.04LTS(WSL/Windows 10 home.)


## Caution.
### License about our codes: GPLv3
我々のコードのライセンスはGPLv3としています。  

ただし、オリジナルのマスターファイルは支払基金の管理下にあり、本プロジェクトのラインセンスの範疇にはありません。  
本研究でオリジナルのマスターを二次利用して作成したoutput_iy, output_si, output_to内のファイル群については自己責任の元、非営利目的に限りご自由に利用いただけますが、商用利用および再配布は禁止いたします。  

処理内容の修正等が必要であればissueに投稿ください。  
あるいは改めて基金HPよりダウンロードし、ご自身で処理を実行してください。  


Our code is licensed under GPLv3.

However, the original master file is under the control of the payment fund and is not covered by the license for this project.
You can freely use the output files created by secondary use of the original master for non-commercial purposes at your own risk, but commercial use and redistribution are prohibited.
If you need to modify the processing content, please download it again from the foundation website and execute the processing yourself.

