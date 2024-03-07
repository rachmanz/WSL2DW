---
Dates: 28/02/2024
Author: Abdurrahman Al-atsary
---

# **Instalasi Modul 1 ABD (Apache Hadoop)**

1. Pertama, pastikan ada WSL didalam windows kalian.

- Jika tidak ada bisa dilakukan dengan :
  1. Buka Command Line (Administrator) : Ketikan `wsl --install`
  2. Ditunggu untuk penyelesaiannya lalu bisa buka **Microsoft Store**, cari **Ubuntu Versi 22.04** kemudian install.
  3. Tunggu hingga selesai semua instalasi.
  4. Lalu, Restart Laptop Anda.

2. Kedua, buka Ubuntu-nya yang ada didalam windows

3. Tunggu sebentar, lalu setup untuk username dan passwordnya.

**Notes: Username dan Password harap dihafal karna sebagai kunci kita untuk setup nanti.**

4. Lalu installasi java dengan mengetikan pada command line : `sudo apt install openjdk-8-jdk --headless`

5. Kemudian ketik `java -version` untuk pengecekan apakah versi java yang terinstall yang sudah benar di versi 8.

6. Lalu buka directory dengan mengetikan : `cd /usr/lib/jvm` dan copas bagian folder javanya _**(Letakan didalam Notepad)**_.

7. Kemudian ketik dalam terminal kembali `cd` untuk balik ke homenya.

8. Kemudian,ketik `sudo nano .bashrc` untuk mengedit file .bashrc (Environment dari Linux)

9. Lalu edit dengan ke arah paling bawah dari semua code dan copas code berikut :

```sh
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin
export HADOOP_HOME=~/hadoop-3.3.6/
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.6.jar
export HADOOP_LOG_DIR=$HADOOP_HOME/logs
export PDSH_RCMD_TYPE=ssh
```

10. Lalu, ketikan didalam shell kembali : `source .bashrc`

11. Selanjutnya, menginstall ssh dengan tujuan keamanan dalam transaksi pertukaran data yang terjadi dalam jaringan.

- Ketik didalam shell : `sudo apt-get install ssh`

12. Selanjutnya dapat melakukaan penginstallan pada hadoop versi: 3.3.6 pada link berikut : [Download disini!](https://hadoop.apache.org/releases.html)

![File Downloader](/asset/image.png)

- Nb: Kedalam binary dan install secara langsung dengan : `wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz`

13. Lakukan untar dari folder hadoop yang sudah didownload :

    > Ketik : `tar -zxvf ~/Downloads/hadoop-3.3.6.tar.gz`

14. Setelah selesai silahkan ketikan didalam terminal :
    `cd hadoop-3.3.6/etc/hadoop` terminal akan berganti didalam folder yang dituju

15. Kemudian dilanjutkan untuk membuka file `hadoop-env.sh` sebagai setup lingkungan hadoop kita untuk menjalankan basic perintahnya dengan mengetikan `nano hadoop-env.sh` nanti terbuka seperti code.

16. Jangan tutup terlebih dahulu, cari `# Java Implementation` dengan mencari dibawahnya variabel `export JAVA_HOME` di tambahkan path dimana java berada disini (default) : `export JAVA_HOME=/home/lib/jvm/java-8-openjdk-amd64`

17. Save File dengan (ctrl + s) + (ctrl + x)

18. Lakukan setup pada beberapa file, masih dalam folder yang sama dengan :

`core-site.xml`

```xml
<configuration>
 <property>
 <name>fs.defaultFS</name>
 <value>hdfs://localhost:9000</value>
 </property>
 <property>
<name>hadoop.proxyuser.dataflair.groups</name> <value>*</value>
 </property>
 <property>
<name>hadoop.proxyuser.dataflair.hosts</name> <value>*</value>
 </property>
 <property>
<name>hadoop.proxyuser.server.hosts</name> <value>*</value>
 </property>
 <property>
<name>hadoop.proxyuser.server.groups</name> <value>*</value>
 </property>
</configuration>
```

`hdfs-site.xml`

```xml
<configuration>
 <property>
 <name>dfs.replication</name>
 <value>1</value>
 </property>
</configuration>
```

`mapred-site.xml`

```xml
<configuration>
 <property>
 <name>mapreduce.framework.name</name>  <value>yarn</value>
 </property>
 <property>
 <name>mapreduce.application.classpath</name>

<value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
 </property>
</configuration>
```

`yarn-site.xml`

```xml
<configuration>
 <property>
 <name>yarn.nodemanager.aux-services</name>
 <value>mapreduce_shuffle</value>
 </property>
 <property>
 <name>yarn.nodemanager.env-whitelist</name>

<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREP END_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
 </property>
</configuration>
```

19. Kemudian setup ssh, ketik `cd` terlebih dahulu untuk kembali di home base awal; kemudian copy 1 baris per baris dengan :

```bash
ssh localhost
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
hadoop-3.3.6/bin/hdfs namenode -format
```

20. Kemudian format file system terlebih dahulu :

> `export PDSH_RCMD_TYPE=ssh`

21. hadoop siap digunakan untuk server file system

```bash
start-all.sh # untuk start server hadoop
stop-all.sh # untuk stop server hadoop
```
