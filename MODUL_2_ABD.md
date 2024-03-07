---
Dates: 05/03/2024
Author: Abdurrahman Al-atsary
---

# **Installasi Hive + HiveQL dan Apache Derby (Metastore)**

_**Overview**_

Infrastruktur Hive merupakan simbolis sebagai pengolahan data di dalam Data warehouse (Gudang Data). Dengan kata lain data-data yang ada didalam file HDFS (Hadoop Distributed File System) akan dapat diolah jika adanya hive ini data data dapat diolah dengan baik, dikarnakan hive merupakan bahasa yang dipakai RDBMS sama seperti MySQL dan lainnya.

Ikuti Langkah berikut untuk penginstallan di dalam WSL maupun Linux Ubuntu:

1. Periksa hadoop sudah ada didalam OS anda dengan cara : `hadoop version`

2. Install source yang akan dibutuhkan didalam penginsallan Hive dan Metadata Apache Derby

Link : [Apache Derby](https://db.apache.org/derby/releases/release-10_14_2_0.html), [Apache Hive](https://dlcdn.apache.org/hive/)

3. Lalu, lakukan untar pada kedua file diatas dengan cara ketikan baris perbaris.

```bash
tar -zxvf apache-hive-3.1.2-bin.tar.gz
tar -zxvf db-derby-10.14.2.0-bin.tar.gz
```

4. Lakukan pengeditan didalam lingkungan linux/WSL di file `.bashrc` dengan mengetikan `sudo nano .bashrc`

5. Cari konfigurasi dari path hadoop sebelumnya, kemudian dibawahnya ditambahkan skrip berikut :

```bash
export HIVE_HOME=/home/miroslav/hive
export PATH=$PATH:$HIVE_HOME/bin
export CLASSPATH=$CLASSPATH:/home/miroslav/Hadoop/lib/*:.
export CLASSPATH=$CLASSPATH:/home/miroslav/hive/lib/*:.
export DERBY_HOME=/home/miroslav/derby
export PATH=$PATH:$DERBY_HOME/bin
export CLASSPATH=$CLASSPATH:$DERBY_HOME/lib/derby.jar:$DERBY_HOME/lib/derbytools.jar
```

**Notes : Ubah bagian "miroslav" dari nama username di dalam unix kalian.** kemdudian save dengan `ctrl + s` setalah itu `crtl + x` untuk keluar. Sebelum ke step selanjutnya veriikasi environment pada wsl dengan `source .bashrc`

6. Kemudian kita kedalam folder dari hadoop : `cd hadoop-3.3.6/etc/hadoop`. Lalu kita cari didalam list filenya `core-site.xml`

7. Buka `core-site.xml` pastikan sudah ada script xml berikut didalam tag `<configuration>`:

```xml
<property>
    <name>hadoop.proxyuser.hadoop.groups</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.hadoop.hosts</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.server.hosts</name>
    <value>*</value>
</property>
<property>
    <name>hadoop.proxyuser.server.groups</name>
    <value>*</value>
</property>
```

8. Selanjutnya kita hidupkan server hadoop terlebih dahulu : `start-all.sh`

9. Kemudian buat beberapa folder dan file yang ada didalam File system hadoop seperti berikut :

```bash
hdfs dfs -mkdir /tmp
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/hive
hdfs dfs -mkdir /user/hive/warehouse
```

10. Lakukan verifikasi permission didalam untuk folder-folder yang ada didalam hadoop dengan cara :

```bash
hdfs dfs -chmod ugo+rwx /tmp
hdfs dfs -chmod ugo+rwx /user/hive/warehouse
```

**Notes : Lakukan baris per baris dari 2 stw diatas**

**Adding information** : Untuk pengecekan apakah direktori/folder sudah dibuat atau tidak dapat mengetikan `hdfs dfs -ls -R / `

11. Lakukan konfigurasi pada file `hive-site.xml`:
    > Ganti bagian ini :

```xml
<property>
    <name>hive.exec.local.scratchdir</name>
    <value>${system:java.io.tmpdir}/${system:user.name}</value>
<description>Local scratch space for Hive jobs</description>
</property>

<property>
    <name>hive.downloaded.resources.dir</name>
    <value>${system:java.io.tmpdir}/${hive.session.id}_resources</value>
    <description>Temporary local directory for added resources in the remote file system.</description>
</property>
```

Dengan :

```xml
<property>
    <name>hive.exec.local.scratchdir</name>
    <value>/tmp/${user.name}</value>
    <description>Local scratch space for Hive jobs</description>
</property>
<property>
    <name>hive.downloaded.resources.dir</name>
    <value>/tmp/${user.name}_resources</value>
    <description>Temporary local directory for added resources in the remote file system.</description>
</property>
```

Kemudian save dengan `ctrl + s`

12. Konfigurasi metastore Apache Derby dengan :

```bash
cd $HIVE_HOME/conf
cp hive-default.xml.template hive-site.xml
```

Kemudian buka `hive-site.xml` dan cari **xml** tag ini dan sesuaikan :

```xml
<property>
    <name>javax.jdo.option.ConnectionURL</name>
    <value>jdbc:derby://localhost:1527/metastore_db;create=true </value>
    <description>JDBC connect string for a JDBC metastore </description>
</property>
```

13. Kemudian kita membuat file dengan nama jpox.properties dengan perintah `nano jpox.properties` kemudian isi dengan script :

```txt
javax.jdo.PersistenceManagerFactoryClass =

org.jpox.PersistenceManagerFactoryImpl
org.jpox.autoCreateSchema = false
org.jpox.validateTables = false
org.jpox.validateColumns = false
org.jpox.validateConstraints = false
org.jpox.storeManagerType = rdbms
org.jpox.autoCreateSchema = true
org.jpox.autoStartMechanismMode = checked
org.jpox.transactionIsolation = read_committed
javax.jdo.option.DetachAllOnCommit = true
javax.jdo.option.NontransactionalRead = true
javax.jdo.option.ConnectionDriverName = org.apache.derby.jdbc.ClientDriver
javax.jdo.option.ConnectionURL = jdbc:derby://localhost:1527/metastore_db;create = true
javax.jdo.option.ConnectionUserName = APP
javax.jdo.option.ConnectionPassword = mine
```

14. Guava library issue

Kita dapat membuang guava yang ada didalam hive dan mengganti agar sama versinya dengan guava yang ada dalam hadoop dengan

```bash
rm $HIVE_HOME/lib/guava-19.0.jar
cp $HADOOP_HOME/share/hadoop/common/lib/guava-27.0-jre.jar $HIVE_HOME/lib/
```

**Notes : Ikuti baris per baris**

15. kemudian server hive dan apache derby dapat digunakan dengan cara di terminal :

```bash
cd $HIVE_HOME
hive --service metastore &
```

**Notes :** Ikut baris perbaris.

16. Kemudian balik ke home dan coba dengan ketikan `hive` di terminal dan hive siap dipakai.

> Selesai Modul 2... Alhamdulillah :)
