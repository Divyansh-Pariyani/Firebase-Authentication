# Firebase-Authentication

<h2>Firebase Configuration</h2>
<p>Head over to Firebase Console and create a new project. The user can choose an existing project as well. Once you create a new project you will redirect to a page where you would get the JavaScript code of Firebase. Click on a code icon which will open a popup.</p>

<img src = "https://i0.wp.com/artisansweb.net/wp-content/uploads/2019/03/code-icon.png?resize=1018%2C433&ssl=1">
<img src = "https://i0.wp.com/artisansweb.net/wp-content/uploads/2019/03/add-firebase-sdk.png?w=622&ssl=1">

Copy the code shown in the popup which requires in the next steps. From the left side menu, click on ‘Database’ and then on ‘Create database’ under the Realtime Database section.

It will open a popup, choose test mode, and click on the ‘Enable’ button.

<h2>Connect Firebase Realtime Database to Your Website Form</h2>
We are done with the Firebase setup. Next, build a form with a few fields. For instance, create the index.html and add the code below to it.

```

!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Firebase</title>
</head>
<body>
    <form method="post" id="frmContact">
        <p>
            Fullname: <input type="text" name="fullname" id="fullname" required />
        </p>
        <p>
            Email: <input type="email" name="email" id="email" required />
        </p>
        <p>
            Subject: <input type="text" name="subject" id="subject" />
        </p>
        <p>
            Message: <textarea name="message" id="message"></textarea>
        </p>
        <button type="submit" name="submit">Submit</button>
    </form>
</body>
</html>
```

Now to interact with the Firebase realtime database, you need to add SDK for the Firebase database. I am going to perform the following steps.

<li>Import SDK of Firebase app and database.</li>
<li>Configure and Initialize the Firebase.</li>
<li>Get a reference of database.</li>
<li>On form submission, send it’s data to the realtime database.</li>
Add the below JavaScript before the closing of the body tag.

```
<script type="module">
// Import the functions you need from the SDKs you need
import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.11/firebase-app.js";
import { getDatabase, ref, set, get, child } from "https://www.gstatic.com/firebasejs/9.6.11/firebase-database.js";
 
// Paste the code from Firebase
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "Your_AuthDomain",
    databaseURL: "YOUR_DATABAE_URL",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_STORAGE_BUCKET",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};
 
// Initialize Firebase
const app = initializeApp(firebaseConfig);
 
// Get a reference to the database service
const db = getDatabase(app);
 
document.getElementById('frmContact').addEventListener('submit', function(e) {
    e.preventDefault();
    set(ref(db, 'users/' + Math.random().toString(36).slice(2, 7)), {
        name: document.getElementById('fullname').value,
        email: document.getElementById('email').value,
        subject: document.getElementById('subject').value,
        message: document.getElementById('message').value
    });
 
    alert('Your form is submitted');
    document.getElementById('frmContact').reset();
});
</script>
```
Here, I am using ‘users’ as a reference and inside it, I am generating a child with a random string of 5 characters. If you didn’t create a child for ‘users’ reference, each time your data will replace the old one. By creating a random string, all submissions will be stored separately without overriding the previus one.

Now, try to submit a form with dummy values and head over to your Firebase realtime database, you should see your data stored in the database. It will look like the below.

<img src = "https://i0.wp.com/artisansweb.net/wp-content/uploads/2019/03/firebase-database-records.png?w=796&ssl=1">
