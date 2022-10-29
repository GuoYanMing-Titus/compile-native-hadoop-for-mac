# compile-native-hadoop-for-mac
# hadoop-forarm64
compile-native-hadoop

Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-50-generic aarch64)

# 編譯環境與依賴套件準備
~~~
sudo apt update
~~~
~~~
sudo apt purge openjdk*
~~~
~~~
sudo apt -y install openjdk-8-jdk
~~~
~~~
sudo apt -y install maven
~~~
~~~
sudo apt -y install build-essential autoconf automake libtool cmake zlib1g-dev pkg-config libssl-dev libsasl2-dev
~~~
~~~
cd ~
~~~
~~~
wget https://github.com/protocolbuffers/protobuf/releases/download/v3.7.1/protobuf-java-3.7.1.tar.gz
~~~
~~~
tar -zxvf protobuf-java-3.7.1.tar.gz
~~~
~~~
cd protobuf-3.7.1/
~~~
~~~
sudo ./configure
~~~
~~~
sudo make -j$(nproc)
~~~
~~~
sudo make install
~~~
~~~
sudo apt -y install snapd
sudo apt -y install libsnappy-dev
sudo apt -y install zstd
sudo apt -y install bzip2
sudo apt -y install libbz2-dev
sudo apt -y install fuse libfuse-dev
sudo apt -y install libzstd-dev
~~~
~~~
cd ~
~~~
~~~
git clone https://github.com/intel/isa-l.git
cd isa-l/
~~~
~~~
sudo ./autogen.sh
~~~
~~~
sudo ./configure
~~~
~~~
sudo make
~~~
~~~
sudo make install
~~~

# 安裝hadoop支援的openssl版本
~~~
cd ~
~~~
~~~
wget http://www.openssl.org/source/openssl-1.1.1q.tar.gz
~~~
~~~
tar -zxf openssl-1.1.1q.tar.gz 
~~~
~~~
cd openssl-1.1.1q/
~~~
~~~
sudo ./config
~~~
~~~
sudo make
~~~
將原本的openssl備份到/tmp
~~~
sudo mv /usr/bin/openssl ~/tmp
~~~
~~~
sudo make install
~~~
將安裝的openssl link到 /usr/bin/openssl
~~~
sudo ln -s /usr/local/bin/openssl /usr/bin/openssl
~~~
~~~
sudo ldconfig
~~~
確認版本
~~~
openssl version
~~~


# 設定環境變數
~~~
sudo vim /etc/profile
~~~
加入
~~~
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-arm64
export LD_LIBRARY_PATH=/usr/local/lib
PATH=$PATH:/usr/lib/jvm/java-8-openjdk-arm64/bin
~~~
重置環境變數
~~~
source /etc/profile
~~~
確認環境變數
~~~
echo $JAVA_HOME
echo $LD_LIBRARY_PATH
~~~


# 下載hadoop source file
~~~
cd ~
~~~
~~~
wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.4/hadoop-3.3.4-src.tar.gz
~~~
~~~
tar zxvf hadoop-3.3.4-src.tar.gz
~~~
~~~
cd hadoop-3.3.4-src/
~~~

# 選擇編譯指令, 開始編譯
~~~
mvn package -Pdist,native -DskipTests -Dtar
~~~
~~~
mvn clean package -Pdist,native -DskipTests -Dtar
~~~
~~~
mvn clean package -Pdist,native -DskipTests -Dtar -Drequire.openssl
~~~

# 編譯完成後檔案路徑在 hadoop-dist
~/hadoop-3.3.4-src/hadoop-dist/target/hadoop-3.3.4.tar.gz
~~~
cd ~/hadoop-3.3.4-src/hadoop-dist/target/hadoop-3.3.4/bin
~~~
查看版本
~~~
./hadoop version
~~~
<img width="1894" alt="Screen Shot 2022-10-11 at 7 43 33 PM" src="https://user-images.githubusercontent.com/29039153/195081101-24752646-7d83-4ba6-95a2-985e51044746.png">
測試 checknative
~~~
./hadoop checknative
~~~
<img width="1769" alt="Screen Shot 2022-10-11 at 7 44 00 PM" src="https://user-images.githubusercontent.com/29039153/195081129-ed47a105-b887-405b-9876-3c3c40bb31c4.png">
