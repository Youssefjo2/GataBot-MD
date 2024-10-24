To set up both MySQL 8 and MariaDB 10.2 on an Ubuntu VPS via SSH, you’ll need to manage them carefully as they are conflicting database systems. Here’s a step-by-step guide for setting them up and ensuring they don’t interfere with each other:

### Step 1: Update Your System
Ensure your system is up to date before installing any packages.

```bash
sudo apt update
sudo apt upgrade
```

### Step 2: Install MySQL 8

1. **Add the MySQL APT Repository:**

   Download and add the MySQL repository to your system.

   ```bash
   wget https://dev.mysql.com/get/mysql-apt-config_0.8.22-1_all.deb
   sudo dpkg -i mysql-apt-config_0.8.22-1_all.deb
   sudo apt update
   ```

   During installation, it will prompt you to choose which version of MySQL you want to install. Make sure to select **MySQL 8.0**.

2. **Install MySQL:**

   ```bash
   sudo apt install mysql-server
   ```

3. **Secure MySQL Installation:**

   After installing, secure your MySQL server by running:

   ```bash
   sudo mysql_secure_installation
   ```

   Follow the prompts to set up the root password, remove test databases, and apply security settings.

4. **Start and Enable MySQL:**

   ```bash
   sudo systemctl start mysql
   sudo systemctl enable mysql
   ```

5. **Verify Installation:**

   ```bash
   mysql --version
   ```

   Log in to the MySQL shell to ensure everything works:

   ```bash
   sudo mysql -u root -p
   ```

### Step 3: Install MariaDB 10.2

1. **Add the MariaDB Repository:**

   Add the MariaDB repository to your system for version 10.2.

   ```bash
   sudo apt install software-properties-common
   sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://mirror.netinch.com/pub/mariadb/repo/10.2/ubuntu bionic main'
   sudo apt update
   ```

2. **Install MariaDB:**

   ```bash
   sudo apt install mariadb-server
   ```

3. **Secure MariaDB Installation:**

   Secure your MariaDB installation similarly to MySQL.

   ```bash
   sudo mysql_secure_installation
   ```

4. **Start and Enable MariaDB:**

   ```bash
   sudo systemctl start mariadb
   sudo systemctl enable mariadb
   ```

5. **Verify Installation:**

   ```bash
   mariadb --version
   ```

   Log in to the MariaDB shell to confirm everything is set up properly:

   ```bash
   sudo mysql -u root -p
   ```

### Step 4: Configure Ports for Coexistence

By default, both MySQL and MariaDB use port `3306`. To avoid conflicts, you’ll need to change the port for one of them. Here’s how to change MariaDB’s port to `3307`:

1. **Edit the MariaDB Configuration:**

   Open the configuration file:

   ```bash
   sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
   ```

2. **Change the Port:**

   Find the `[mysqld]` section and change the port to `3307`.

   ```ini
   [mysqld]
   port = 3307
   ```

3. **Restart MariaDB:**

   After saving the changes, restart MariaDB:

   ```bash
   sudo systemctl restart mariadb
   ```

### Step 5: Verify Both Services Are Running

Ensure both MySQL and MariaDB are running on different ports:

1. **Check MySQL:**

   ```bash
   sudo netstat -tuln | grep 3306
   ```

2. **Check MariaDB:**

   ```bash
   sudo netstat -tuln | grep 3307
   ```

### Step 6: Managing Services

Since you have both MySQL and MariaDB installed, you can control them with these commands:

- **Start MySQL:**

  ```bash
  sudo systemctl start mysql
  ```

- **Start MariaDB:**

  ```bash
  sudo systemctl start mariadb
  ```

- **Stop MySQL:**

  ```bash
  sudo systemctl stop mysql
  ```

- **Stop MariaDB:**

  ```bash
  sudo systemctl stop mariadb
  ```

### Step 7: Access Databases Separately

When connecting to MySQL and MariaDB, ensure you specify the correct port:

