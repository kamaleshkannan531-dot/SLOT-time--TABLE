# Ex 04 - Slot Time Table

## Date: 05-11-2026
## Name: V Kamalesh Kannan
## Register Number: 26011247
## Reference Number: 212224010001
## Course Code: 19AI414
## Course Title: Fundamentals of Web Application Development
## Department: Artificial Intelligence and Data Science
## Institution: Saveetha Engineering College (Autonomous)

---

## AIM:
To write an HTML webpage and deploy a Django web application to display the semester slot timetable.

---

## ALGORITHM:
- **STEP 1:** Create a Django project using `django-admin startproject timetable_project .`
- **STEP 2:** Create an application inside the Django project using `python manage.py startapp timetable_app`.
- **STEP 3:** Register `timetable_app` in `INSTALLED_APPS` and configure the template directory `'DIRS': [BASE_DIR / 'templates']` in `timetable_project/settings.py`.
- **STEP 4:** Create a templates directory and design the timetable layout using HTML table tags (`<table>`, `<tr>`, `<th>`, `<td>`), with `rowspan` for tea and lunch breaks and `colspan` for lab sessions.
- **STEP 5:** Create a view function `slot_timetable(request)` in `timetable_app/views.py` to render `slot_timetable.html` and configure the URL patterns in `timetable_app/urls.py` and `timetable_project/urls.py`.
- **STEP 6:** Execute the program using `python manage.py runserver` command and verify the output at `http://127.0.0.1:8000/`.

---

## PROGRAM:

### 1. `timetable_project/urls.py`
```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('timetable_app.urls')),
]
```

### 2. `timetable_app/urls.py`
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.slot_timetable, name='slot_timetable'),
    path('timetable/', views.slot_timetable, name='timetable_alias'),
]
```

### 3. `timetable_app/views.py`
```python
from django.shortcuts import render

def slot_timetable(request):
    """
    Renders the Semester Slot Timetable web page
    for 19AI414 - Fundamentals of Web Application Development.
    """
    return render(request, 'slot_timetable.html')
```

### 4. `templates/slot_timetable.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Slot Time Table - V Kamalesh Kannan (26011247)</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #eef2f7;
            color: #333;
            padding: 25px;
            display: flex;
            justify-content: center;
        }
        .container {
            background-color: #ffffff;
            width: 100%;
            max-width: 1200px;
            padding: 30px;
            border-radius: 12px;
            box-shadow: 0 10px 25px rgba(0, 0, 0, 0.08);
            border: 1px solid #dde3ea;
        }
        .college-header {
            text-align: center;
            border-bottom: 3px double #003366;
            padding-bottom: 15px;
            margin-bottom: 25px;
        }
        .college-header h1 {
            color: #003366;
            font-size: 26px;
            text-transform: uppercase;
            letter-spacing: 1.5px;
            margin-bottom: 5px;
        }
        .college-header h2 {
            color: #d97706;
            font-size: 16px;
            font-weight: 600;
            letter-spacing: 1px;
            text-transform: uppercase;
            margin-bottom: 8px;
        }
        .college-header h3 {
            color: #2c3e50;
            font-size: 18px;
            margin-top: 5px;
        }
        .student-info {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            background: #f8fafc;
            border: 1px solid #e2e8f0;
            border-left: 5px solid #003366;
            border-radius: 6px;
            padding: 12px 20px;
            margin-bottom: 25px;
            font-size: 15px;
        }
        .student-info strong {
            color: #003366;
        }
        .section-title {
            color: #003366;
            font-size: 18px;
            margin-bottom: 12px;
        }
        .table-responsive {
            overflow-x: auto;
            margin-bottom: 30px;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            text-align: center;
            font-size: 13.5px;
        }
        th, td {
            border: 1px solid #cbd5e1;
            padding: 10px 8px;
        }
        th {
            background-color: #003366;
            color: #ffffff;
            font-weight: 600;
            text-transform: uppercase;
            font-size: 13px;
        }
        .timetable-table th.time-header {
            background-color: #1e40af;
            font-size: 12px;
        }
        .day-column {
            background-color: #0f172a;
            color: #ffffff;
            font-weight: bold;
            text-transform: uppercase;
            width: 110px;
        }
        tr:nth-child(even) td:not(.break):not(.lunch):not(.day-column) {
            background-color: #f8fafc;
        }
        tr:hover td:not(.break):not(.lunch):not(.day-column) {
            background-color: #f1f5f9;
        }
        .break, .lunch {
            background-color: #fef3c7 !important;
            color: #92400e;
            font-weight: 800;
            font-size: 12px;
            letter-spacing: 2px;
            line-height: 1.8;
            width: 35px;
            text-transform: uppercase;
            border-color: #fcd34d;
        }
        .course-code {
            font-weight: 700;
            color: #0f172a;
            display: block;
        }
        .course-name {
            font-size: 11px;
            color: #475569;
        }
        .lab-slot {
            background-color: #e0f2fe !important;
            color: #0369a1;
            font-weight: 600;
        }
        .special-slot {
            background-color: #f3e8ff !important;
            color: #6b21a8;
            font-weight: 600;
        }
        .subject-table th {
            background-color: #334155;
        }
        .subject-table td.sno {
            font-weight: bold;
            width: 70px;
        }
        .subject-table td.code {
            font-weight: 700;
            color: #003366;
            width: 150px;
        }
        .subject-table td.name {
            text-align: left;
            padding-left: 15px;
        }
        .footer {
            text-align: center;
            margin-top: 25px;
            padding-top: 15px;
            border-top: 1px solid #e2e8f0;
            font-size: 13px;
            color: #64748b;
        }
    </style>
