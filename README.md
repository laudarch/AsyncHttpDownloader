AsyncHttpDownloader
==========================
AsyncHttpDownloader is an android open source project which is used to download and manager download tasks.

**Features**
* Multiple download tasks.
* Manager download tasks.
* Listener to download events across Activities.

**Adding the library to your project**
* Download this project and unzip to your root Android Studio workplace.
* Make edition to your project's settings.gradle file:
```gradle
	include ':app'
	include '..:AsyncHttpDownloader:asyncHttpDownloaderLibrary' //add this line
```
* Add gradle compile code to your app leve build.gradle file:
```gradle
	dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
	compile 'com.android.support:appcompat-v7:23.0.0'  //add support v7 library
    compile project(':..:MaterialAppBase:materialAppBaseLibrary')   //add this library
    //and your other dependencies.
}
```

**Implements codes**
* Setting file saving storage
```java
	// If you want to set default storage to save the destination files, you should either do it so
    // once the application is started, or you can do it when you first time implement the downloader.
    // If you set this, the directory will be created under your SD card root directory (if not exists).
    // If you do not set this, all downloaded files will be saved under /sdcard/temp/ directory if you 
    // either not give a destination file url.
    AsyncHttpDownloader.getInstance().setDefaultDirectoryUnderSdCard("ExampleAsyncDownload/downloads");
```

* Adding a download mission to the pool
```java
	// If you don't give a destination file path, the downloaded file will be saved to the default storage
	AsyncHttpDownloader.getInstance().addDownloadTask(this, downloadUrl2);
	
	// you can also specify a destination file path where you want the downloaded file to be
    AsyncHttpDownloader.getInstance().addDownloadTask(this, downloadUrl3, destinationFileUrl);
```

* Register a listener to listen ongoing downloads
```java

	//Instantiate a new listener
	AsyncHttpDownloaderListener downloaderListener = new AsyncHttpDownloaderListener(
        new AsyncHttpDownloaderListener.Callback() {
        
        @Override
        public void onProgressUpdate(String downloadUrl, int total, int completed) {
            
        }

        @Override
        public void onFailed(String downloadUrl, String errorMessage) {

        }

        @Override
        public void onSuccess(String downloadUrl, File file) {

        }
    });
    
    //You may want to register the listener when the activity onCreate.
    @Override
    public void onCreate(Bundle savedInstanceState){
    	super.onCreate(savedInstanceState);
    	downloaderListener.register(this);
    }
    
    //Don't remember unregister the listener when your activity is destroyed
    @Override
    public void onDestroy(){
    	downloaderListener.unregister(this);
    	
    	// When the activity is destroyed, cancel all running tasks if you want.
        // If you don't cancel, it will run in the background until all tasks are
        // done, when you open the activity again, you can still see the running task.
        AsyncHttpDownloader.getInstance().cancelAllRunningTasks();
        
    	super.onDestroy();
    }
```