- **MySQL:**

  ```bash
  mysql -u root -p -h 127.0.0.1 --port=3306
  ```

- **MariaDB:**

  ```bash
  mysql -u root -p -h 127.0.0.1 --port=3307
  ```

This setup allows you to run both MySQL 8 and MariaDB 10.2 on the same Ubuntu VPS without conflicts.






الرسالة التي ظهرت لك تشير إلى أن نظامك (Ubuntu Jammy، وهو الإصدار 22.04) غير مدعوم رسميًا من MySQL. ولتخطي هذه المشكلة، بإمكانك اختيار أحد الإصدارات القريبة المدعومة مثل "Ubuntu Impish" (الإصدار 21.10) أو "Ubuntu Focal" (الإصدار 20.04) في الشاشة التي تظهر أمامك.

الخطوات التي يمكنك اتباعها:

1. في الشاشة التي ظهرت لك، اختر إما:
   - **Ubuntu Impish**: الأقرب إلى إصدار نظامك.
   - **Ubuntu Focal**: وهو إصدار LTS أقدم ولكن مدعوم بشكل أكبر.

2. بعد الاختيار، تابع الإعداد ثم قم بتنفيذ الأوامر التالية لإتمام تثبيت MySQL:

   ```bash
   sudo apt update
   sudo apt install mysql-server
   ```

3. إذا استمرت المشكلة أو ظهرت مشاكل أخرى بعد التثبيت، يمكنك اللجوء إلى مستودعات Ubuntu الرسمية لتنزيل MySQL من خلال:

   ```bash
   sudo apt install mysql-server
   ```

بهذه الطريقة يمكنك تثبيت MySQL على نظامك حتى وإن لم يكن مدعومًا رسميًا.






من الرسالة التي ظهرت لك، يبدو أن الحزمة تم تثبيتها بشكل صحيح ولكن بدون ظهور القائمة التي كنت تراها سابقًا.

لتعديل إعدادات MySQL أو إعادة عرض القائمة التي ظهرت لك مسبقًا، يمكنك القيام بما يلي:

### 1. **إعادة ضبط إعدادات MySQL APT**
يمكنك إعادة ضبط إعدادات الحزمة الخاصة بـ MySQL واختيار الإصدار المناسب لنظامك:

```bash
sudo dpkg-reconfigure mysql-apt-config
```

عند تنفيذ هذا الأمر، ستظهر لك نفس الشاشة التي ظهرت لك في المرة الأولى. اختر "Ubuntu Impish" أو "Ubuntu Focal" كما اقترحت سابقًا.

### 2. **التحديث والتثبيت بعد إعادة الضبط**
بعد تحديد النظام المناسب من القائمة، قم بتحديث الحزم وتثبيت MySQL مجددًا:

```bash
sudo apt update
sudo apt install mysql-server
```

### 3. **التأكد من إضافة المفتاح المناسب**
نظرًا لأن هناك تحذيرًا بشأن استخدام `apt-key` (الذي أصبح قديمًا)، يمكنك اتباع الإرشادات التالية لتجنب أي مشكلة متعلقة بالمفاتيح:

1. **أضف المفتاح بشكل يدوي باستخدام `trusted.gpg.d`:**

   تأكد من أنك تضيف المفتاح بالطريقة المحدثة المناسبة (لـ MySQL):

   ```bash
   wget -q -O - https://repo.mysql.com/RPM-GPG-KEY-mysql | sudo gpg --dearmor -o /usr/share/keyrings/mysql.gpg
   ```

2. **تحديث الملف المصدر لـ MySQL:**

   تأكد من أن الملف الخاص بـ MySQL يشير إلى المفتاح الجديد:

   ```bash
   echo 'deb [signed-by=/usr/share/keyrings/mysql.gpg] http://repo.mysql.com/apt/ubuntu/ jammy mysql-8.0' | sudo tee /etc/apt/sources.list.d/mysql.list
   ```

