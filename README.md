(https://github.com/ADmad/CakeMongo.git)
** Unfortunately, it appears that the CakeMongo plugin is no longer maintained or available for newer versions of CakePHP, such as 2.6. The plugin you referred to is for CakePHP 5.x, and thus isn't compatible with your version of CakePHP*
*



For installing **CakePHP 2.1.0** and **MongoDB** on **Windows**, follow these steps. I'll guide you through the installation process using **XAMPP** for Apache, PHP, and MySQL, and also setting up **MongoDB** and its PHP driver.

---

## **Step 1: Install XAMPP**

1. **Download XAMPP**:
   - Go to [XAMPP's with php 5.6](https://sourceforge.net/projects/xampp/files/XAMPP%20Windows/5.6.40/) and download the **XAMPP** installer for Windows (preferably with PHP 5.6 to match CakePHP 2.1.0 requirements).
   
2. **Install XAMPP**:
   - Run the installer and follow the instructions to install it. Choose to install **Apache** and **MySQL** during the setup process.
   
3. **Start Apache and MySQL**:
   - Once XAMPP is installed, open the **XAMPP Control Panel** and click the "Start" buttons for both **Apache** and **MySQL**.

---

## **Step 2: Download and Install CakePHP 2.1.0**

1. **Download CakePHP 2.1.0**:
   - You can download CakePHP 2.1.0 from GitHub: [CakePHP 2.1.0 GitHub release](https://github.com/cakephp/cakephp/archive/2.1.0.tar.gz).
   - Extract the downloaded archive to the `htdocs` folder in your XAMPP installation, which is typically located at:
     ```
     C:\xampp\htdocs\
     ```

2. **Rename the Folder**:
   - After extracting, rename the folder to something like `myapp`. Your path should look like:
     ```
     C:\xampp\htdocs\myapp
     ```

3. **Set Folder Permissions**:
   - Ensure that the `tmp` and `webroot` folders inside `C:\xampp\htdocs\myapp\app` are writable. This can be done by right-clicking the folders and modifying their properties.

---

## **Step 3: Configure CakePHP**

1. **Database Configuration**:
   Even though you’ll be using MongoDB, CakePHP requires a default database configuration. You can use MySQL for this:

   - Open `C:\xampp\htdocs\myapp\app\Config\database.php`.
   - Edit the `default` configuration with your MySQL credentials (default is `root` with no password):

   ```php
   class DATABASE_CONFIG {
       public $default = array(
           'datasource' => 'Database/Mysql',
           'persistent' => false,
           'host' => 'localhost',
           'login' => 'root',
           'password' => '',
           'database' => 'cakephp',
           'prefix' => '',
           'encoding' => 'utf8',
       );
   }
   ```

2. **Test CakePHP Installation**:
   - Open your browser and go to `http://localhost/myapp`. You should see the CakePHP welcome page.

---

## **Step 4: Install MongoDB on Windows**

1. **Download MongoDB**:
   - Go to the [MongoDB download page](https://www.mongodb.com/try/download/community) and download the installer for Windows.
   
2. **Install MongoDB**:
   - Run the MongoDB installer and follow the instructions. Make sure to install **MongoDB as a Windows service** and to specify the installation path, typically:
     ```
     C:\Program Files\MongoDB\Server\{version}\
     ```

3. **Create Data Directory**:
   - MongoDB requires a data directory. Create the following folder:
     ```
     C:\data\db
     ```

4. **Start MongoDB**:
   - Open the **Command Prompt** and run MongoDB with the following command:
     ```bash
     "C:\Program Files\MongoDB\Server\{version}\bin\mongod.exe"
     ```
   - This should start MongoDB, listening on its default port (27017).

---

## **Step 5: Install MongoDB PHP Driver (Legacy `mongo` Driver)**

Since CakePHP 2.1.0 runs on PHP 5.x, you'll need the legacy MongoDB PHP driver, `mongo`.

1. **Download MongoDB PHP Driver**:
   - Go to the PECL MongoDB driver download page: [PECL MongoDB legacy](https://pecl.php.net/package/mongo/1.6.16/windows) (choose the appropriate PHP version, such as `5.6` if you're using PHP 5.6 with XAMPP).
   - Download the correct `.dll` file that matches your PHP version (thread-safe or non-thread-safe, x86 or x64 based on your setup).

2. **Install the PHP Driver**:
   - Copy the `.dll` file into your XAMPP’s `php\ext` directory, for example:
     ```
     C:\xampp\php\ext\php_mongo.dll
     ```

3. **Enable the Mongo Extension**:
   - Open `C:\xampp\php\php.ini` and add the following line at the end of the file:
     ```ini
     extension=php_mongo.dll
     ```

4. **Restart Apache**:
   - Open the XAMPP Control Panel and click **Stop** and then **Start** for Apache to reload the PHP configuration.

5. **Verify the MongoDB Driver Installation**:
   - Create a `test.php` file in `C:\xampp\htdocs\` with the following code:
     ```php
     <?php
     phpinfo();
     ?>
     ```
   - Open `http://localhost/test.php` in your browser and check for the `mongo` extension in the PHP info page.

---

## **Step 6: Connect MongoDB to CakePHP**

1. **Create a CakePHP Controller**:
   Create a controller in `C:\xampp\htdocs\myapp\app\Controller\UsersController.php` that connects to MongoDB:

   ```php
   App::uses('AppController', 'Controller');

   class UsersController extends AppController {
       public function index() {
           // Create MongoDB connection
           $mongoClient = new MongoClient(); // Connects to localhost by default
           $db = $mongoClient->selectDB('my_database'); // Select your MongoDB database
           $collection = $db->selectCollection('users'); // Choose your collection

           // Example: Find a user
           $user = $collection->findOne(['name' => 'John Doe']);

           // Pass the user data to the view
           $this->set('user', $user);
       }
   }
   ```

2. **Create a View**:
   Create a view file in `C:\xampp\htdocs\myapp\app\View\Users\index.ctp` to display the MongoDB data:

   ```php
   <h1>User Info</h1>
   <p>Name: <?php echo $user['name']; ?></p>
   <p>Email: <?php echo $user['email']; ?></p>
   ```

3. **Test the Connection**:
   Go to `http://localhost/myapp/users/index` in your browser. You should see user data from MongoDB.

---

By following these steps, you should have CakePHP 2.1.0 installed and connected to MongoDB on your local Windows machine. Let me know if you encounter any issues or need further assistance!




. problem 

**4.4.9**
- Note: MongoDB version 4.4.9 has a critical issue, if you are running this version in production, please familiarize yourself with SERVER-77506, SERVER-82353, WT-10461 (if running on ARM64 or POWER system architectures), and WT-10551 (if relying on incremental backups on Ops Manager or Cloud Manager clusters) and consider upgrading to the latest version