</head>
<body>

<div class="container">
    <header class="college-header">
        <h1>Saveetha Engineering College</h1>
        <h2>Autonomous Institution • Affiliated to Anna University</h2>
        <h3>DEPARTMENT OF ARTIFICIAL INTELLIGENCE AND DATA SCIENCE</h3>
    </header>

    <div class="student-info">
        <span><strong>Student Name:</strong> V Kamalesh Kannan</span>
        <span><strong>Register No:</strong> 26011247</span>
        <span><strong>Reference No:</strong> 212224010001</span>
        <span><strong>Semester:</strong> IV (2025 - 2026)</span>
    </div>

    <h4 class="section-title">📅 Semester Slot Timetable</h4>
    <div class="table-responsive">
        <table class="timetable-table">
            <thead>
                <tr>
                    <th rowspan="2">Day</th>
                    <th class="time-header">08:30 - 09:30</th>
                    <th class="time-header">09:30 - 10:30</th>
                    <th rowspan="2" class="break">B<br>R<br>E<br>A<br>K</th>
                    <th class="time-header">10:50 - 11:50</th>
                    <th class="time-header">11:50 - 12:50</th>
                    <th rowspan="2" class="lunch">L<br>U<br>N<br>C<br>H</th>
                    <th class="time-header">01:40 - 02:40</th>
                    <th class="time-header">02:40 - 03:30</th>
                    <th class="time-header">03:30 - 04:15</th>
                </tr>
                <tr>
                    <th>1</th>
                    <th>2</th>
                    <th>3</th>
                    <th>4</th>
                    <th>5</th>
                    <th>6</th>
                    <th>7</th>
                </tr>
            </thead>
            <tbody>
                <!-- Monday -->
                <tr>
                    <td class="day-column">Monday</td>
                    <td>
                        <span class="course-code">19AI414</span>
                        <span class="course-name">FWAD</span>
                    </td>
                    <td>
                        <span class="course-code">19AI301</span>
                        <span class="course-name">Python</span>
                    </td>
                    <td rowspan="6" class="break">T<br>E<br>A<br><br>B<br>R<br>E<br>A<br>K</td>
                    <td>
                        <span class="course-code">19AI302</span>
                        <span class="course-name">Maths</span>
                    </td>
                    <td>
                        <span class="course-code">19CS405</span>
                        <span class="course-name">OS</span>
                    </td>
                    <td rowspan="6" class="lunch">L<br>U<br>N<br>C<br>H<br><br>B<br>R<br>E<br>A<br>K</td>
                    <td colspan="2" class="lab-slot">
                        <span class="course-code">19AI414</span>
                        <span class="course-name">Web App Lab</span>
                    </td>
                    <td>Library</td>
                </tr>

                <!-- Tuesday -->
                <tr>
                    <td class="day-column">Tuesday</td>
                    <td>
                        <span class="course-code">19CS406</span>
                        <span class="course-name">DBMS</span>
                    </td>
                    <td>
                        <span class="course-code">19AI414</span>
                        <span class="course-name">FWAD</span>
                    </td>
                    <td>
                        <span class="course-code">19AI301</span>
                        <span class="course-name">Python</span>
                    </td>
                    <td>
                        <span class="course-code">19AI302</span>
                        <span class="course-name">Maths</span>
                    </td>
                    <td colspan="2" class="lab-slot">
                        <span class="course-code">19CS406</span>
                        <span class="course-name">DBMS Lab</span>
                    </td>
                    <td>Sports</td>
                </tr>

                <!-- Wednesday -->
                <tr>
                    <td class="day-column">Wednesday</td>
                    <td>
                        <span class="course-code">19AI302</span>
                        <span class="course-name">Maths</span>
                    </td>
                    <td>
                        <span class="course-code">19CS405</span>
                        <span class="course-name">OS</span>
                    </td>
                    <td>
                        <span class="course-code">19AI414</span>
                        <span class="course-name">FWAD</span>
                    </td>
                    <td>
                        <span class="course-code">19CS406</span>
                        <span class="course-name">DBMS</span>
                    </td>
                    <td class="special-slot">Mentor Meet</td>
                    <td>
                        <span class="course-code">19AI301</span>
                        <span class="course-name">Python</span>
                    </td>
                    <td>Self Study</td>
                </tr>

                <!-- Thursday -->
                <tr>
                    <td class="day-column">Thursday</td>
                    <td>
                        <span class="course-code">19AI301</span>
                        <span class="course-name">Python</span>
                    </td>
                    <td>
                        <span class="course-code">19AI302</span>
                        <span class="course-name">Maths</span>
                    </td>
                    <td>
                        <span class="course-code">19CS405</span>
                        <span class="course-name">OS</span>
                    </td>
                    <td>
                        <span class="course-code">19AI414</span>
                        <span class="course-name">FWAD</span>
                    </td>
                    <td colspan="2" class="lab-slot">
                        <span class="course-code">19AI301</span>
                        <span class="course-name">Python Lab</span>
                    </td>
                    <td>Seminar</td>
                </tr>

                <!-- Friday -->
                <tr>
                    <td class="day-column">Friday</td>
                    <td>
                        <span class="course-code">19CS405</span>
                        <span class="course-name">OS</span>
                    </td>
                    <td>
                        <span class="course-code">19CS406</span>
                        <span class="course-name">DBMS</span>
                    </td>
                    <td>
                        <span class="course-code">19AI302</span>
                        <span class="course-name">Maths</span>
                    </td>
                    <td>
                        <span class="course-code">19AI301</span>
                        <span class="course-name">Python</span>
                    </td>
                    <td>
                        <span class="course-code">19AI414</span>
                        <span class="course-name">FWAD</span>
                    </td>
                    <td class="special-slot">Aptitude</td>
                    <td>Club Activity</td>
                </tr>

                <!-- Saturday -->
                <tr>
                    <td class="day-column">Saturday</td>
                    <td>
                        <span class="course-code">19AI414</span>
                        <span class="course-name">FWAD</span>
                    </td>
                    <td>
                        <span class="course-code">19CS406</span>
                        <span class="course-name">DBMS</span>
                    </td>
                    <td>
                        <span class="course-code">19CS405</span>
                        <span class="course-name">OS</span>
                    </td>
                    <td>
                        <span class="course-code">19AI301</span>
                        <span class="course-name">Python</span>
                    </td>
                    <td colspan="3" class="special-slot">Mini Project Review / Presentation</td>
                </tr>
            </tbody>
        </table>
    </div>

    <h4 class="section-title">📚 Course Details</h4>
    <div class="table-responsive">
        <table class="subject-table">
            <thead>
                <tr>
                    <th style="width: 80px;">S.No</th>
                    <th style="width: 160px;">Course Code</th>
                    <th>Course Title</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td class="sno">1</td>
                    <td class="code">19AI414</td>
                    <td class="name">Fundamentals of Web Application Development (FWAD)</td>
                </tr>
                <tr>
                    <td class="sno">2</td>
                    <td class="code">19AI301</td>
                    <td class="name">Python Programming and Practice</td>
                </tr>
                <tr>
                    <td class="sno">3</td>
                    <td class="code">19AI302</td>
                    <td class="name">Engineering Mathematics and Discrete Structures</td>
                </tr>
                <tr>
                    <td class="sno">4</td>
                    <td class="code">19CS405</td>
                    <td class="name">Operating System Concepts and Design</td>
                </tr>
                <tr>
                    <td class="sno">5</td>
                    <td class="code">19CS406</td>
                    <td class="name">Database Management Systems (DBMS)</td>
                </tr>
                <tr>
                    <td class="sno">6</td>
                    <td class="code">19EY708</td>
                    <td class="name">Soft Skills and Professional Aptitude</td>
                </tr>
            </tbody>
        </table>
    </div>

    <footer class="footer">
        <p>19AI414 - Fundamentals of Web Application Development | Ex 04 - Slot Time Table | V Kamalesh Kannan (26011247)</p>
    </footer>
