#建立第一个应用

![Memos, a minimalist notepad app](images/originals/memos-app.png)

在本章中，我们将着手建立一个名为**Menosz**的简易记事本应用。在开工之前，让我们先来回顾下该应用的运行方式。

该应用具有三个界面。主界面列出记事条目。当你点击一条记事（或者添加一条记事）的时候，就会跳转到可以对相关标题和内容进行编辑的详细界面。如下图所示：

![Memos, editing screen](images/originals/memos-editing-screen.png)

在上示界面中，我们可以通过点击垃圾桶图标来删除这项记事。点击之后会弹出一个请求确认的对话框。

![Memos, note removal confirmation screen](images/originals/memos-delete-screen.png)

该记事本的源码在这里：[the Memos Github Repo](https://github.com/soapdog/memos-for-firefoxos)（或zip格式：[the Memos.zip](https://github.com/soapdog/memos-for-firefoxos/archive/master.zip)）。推荐大家下载简单快捷的zip压缩文件。在本书的**code**文件夹里也有备份：[github repository for this book](https://github.com/soapdog/firefoxos-quick-guide)

Memos采用[IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB/Using_IndexedDB)作为数据库来保存条目；采用[Gaia Building Blocks](http://buildingfirefoxos.com/building-blocks)来建立交互界面。再版时我会和大家更加深入细致地探讨Gaia Building Blocks，现在我们只管去用它。读者可以通过上述链接了解交互界面开发工具等各类详细内容。

首先我们为要开发的应用建立一个名为 **memos**的文件夹。

##创建应用的mainfest文件
Memos的mainfest文相当简洁。先在**memos**文件下创建一个名为**manifest.webapp** 的文件。mainfest是一种用来描述应用属性的[JSON](http://json.org)格式文件。该文件通常包含应用名称，应用图标源地址，从哪个文件开始加载应用，该应用会调用哪些用户级API函数等各项信息。

以下我们可以看到Memos应用中mainfest文件的详细内容。在复制这些数据的时候要注意避免在文本中添加额外的逗号导致JSON格式出错。当前有很多帮助开发者合法化JSON文件格式的工具，不过这里推荐一款专门用来生成mainfest文件的在线工具。详情参见[http://appmanifest.org/](http://appmanifest.org/)（译注：这个链接有问题）。关于mainfest的更多内容参见[this page on MDN about them](https://developer.mozilla.org/docs/Apps/Manifest)。

<<[Memos manifest file (*manifest.webapp*)](code/memos/manifest.webapp)

我们来看下以上mainfest文件中包含了哪些字段。

|Field		|Description                                                                        |
|-----------|-----------------------------------------------------------------------------------|
|name		|This is the name of the application.		                                                |
|version	|This is the current version of the app. 										    |
|launch_path|What file is used to launch your application.					                    |
|permissions|What API permissions your app requests. More information about this below.				|
|developer  |Who developed this application 													|
|icons		|The icons used by the app in many different sizes.									|

这些字段中比较重要的是授权许可字段，通过在该字段中声明对*存储设备*的读写权限，应用才能运用IndexedDB无限制地存储数据。[^存储限制]（由于权限许可应用软件才可以随心所欲地存储-不过要注意不要过多占用设备的存储空间）。

[^存储权限]：相关权限的更多内容参见[the page on MDN about app permissions](https://developer.mozilla.org/en-US/docs/Web/Apps/App_permissions)。

既然已经建好mainfest文件，接下来我们开始着手创建HTML文件。

##创建HTML文件
创建HTML文件之前，我们简略地探讨一些关于[Gaia Building Blocks](http://buildingfirefoxos.com/building-blocks)的内容。Gaia Building Blocks中包含了很多可以重用到开发者自己的应用上，Firefox OS风格的交互界面代码模板。

就像网页上那样，开发者并没有被强制要求采用以上Firefox OS风格的交互界面模块。是否采用Gaia模块完全取决于开发者自身抉择。并且，一个好的应用本来就应当具有别具一格的自身特点和用户体验。还要说明一点，开发者的应用并不会因为没有采用Gaia模板而遭受偏见或惩罚。我在本书中采用Gaia界面模板是因为本人在UI设计方面不行（也没钱另聘平面设计师）。

The HTML structure that we use in this application was built following the patterns adopted by the Gaia Building Blocks where each screen is a `<section>` and the elements follow a predefined format. If you haven't already, download the source code from the [memos repository](https://github.com/soapdog/memos-for-firefoxos) so that you have the files (including the Building Blocks) to use. For those not confident with git and GitHub, the files are also available as a [.zip file](https://github.com/soapdog/memos-for-firefoxos/archive/master.zip).

W> Warning: The version of the Gaia Building Blocks I used for this app is not the most up-to-date available from Mozilla. Trying to update to the current version will, unfortunately, break the Memos app. In your own projects, however, always use the latest version of the Gaia Building Blocks.

### Including the Building Blocks

Before doing anything else copy the **shared** and the **styles** folders that you obtained by downloading the Memos repository to the **memos** folder you created. This will allow use to use the Gaia Building Blocks in our app.

Let's begin our **index.html** files by including the needed bits.

~~~~~~~~
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <link rel="stylesheet" type="text/css" href="style/base.css" />
    <link rel="stylesheet" type="text/css" href="style/ui.css" />
    <link rel="stylesheet" type="text/css" href="style/building_blocks.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style/headers.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style_unstable/lists.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style_unstable/toolbars.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style/input_areas.css" />
    <link rel="stylesheet" type="text/css"
          href="shared/style/confirm.css" />
    <title>Memos</title>
</head>
~~~~~~~~

On *line 01* we declare the DOCTYPE as HTML5. From *line 05 up to 15* we include the CSS from the various components that we're going to use in our app such as headers, lists, text entry fields and more.

### Building the main screen

Now we can start building the various screens. As mentioned earlier, each screen used by our app is a `<section>` inside the HTML `<body>`. The body tag must have an attribute *role* with its value equal to *application* because that is used by the CSS selectors to build the interface, so our body tag will be `<body role="application">`. Let's build the first screen and declare our body tag as well.

~~~~~~~~
<body role="application">

<section role="region" id="memo-list">
    <header>
        <menu type="toolbar">
            <a id="new-memo" href="#"><span class="icon icon-add">add</span></a>
        </menu>
        <h1>Memos</h1>
    </header>
    <article id="memoList" data-type="list"></article>
</section>
~~~~~~~~

Our screen has a `<header>` containing a button to add new notes and the application name. The screen also has an `<article>` which will be used to hold the list of stored notes. We're going to use the button and the article IDs to capture events when we reach the JavaScript implementation part.

Be aware that each screen is a fairly straight forward HTML chunk. Building these same screens in many languages usually requires a lot more work. All we're doing is declaring our containers and giving them IDs when we need to reference them later.

Now that the main screen is done, let's build the editing screen.

### Building the editing screen

The editing screen is a bit more complex because it also holds the dialog box used when the user tries to delete a note.

~~~~~~~~
<section role="region" id="memo-detail" class="skin-dark hidden">
    <header>
        <button id="back-to-list"><span class="icon icon-back">back</span>
        </button>
        <menu type="toolbar">
            <a id="share-memo" href="#"><span class="icon icon-share">share</span>
            </a>
        </menu>
        <form action="#">
            <input id="memo-title" placeholder="Memo Title" required="required"
	           type="text">
            <button type="reset">Remove text</button>
        </form>
    </header>
    <p id="memo-area">
        <textarea placeholder="Memo content" id="memo-content"></textarea>
    </p>
    <div role="toolbar">
        <ul>
            <li>
                <button id="delete-memo" class="icon-delete">Delete</button>
            </li>
        </ul>
    </div>
    <form id="delete-memo-dialog" role="dialog" data-type="confirm"
          class="hidden">
        <section>
            <h1>Confirmation</h1>
            <p>Are you sure you want to delete this memo?</p>
        </section>
        <menu>
            <button id="cancel-delete-action">Cancel</button>
            <button id="confirm-delete-action" class="danger">Delete</button>
        </menu>
    </form>
</section>
~~~~~~~~

At the top of the screen, represented by the `<header>` element, we have:

 * a back button to return to the main screen,
 * a text entry field that is used to hold the title of the given note,
 * and a button that is used to share the note over email.

Below the top toolbar, we have a paragraph holding a `<textarea>` that holds the content of the note and then another toolbar with a trashcan button used to delete the current viewed note.

These three elements and their child nodes are the editing screen. After them we have a `<form>` that is used as a dialog box containing the confirmation screen that is presented to the user when he or she tries to delete a note. This dialog box is pretty simple, it only contains the text of the confirmation prompt and two buttons; one for deleting the note and another for canceling the action.

Now that we're closing this `<section>` we have all our screens implemented and the remaining HTML code is only there to include the JavaScript files and close the html file.

~~~~~~~~
<script src="/js/model.js"></script>
<script src="/js/app.js"></script>
</body>
</html>
~~~~~~~~

## Crafting the JavaScript code

Now we're going to breathe life into our app by adding JavaScript. To better organize this code, I've divided the JavaScript code into two files:

* **model.js:** contains the routines to deal with storage and retrieval of notes but does not contain any app logic or anything related to the interface or data entry. In theory, we could reuse this same file in other apps that required text notes.
* **app.js:** attaches the HTML elements with their event handlers and contains the app logic.

Both files should be placed inside a **js** folder next to the **style** and **shared** folders.

### model.js

We're going to use [IndexedDB](https://developer.mozilla.org/en-US/docs/IndexedDB/Using_IndexedDB) to store our notes. Since we asked the *storage* permission on the app manifest we can store as many notes as we want - however, we should not abuse this! Firefox OS devices generally have very limited storage space, so you always need to be mindful of what data you store (users will delete and down-rate your app if it uses too much storage space!). And storing excessive amounts of data will have a performance penalty, which will make your app feel sluggish. Please also note that when you submit an application to the Firefox OS Marketplace, reviewers will ask you why you need unlimited storage space - if you can't justify why, your application will be rejected.  

The part of the code from *model.js* that is shown below is responsible for opening the connection and creating the storage.

A> Important: This code was written to be understood easily and does not represent the best practices for JS programming. Some global variables are used (I'm so going to hell for this) among other tidbits. The error handling code is basically non-existant. The main objective of this book is to teach the *workflow* of developing apps for Firefox OS and not teaching best JS patterns. That being said, depending on feedback, I will update the code in this book to better reflect best practices if enough people think it will not impact the beginners.

~~~~~~~
var dbName = "memos";
var dbVersion = 1;

var db;
var request = indexedDB.open(dbName, dbVersion);

request.onerror = function (event) {
    console.error("Can't open indexedDB!!!", event);
};
request.onsuccess = function (event) {
    console.log("Database opened ok");
    db = event.target.result;
};

request.onupgradeneeded = function (event) {

    console.log("Running onUpgradeNeeded");

    db = event.target.result;

    if (!db.objectStoreNames.contains("memos")) {

        console.log("Creating objectStore for memos");

        var objectStore = db.createObjectStore("memos", {
            keyPath: "id",
            autoIncrement: true
        });
        objectStore.createIndex("title", "title", {
            unique: false
        });

        console.log("Adding sample memo");
        var sampleMemo1 = new Memo();
        sampleMemo1.title = "Welcome Memo";
        sampleMemo1.content = "This is a note taking app. Use the plus sign " +
	                      "in the topleft corner of the main screen to " +
			      "add a new memo. Click a memo to edit it. All " +
			      "your changes are automatically saved.";

        objectStore.add(sampleMemo1);
    }
}
~~~~~~~

A> Important: Forgive me again for the globals, this is an educational resource only. Another detail is that I removed the comments from the source code to save space in the book. If you pick the source from GitHub you will get all the comments.

The code above creates a *db* object and a *request* object. The *db* object is used by other functions in the source to manipulate the notes storage.

On the implementation of the `request.onupgradeneeded` function we also create a welcome note. This function is executed when the application runs for the first time (or when the database version changes). This way once the application launches for the first time, the database is initialized with a single welcome note.

With our connection open and the storage initialized its time to implement the basic functions for note manipulation.

~~~~~~~~
function Memo() {
    this.title = "Untitled Memo";
    this.content = "";
    this.created = Date.now();
    this.modified = Date.now();
}

function listAllMemoTitles(inCallback) {
    var objectStore = db.transaction("memos").objectStore("memos");
    console.log("Listing memos...");

    objectStore.openCursor().onsuccess = function (event) {
        var cursor = event.target.result;
        if (cursor) {
            console.log("Found memo #" + cursor.value.id +
	                         " - " + cursor.value.title);
            inCallback(null, cursor.value);
            cursor.continue();
        }
    };
}

function saveMemo(inMemo, inCallback) {
    var transaction = db.transaction(["memos"], "readwrite");
    console.log("Saving memo");

    transaction.oncomplete = function (event) {
        console.log("All done");
    };

    transaction.onerror = function (event) {
        console.error("Error saving memo:", event);
        inCallback({
            error: event
        }, null);

    };

    var objectStore = transaction.objectStore("memos");

    inMemo.modified = Date.now();

    var request = objectStore.put(inMemo);
    request.onsuccess = function (event) {
        console.log("Memo saved with id: " + request.result);
        inCallback(null, request.result);

    };
}

function deleteMemo(inId, inCallback) {
    console.log("Deleting memo...");
    var request = db.transaction(["memos"],
                  "readwrite").objectStore("memos").delete(inId);

    request.onsuccess = function (event) {
        console.log("Memo deleted!");
        inCallback();
    };
}
~~~~~~~~

On the piece of code above we create a constructor function that creates new Memos with some fields already initialized. After that we implement functions for listing, saving and removing notes. Many of these functions receive a callback parameter called `inCallback` which is a function to be called after the function does its thing. This is needed due to the asynchronous nature of IndexedDB. All callbacks have the same signature which is `callback(error, value)` where one of the values is null depending on the outcome of the previous function.

A> Since this is a beginner book I've opted not to use [*Promises*](https://developer.mozilla.org/en-US/docs/Mozilla/JavaScript_code_modules/Promise.jsm/Promise) since many beginners are not familiar with the concept. I recommend using such concepts to create easier to maintain code that is more pleasant to read.

Now that our note storage and manipulation functions are ready, let's implement our app logic in a file called **app.js**.

### app.js

This file will contain our app logic. Since the source code is too large for me to place it all at once on the book, I will break it in parts and explain each part piece by piece.

~~~~~~~~
var listView, detailView, currentMemo, deleteMemoDialog;

function showMemoDetail(inMemo) {
    currentMemo = inMemo;
    displayMemo();
    listView.classList.add("hidden");
    detailView.classList.remove("hidden");
}


function displayMemo() {
    document.getElementById("memo-title").value = currentMemo.title;
    document.getElementById("memo-content").value = currentMemo.content;
}

function shareMemo() {
    var shareActivity = new MozActivity({
        name: "new",
        data: {
            type: "mail",
            body: currentMemo.content,
            url: "mailto:?body=" + encodeURIComponent(currentMemo.content) +
	                "&subject=" + encodeURIComponent(currentMemo.title)

        }
    });
    shareActivity.onerror = function (e) {
        console.log("can't share memo", e);
    };
}

function textChanged(e) {
    currentMemo.title = document.getElementById("memo-title").value;
    currentMemo.content = document.getElementById("memo-content").value;
    saveMemo(currentMemo, function (err, succ) {
        console.log("save memo callback ", err, succ);
        if (!err) {
            currentMemo.id = succ;
        }
    });
}

function newMemo() {
    var theMemo = new Memo();
    showMemoDetail(theMemo);
}
~~~~~~~~

At the beginning we declare some global variables (yuck!!!) to hold references to some DOM elements that we want to use later inside some functions. The most interesting global is `currentMemo` which is an object that holds the current note that the user is reading.

The `showMemoDetail()` and `displayMemo()` functions work together. The first one loads the selected note into the `currentMemo` and manipulates the CSS of the elements so that the editing screen is shown. The second one picks the content from the `currentMemo` variable and places it on the screen. We could do both things on the same function but having them separate makes it easier to experiment with new implementations.

The `shareMemo()` function uses a [WebActivity](https://hacks.mozilla.org/2013/01/introducing-web-activities/) to open the email application with a new message pre-filled with the selected notes content.

The `textChanged()` function picks the data from the entry fields and place them into the `currentMemo` object and then saves the note. This is done because the application is an `auto-save` app where your content is always saved. All alterations on the content or title of the note will trigger this function and the note will always be saved on the IndexedDB storage.

The `newMemo()` function creates a new note and opens the editing screen with it.

~~~~~~~~
function requestDeleteConfirmation() {
    deleteMemoDialog.classList.remove("hidden");
}

function closeDeleteMemoDialog() {
    deleteMemoDialog.classList.add("hidden");
}

function deleteCurrentMemo() {
    closeDeleteMemoDialog();
    deleteMemo(currentMemo.id, function (err, succ) {
        console.log("callback from delete", err, succ);
        if (!err) {
            showMemoList();
        }
    });
}

function showMemoList() {
    currentMemo = null;
    refreshMemoList();
    listView.classList.remove("hidden");
    detailView.classList.add("hidden");
}
~~~~~~~~

The `requestDeleteConfirmation()` function is responsible for showing the note removal confirmation dialog.

The `closeDeleteMemoDialog()` and `deleteCurrentMemo()` are triggered by the buttons on the removal confirmation dialog.

The `showMemoList()` function does some clean up before showing the list of stored notes. For example, it cleans the content of `currentMemo` since we're not reading any memo yet.

~~~~~~~~
function refreshMemoList() {
    if (!db) {
        // HACK:
        // this condition may happen upon first time use when the
        // indexDB storage is under creation and refreshMemoList()
        // is called. Simply waiting for a bit longer before trying again
        // will make it work.
        console.warn("Database is not ready yet");
        setTimeout(refreshMemoList, 1000);
        return;
    }
    console.log("Refreshing memo list");

    var memoListContainer = document.getElementById("memoList");


    while (memoListContainer.hasChildNodes()) {
        memoListContainer.removeChild(memoListContainer.lastChild);
    }

    var memoList = document.createElement("ul");
    memoListContainer.appendChild(memoList);

    listAllMemoTitles(function (err, value) {
        var memoItem = document.createElement("li");
        var memoP = document.createElement("p");
        var memoTitle = document.createTextNode(value.title);

        memoItem.addEventListener("click", function (e) {
            console.log("clicked memo #" + value.id);
            showMemoDetail(value);

        });

        memoP.appendChild(memoTitle);
        memoItem.appendChild(memoP);
        memoList.appendChild(memoItem);


    });
}
~~~~~~~~

The `refreshMemoList()` function modifies the DOM by building element by element the list of notes that is displayed on the screen. It would be a lot easier to use some templating aid such as [handlebars](http://handlebarsjs.com/) or [underscore](http://underscorejs.org/) but since this app is built using nothing but *vanilla javascript* we're doing everything by hand. This function is called by `showMemoList()` that was shown above.

These are all the functions used by our app. The only part of the code that is missing is the initialization of the event handlers and the initial call of `refreshMemoList()`.

~~~~~~~
window.onload = function () {
    // elements that we're going to reuse in the code
    listView = document.getElementById("memo-list");
    detailView = document.getElementById("memo-detail");
    deleteMemoDialog = document.getElementById("delete-memo-dialog");

    // All the listeners for the interface buttons and for the input changes
    document.getElementById("back-to-list")
            .addEventListener("click", showMemoList);
    document.getElementById("new-memo")
            .addEventListener("click", newMemo);
    document.getElementById("share-memo")
            .addEventListener("click", shareMemo);
    document.getElementById("delete-memo")
            .addEventListener("click", requestDeleteConfirmation);
    document.getElementById("confirm-delete-action")
            .addEventListener("click", deleteCurrentMemo);
    document.getElementById("cancel-delete-action")
            .addEventListener("click", closeDeleteMemoDialog);
    document.getElementById("memo-content")
            .addEventListener("input", textChanged);
    document.getElementById("memo-title")
            .addEventListener("input", textChanged);

    // the entry point for the app is the following command
    refreshMemoList();

};
~~~~~~~

Now all files are ready and we can begin trying our application on the simulator. There are two ways of doing this depending if you're using the **App Manager** or the old **Firefox OS 1.1 simulator**. In the following section we'll show both. There will be specific chapters on each technology later.

If you're running **Firefox 29 or newer** then you have the **App Manager**, if you're running an older version then you can use the old simulator. Be aware that the App Manager is only able to connect to Firefox OS 1.2+ devices.

If you have a Firefox OS 1.1 device and you're running Firefox 29 then install the Firefox OS 1.1 simulator version 5.0 available for [Mac OS X](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-mac.xpi), [Linux](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-linux.xpi) or [Windows](http://ftp.mozilla.org/pub/mozilla.org/labs/r2d2b2g/r2d2b2g-5.0pre7-windows.xpi). If you install this simulator you will be able to follow the instructions for the old Firefox OS 1.1 simulator and be able to connect your device.

If your device running Firefox OS 1.1 is unlocked and able to receive version 1.2 or later then you should upgrade because it will make your life a lot easier. The Geeksphone Keon, Geeksphone Peak and Geeksphone Revolution have daily Firefox OS builds available at [http://downloads.geeksphone.com/](http://downloads.geeksphone.com/). The ZTE Open can also be upgraded following the instructions at [Upgrading your ZTE Open to Firefox 1.1 or 1.2 (fastboot enabled)](https://hacks.mozilla.org/2014/01/upgrading-your-zte-open-to-firefox-1-1-or-1-2-fastboot-enabled/). The LG Fireweb can't be upgraded, if you are like me and don't like this then go bug LG to open/upgrade their device on Twitter. The Alcatel One Touch Fire can be unlocked but this type of instruction is beyond this book.

W>Notice: This [bug request #1001590](https://bugzilla.mozilla.org/show_bug.cgi?id=1001590) on bugzilla will fix the current problem of not being able to run the Firefox OS 1.1 simulator on Firefox 29.

## Testing the app with the App Manager

Before we try our application on the simulator we'd better check out if the files are in the correct place. Your memos folder should look like this one:

![List of files used by Memos](images/originals/memos-file-list.png)

If you have a hunch that you wrote something wrong, just compare your version with the one on [the memos github repository](https://github.com/soapdog/memos-for-firefoxos) (There is also a copy of the source code in a folder called **code** on the [book repository](https://github.com/soapdog/guia-rapido-firefox-os) ).

To open the *Simulator Dashboard* go to the menu for **Tools -> Web Developer -> App Manager**.

![Where you can find the App Manager](images/originals/locate-app-manager.png)

With the App Manager open, click the **Add Packaged App** option on the **Apps tab** and browse to where you placed the memos files and select that folder.

![Adding a new app](images/originals/app-manager-add-packaged-app.png)

If everything works as expected you will see the Memos app on the list of apps.

![Memos showing on the App Manager](images/originals/app-manager-showing-memos.png)

After adding your application, click the **Start Simulator** button and run one of your installed simulators. If you haven't installed any simulator yet, I suggest you follow the instructions on screen and install them all.

With the Simulator running press the **Update** button on the memos listing on the **App Manager** to install memos on the running Simulator. After the installation the memos icon will appear at the Simulator home screen. You can just click it to run your app.


![Memos installed on the Simulator](images/originals/app-manager-updating-memos.png)

Congratulations! You created and tested your first app. It's not a complex or revolutionary app - but I hope it helped you understand the development workflow of Firefox OS. As you can see, it's not very different from standard Web development.  

Remember that whenever you alter some of the source files you need to press the **Update** button to update the copy of the app that is stored on the running Simulator.

## Testing the app on the simulator

Before we try our application on the simulator we'd better check out if the files are in the correct place. Your memos folder should look like this one:

![List of files used by Memos](images/originals/memos-file-list.png)

If you have a hunch that you wrote something wrong, just compare your version with the one on [the memos github repository](https://github.com/soapdog/memos-for-firefoxos) (There is also a copy of the source code in a folder called **code** on the [book repository](https://github.com/soapdog/guia-rapido-firefox-os) ).

To open the *Simulator Dashboard* go to the menu for **Tools -> Web Developer -> Firefox OS Simulator**.

![How to open simulator dashboard](images/originals/tools-web-developer-simulator.png)

With the dashboard open, click the **Add Directory** button and browse to where you placed the memos files and select the app manifest.

![Adding a new app](images/originals/simulator-add-directory.png)

If everything works as expected you will see the Memos app on the list of apps.

![Memos showing on the dashboard](images/originals/memos-on-dashboard-display.png)

When you add a new application, the simulator will launch with your new app running - allowing you to test it. Now you can test all the features for Memos.

Congratulations! You created and tested your first app. It's not a complex or revolutionary app - but I hope it helped you understand the development workflow of Firefox OS. As you can see, it's not very different from standard Web development.  

Remember that whenever you alter some of the source files you need to press the **Refresh** button to update the copy of the app that is stored on the simulator.

## Summary

In this chapter we built our first application for Firefox OS and saw it running on the simulator. In the next chapter we're going to check out the developer tools that comes bundled with Firefox, they will make your life a lot easier when developing applications.