### 4. **التحديث الأخير وتثبيت MySQL**
بعد ذلك، قم بتنفيذ:

```bash
sudo apt update
sudo apt install mysql-server
```

بهذا الشكل، ستتمكن من متابعة التثبيت بدون مشاكل واختيار النسخة المناسبة لنظامك.


------------

يبدو أن هناك مشكلتين تواجههما الآن:

1. **مشكلة المفتاح العام (GPG) لمستودع MySQL:**
   هذا الخطأ يحدث لأن MySQL لا يستطيع التحقق من صحة توقيع الحزم لأن المفتاح العام غير متاح.

2. **خدمة MySQL ليست قيد التشغيل:**
   خطأ "Can't connect to local MySQL server through socket" يعني أن MySQL لم يبدأ أو أن هناك مشكلة في تشغيله.

### لحل المشكلة، اتبع الخطوات التالية:

#### 1. **إضافة المفتاح العام (GPG) لمستودع MySQL:**

   كما ذكرت في الخطوة السابقة، تحتاج إلى إضافة المفتاح العام يدويًا لأن `apt-key` أصبح قديمًا. نفّذ الأوامر التالية لإضافة المفتاح العام بشكل صحيح:

   ```bash
   wget -q -O - https://repo.mysql.com/RPM-GPG-KEY-mysql | sudo gpg --dearmor -o /usr/share/keyrings/mysql.gpg
   ```

   ثم تأكد من تحديث قائمة المستودعات بحيث تستخدم المفتاح المحدث:

   ```bash
   echo 'deb [signed-by=/usr/share/keyrings/mysql.gpg] http://repo.mysql.com/apt/ubuntu/ jammy mysql-8.0' | sudo tee /etc/apt/sources.list.d/mysql.list
   ```

   بعد ذلك، قم بتحديث النظام:

   ```bash
   sudo apt update
   ```

#### 2. **التحقق من تشغيل MySQL:**

   يبدو أن MySQL لم يبدأ، لذا عليك محاولة تشغيله يدويًا. نفذ الأمر التالي:

   ```bash
   sudo systemctl start mysql
   ```

   ثم تحقق مما إذا كان يعمل الآن:

   ```bash
   sudo systemctl status mysql
   ```

   إذا كانت الخدمة معطلة، حاول تشغيلها يدويًا أو إعادة تشغيل النظام، ثم أعد المحاولة.