</div>

</body>
</html>
```

---

## OUTPUT:
The timetable web page is rendered at `http://127.0.0.1:8000/`:

```text
==========================================================================================
                              SAVEETHA ENGINEERING COLLEGE
                       AUTONOMOUS INSTITUTION • AFFILIATED TO ANNA UNIVERSITY
                  DEPARTMENT OF ARTIFICIAL INTELLIGENCE AND DATA SCIENCE
------------------------------------------------------------------------------------------
Student Name: V Kamalesh Kannan   | Register No: 26011247   | Reference No: 212224010001
==========================================================================================
Day        08:30-09:30   09:30-10:30   BREAK   10:50-11:50   11:50-12:50   LUNCH   01:40-02:40   02:40-03:30   03:30-04:15
Monday     19AI414       19AI301       TEA     19AI302       19CS405       LUNCH   [--- 19AI414 LAB ---]        Library
Tuesday    19CS406       19AI414       TEA     19AI301       19AI302       LUNCH   [--- 19CS406 LAB ---]        Sports
Wednesday  19AI302       19CS405       TEA     19AI414       19CS406       LUNCH   Mentor Meet   19AI301        Self Study
Thursday   19AI301       19AI302       TEA     19CS405       19AI414       LUNCH   [--- 19AI301 LAB ---]        Seminar
Friday     19CS405       19CS406       TEA     19AI302       19AI301       LUNCH   19AI414       Aptitude       Club
Saturday   19AI414       19CS406       TEA     19CS405       19AI301       LUNCH   [--- Mini Project Review ---]
==========================================================================================
Course Code Mapping:
1. 19AI414 - Fundamentals of Web Application Development (FWAD)
2. 19AI301 - Python Programming and Practice
3. 19AI302 - Engineering Mathematics and Discrete Structures
4. 19CS405 - Operating System Concepts and Design
5. 19CS406 - Database Management Systems (DBMS)
6. 19EY708 - Soft Skills and Professional Aptitude
==========================================================================================
```

---

## RESULT:
Thus, the website to display the semester slot timetable using Django Web Framework, HTML, and CSS has been successfully developed, integrated, and verified in the browser.
