# ATAC-seq practice

## Linux环境及软件安装

使用conda创建ATAC-seq分析环境，并在这个环境下安装分析软件

```shell
#创建环境
conda  create -n atac -y   python=2 bwa
#change enviroments
source activate atac
#查看环境是否切换成功
conda info --envs #该命令列出已有的环境，*指示的为当前环境
#批量安装软件
conda install -y sra-tools  
conda install -y trim-galore  samtools bedtools
conda install -y deeptools homer  meme
conda install -y macs2 bowtie bowtie2 
conda install -y  multiqc 
conda install -y  sambamba
#若中间出现下载不完全，则继续执行原脚本，conda会自动监测已经安装成功的程序，然后再次安装未成功的程序

#备份环境以便移植
conda env export > environment.yml
#将以上文件放在工作目录下，可以用以下命令创建环境
conda env create -f environment.yml
source activate atac #需要记住环境名字，再次激活。若在相同机器的同一账号下，直接执行source即可调查环境
```



## 组织项目目录

```shell
mkdir -p /data1/yaoyh/ATAC_seq_learn
cd /data1/yaoyh/ATAC_seq_learn

mkdir {sra,raw,clean,align,peaks,motif,qc}
```

## 下载SRA数据

* **从NCBI网站下载SRA accession no.的列表文件**

  ![](http://ww1.sinaimg.cn/large/005SiqoKly1g4ul5vttkdj312y0h4q3t.jpg)

  * **`prefetch`批量下载**

```shell
cd /data1/yaoyh/ATAC_seq_learn/sra
#每次重新登陆都需要配置环境
source activate atac

cat srr.list |while read id;do ( nohup  prefetch $id & );done
#该命令默认将sra文件下载到/home/yaoyh/ncbi/public/sra/
```

