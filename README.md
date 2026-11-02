# DLA_Web
This project builds a student results system using one Google Sheet as the database. A Google Apps Script turns the sheet into a JSON feed. A responsive website (HTML, CSS, JavaScript) and an Android app (Flutter) both read this feed, offering search, sorting and detail views. Editing a row in the sheet updates both after a refresh.
# Student Results Website and Android App

A student results system that uses one Google Sheet as the database. A Google Apps Script turns the sheet into a JSON feed. A responsive website (HTML, CSS, JavaScript) and an Android app (Flutter) both read this feed, offering search, sorting and detail views. Editing a row in the sheet updates both after a refresh.

## How it works

```
Google Sheet --> Apps Script (doGet) --> JSON URL --+--> Website (fetch)
                                                    +--> Android app (http.get)
```

| Component | Role |
|---|---|
| Google Sheet | Stores the student records (single source of data) |
| Apps Script web app | Reads the sheet and returns the rows as JSON through a URL |
| Website (HTML, CSS, JS) | Fetches the JSON, shows a searchable and sortable table |
| Android app (Flutter) | Fetches the same JSON, shows a student list and detail screen |

## Project files

```
student-website/
  index.html     Page structure (search box, table)
  style.css      Styling and responsive card layout for phones
  script.js      Fetches the JSON, handles search and sorting
  students.csv   Sample data (10 students) to import into the sheet
```

## Features

- Search by roll number or student name (case-insensitive)
- Sort by any column; click a header again to reverse the order
- Responsive layout: the table becomes stacked cards on screens up to 600 px wide
- Live data: change the sheet and refresh the page to see the update

## Setup

### 1. Prepare the Google Sheet

1. Create a blank Google Sheet named **Student Records**.
2. Import `students.csv` via **File > Import > Upload > Replace current sheet**.
3. Make sure row 1 has these headers in columns A to G: `Sr No, Roll No, Student Name, 10th, 12th, Sem 1, Sem 2`.
4. Keep the data on the first tab, with no merged cells, blank rows or titles above the header.
5. Marks must be plain numbers (for example `85.5`, not `85.5%` as text).

### 2. Create the data feed (Apps Script)

1. In the sheet, open **Extensions > Apps Script** and paste the `doGet()` function from the activity guide into `Code.gs`. It reads the first sheet, drops the header row and returns each student as JSON with the keys `srNo`, `rollNo`, `name`, `tenth`, `twelfth`, `sem1`, `sem2`.
2. Click **Deploy > New deployment**, choose **Web app**, set **Execute as: Me** and **Who has access: Anyone**, then deploy and authorize.
3. Copy the Web app URL (it ends with `/exec`) and open it in a browser. You should see the data as JSON.

> If you change the Apps Script code later, redeploy with **Deploy > Manage deployments > Edit > New version**. Changing only the sheet data needs no redeployment.

### 3. Run the website

1. Open `script.js` and replace `PASTE_YOUR_WEB_APP_URL_HERE` with your Web app URL.
2. Open `index.html` in a browser. If the browser blocks the request, use the VS Code **Live Server** extension.

### 4. Host the website (free options)

- **GitHub Pages:** upload the three files to a public repository, then enable **Settings > Pages** on the `main` branch.
- **Netlify:** drag the `student-website` folder onto the Netlify Sites page.
- **Firebase Hosting:** run `firebase init hosting` and `firebase deploy`, using `.` as the public directory.

## Android app (Flutter)

The app reuses the same JSON feed, so no changes to the sheet or script are needed.

1. Install Android Studio and the Flutter SDK, then run `flutter doctor` and fix any issues.
2. Create the project: `flutter create student_app`, then `cd student_app`.
3. Add the HTTP package: `flutter pub add http`.
4. Add `<uses-permission android:name="android.permission.INTERNET"/>` to `android/app/src/main/AndroidManifest.xml`, above the `<application>` tag.
5. Add the model, API service, list screen (with search and pull-to-refresh), detail screen and `main.dart` from the activity guide, and set `apiUrl` to your Web app URL.
6. Run on an emulator or phone with `flutter run`.
7. Build a shareable APK with `flutter build apk --release`. The file is created at `build/app/outputs/flutter-apk/app-release.apk`.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Could not load data" | Check the URL in `script.js`. It must end with `/exec`, and **Who has access** must be **Anyone**. |
| Blank table, no error | Confirm the data is on the first sheet and the header row is row 1. |
| Old data still showing | Hard refresh with Ctrl + Shift + R. If you changed the script code, redeploy a new version. |
| Authorization error | Redeploy and complete the **Authorize access** step again. |

## Final checklist

- [ ] Sheet has one header row, one student per row, no merged cells
- [ ] Apps Script deployed as a Web app (Anyone) and URL copied
- [ ] Opening the URL in a browser shows JSON
- [ ] URL pasted into `script.js`
- [ ] Search and sorting work; layout is responsive on mobile
- [ ] Website hosted on GitHub Pages, Netlify or Firebase
- [ ] Sheet edit appears on the website after refresh
- [ ] Flutter app shows the list, search and detail screens
- [ ] APK built and installed

## Credits

Based on *Digital Literacy and Application, Unit 6, Activity 6: Student Results Website and Android Mobile App*.