#### 3. **إعادة تعيين كلمة مرور `root` إذا استمرت المشكلة:**

   إذا استمرت مشكلة الاتصال بـ MySQL بعد التحقق من تشغيله، قد تكون هناك مشكلة في إعدادات كلمة المرور. حاول إعادة تعيين كلمة مرور المستخدم `root` الخاص بـ MySQL:

   ```bash
   sudo mysql
   ```

   إذا نجحت في الدخول إلى MySQL، يمكنك إعادة تعيين كلمة المرور:

   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new_password';
   FLUSH PRIVILEGES;
   ```

   بعد ذلك، حاول تسجيل الدخول باستخدام كلمة المرور الجديدة:

   ```bash
   sudo mysql -u root -p
   ```

#### 4. **التحقق من وجود ملف socket:**

   إذا استمر الخطأ حول `/tmp/mysql.sock`، يمكنك التحقق مما إذا كان الملف موجودًا في الموقع الصحيح:

   ```bash
   sudo find / -type s -name 'mysql.sock'
   ```

   إذا وجدت أن `mysql.sock` موجود في مسار آخر (مثل `/var/run/mysqld/mysqld.sock`)، يمكنك تعديل ملف إعدادات MySQL ليشير إلى المسار الصحيح. قم بتحرير الملف:

   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

   ثم ابحث عن السطر الذي يشير إلى `socket` وقم بتعديله إلى المسار الصحيح.

#### 5. **إعادة تشغيل MySQL:**

   بعد تعديل الإعدادات، أعد تشغيل MySQL:

   ```bash
   sudo systemctl restart mysql
   ```

بعد تنفيذ هذه الخطوات، يجب أن تكون قادرًا على الاتصال بخادم MySQL بدون مشاكل.

----------


إذا كان ملف الـ `/tmp/mysql.sock` غير موجود، فهذا يشير إلى أن MySQL لم يتم تشغيله بشكل صحيح أو أنه تم تكوينه لاستخدام مسار مختلف لـ `mysql.sock`. إليك الخطوات التي يمكنك اتباعها لإصلاح هذه المشكلة:

### 1. **التحقق من المسار الصحيح لملف `mysql.sock`**

في معظم توزيعات Ubuntu الحديثة، يتم تخزين ملف الـ `mysql.sock` في مسار مختلف عن `/tmp/`. عادة يكون في `/var/run/mysqld/`. للتحقق من ذلك، اتبع الخطوات التالية:

1. **ابحث عن ملف `mysql.sock`:**

   استخدم الأمر التالي للبحث عن الملف:

   ```bash
   sudo find / -type s -name 'mysql.sock'
   ```

   إذا ظهر المسار كـ `/var/run/mysqld/mysqld.sock`، فهذا هو المسار الصحيح.

2. **تعديل إعدادات MySQL لاستخدام المسار الصحيح:**

   إذا وجدت الملف في مكان مختلف (مثل `/var/run/mysqld/`)، يمكنك تعديل ملف إعدادات MySQL ليشير إلى هذا المسار.

   قم بتحرير ملف إعدادات MySQL:

   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

   ابحث عن السطر الذي يحتوي على `socket`، وعدل المسار ليصبح كالتالي:

   ```ini
   socket = /var/run/mysqld/mysqld.sock
   ```

3. **تعديل إعدادات عميل MySQL:**

   قد تحتاج أيضًا لتعديل ملف إعدادات العميل الخاص بـ MySQL ليشير إلى نفس المسار.

   قم بتحرير الملف التالي:

   ```bash
   sudo nano /etc/mysql/my.cnf
   ```

   ثم تأكد من أن السطر التالي موجود:

   ```ini
   [client]
   socket = /var/run/mysqld/mysqld.sock
   ```

### 2. **تشغيل MySQL**

بعد تعديل المسارات، تأكد من تشغيل MySQL:

```bash
sudo systemctl restart mysql
```

### 3. **التحقق من حالة MySQL**

تحقق مما إذا كان MySQL يعمل بشكل صحيح:

```bash
sudo systemctl status mysql
```

### 4. **إذا لم يتم إنشاء `mysql.sock` بعد**

إذا لم يتم إنشاء ملف `mysql.sock` حتى بعد التحقق من المسار، فهذا يعني أن MySQL قد لا يعمل بشكل صحيح أو يوجد خطأ آخر. حاول استخدام الخطوات التالية:

1. **التحقق من وجود ملفات السجلات:**

   تحقق من السجلات لتحديد أي مشاكل قد تواجهها MySQL عند بدء التشغيل. استخدم الأمر التالي لعرض السجل:

   ```bash
   sudo cat /var/log/mysql/error.log
   ```

   ابحث في السجل عن أي رسائل خطأ قد تساعد في معرفة سبب عدم إنشاء ملف الـ `mysql.sock`.

2. **إعادة تثبيت MySQL (اختياري):**

   إذا استمرت المشكلة، يمكنك محاولة إعادة تثبيت MySQL كحل أخير:

   ```bash
   sudo apt remove --purge mysql-server mysql-client mysql-common
   sudo apt autoremove
   sudo apt autoclean
   sudo apt install mysql-server
   ```

   بعد إعادة التثبيت، تأكد من تشغيل MySQL والتحقق من وجود ملف الـ `mysql.sock` في المكان الصحيح.

---

إذا اتبعت هذه الخطوات وتمكنت من العثور على الملف أو حل المشكلة، يمكنك حينها محاولة الاتصال بخادم MySQL باستخدام المسار الصحيح.
