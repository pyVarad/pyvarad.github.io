---
layout: post
title: College Management System
mermaid: true
---

# 📚 College Management System – Initial Draft

## 🚀 Overview

The College Management System (CMS) is designed to manage academic, administrative, and community activities within a college environment. The system supports multiple user roles, detailed profiling, academic structuring, and operational features such as attendance and assessments.

---

## 🧑‍💼 **User Domain**

### **Description**
The `User` domain represents all users of the system. Each user can assume one or more roles:
- **Students**
- **Teachers**
- **Head of the Departments (HOD)**
- **Administrators**
- **Parents**

### **Attributes**
- `UserID`
- `Username`
- `Password`
- `Email`
- `Mobile`
- `Role` (Enum: Student, Teacher, HOD, Admin, Parent)
- `Status`

### **API Scope**
- Create/Update/Delete Users
- Assign Roles to Users
- Parent-Student Associations

---

## 🧑‍🎓 **Student Profile**

### **Description**
Manages additional profile information for each student, offering a holistic view of their academic and non-academic involvement.

### **Attributes**
- `UserID` (FK)
- `EnrollmentNumber`
- `CourseID` (FK)
- `YearOfAdmission`
- `ProfileNote`
- **Contributions**:
  - Sports
  - Student Community/Group Membership
  - Cultural Participation

---

## 👨‍🏫 **Teacher Profile**

### **Description**
Holds detailed academic and professional background for teachers.

### **Attributes**
- `UserID` (FK)
- `Qualification`
- `Specialization`
- `WhitePapersPublished`
- `ExperienceSummary`

---

## 🎓 **Course Domain (Discipline)**

### **Description**
Defines the primary academic programs offered by the institution.

### **Sample Courses**
- B.Tech Electrical & Electronics
- B.Tech Mechanical Engineering
- B.Tech Computer Science Engineering
- B.Tech Electronics and Communication Engineering

### **Attributes**
- `CourseID`
- `CourseName`
- `Duration`

---

## 📅 **Semester**

### **Description**
Represents the academic periods within a course.

### **Attributes**
- `SemesterID`
- `CourseID` (FK)
- `SemesterNumber`
- `Year`

---

## 📖 **Subject**

### **Description**
Lists subjects taught within a semester and their specifics.

### **Attributes**
- `SubjectID`
- `SubjectName`
- `Weightage`
- `CourseID` (FK)
- `SemesterID` (FK)

### **Modules**
Each subject is subdivided into **5 Modules**:
- `ModuleID`
- `SubjectID` (FK)
- `ModuleNumber`
- `ModuleName`
- `ModuleDescription`

---

## 🔗 **Associations**

- **SubjectTeacherAssociation**: Links teachers to subjects.
- **StudentSubjectEnrollment**: Links students to subjects.

---

## 📝 **Assessment**

### **Description**
Records evaluation scores for students in various subjects.

### **Attributes**
- `AssessmentID`
- `SubjectID` (FK)
- `StudentID` (FK)
- `AssessmentType`
- `Score`
- `MaxScore`
- `Date`

---

## 📊 **Attendance**

### **Description**
Tracks attendance records for students on a subject basis.

### **Attributes**
- `AttendanceID`
- `SubjectID` (FK)
- `StudentID` (FK)
- `TeacherID` (FK)
- `Date`
- `Status` (Present/Absent)

---

# 🛠️ **API Overview**

| **API**                            | **Purpose**                                                      |
|------------------------------------|------------------------------------------------------------------|
| `POST /users`                      | Create a new user                                                |
| `POST /roles`                      | Assign a role to a user                                          |
| `GET /students/{id}/profile`       | Retrieve student profile                                         |
| `POST /students/{id}/profile`      | Update student profile                                          |
| `GET /teachers/{id}/profile`       | Retrieve teacher profile                                         |
| `POST /teachers/{id}/profile`      | Update teacher profile                                          |
| `GET /courses`                     | List all courses                                                 |
| `POST /courses`                    | Add a new course                                                 |
| `GET /subjects`                    | List all subjects                                                |
| `POST /subjects`                   | Add a new subject                                                |
| `POST /attendance`                 | Mark attendance                                                 |
| `GET /attendance/{student_id}`     | View attendance                                                 |
| `POST /assessments`                | Add assessment data                                             |
| `GET /assessments/{student_id}`    | View assessment scores                                          |

---

# 🗺️ **Entity Relationship Diagram **

<div class="mermaid">
erDiagram
    USER {
        int UserID PK
        string Username
        string Password
        string Email
        string Mobile
        string Role
    }
    STUDENT_PROFILE {
        int UserID PK, FK
        string EnrollmentNumber
        int CourseID FK
        string YearOfAdmission
        string ProfileNote
    }
    TEACHER_PROFILE {
        int UserID PK, FK
        string Qualification
        string Specialization
        string WhitePapers
        string ExperienceSummary
    }
    COURSE {
        int CourseID PK
        string CourseName
        string Duration
    }
    SEMESTER {
        int SemesterID PK
        int CourseID FK
        int SemesterNumber
        string Year
    }
    SUBJECT {
        int SubjectID PK
        string SubjectName
        string Weightage
        int CourseID FK
        int SemesterID FK
    }
    MODULE {
        int ModuleID PK
        int SubjectID FK
        int ModuleNumber
        string ModuleName
        string ModuleDescription
    }
    SUBJECT_TEACHER_ASSOCIATION {
        int ID PK
        int SubjectID FK
        int TeacherID FK
    }
    STUDENT_SUBJECT_ENROLLMENT {
        int ID PK
        int StudentID FK
        int SubjectID FK
    }
    ASSESSMENT {
        int AssessmentID PK
        int SubjectID FK
        int StudentID FK
        string AssessmentType
        float Score
        float MaxScore
        string Date
    }
    ATTENDANCE {
        int AttendanceID PK
        int SubjectID FK
        int StudentID FK
        int TeacherID FK
        string Date
        string Status
    }

    USER ||--o{ STUDENT_PROFILE : has
    USER ||--o{ TEACHER_PROFILE : has
    COURSE ||--o{ SEMESTER : contains
    COURSE ||--o{ SUBJECT : offers
    SEMESTER ||--o{ SUBJECT : includes
    SUBJECT ||--o{ MODULE : consists_of
    SUBJECT ||--o{ SUBJECT_TEACHER_ASSOCIATION : assigned_to
    SUBJECT ||--o{ STUDENT_SUBJECT_ENROLLMENT : enrolled_by
    STUDENT_PROFILE ||--o{ STUDENT_SUBJECT_ENROLLMENT : enrolled
    STUDENT_PROFILE ||--o{ ASSESSMENT : assessed_in
    STUDENT_PROFILE ||--o{ ATTENDANCE : attends
    TEACHER_PROFILE ||--o{ SUBJECT_TEACHER_ASSOCIATION : teaches
    TEACHER_PROFILE ||--o{ ATTENDANCE : records
</div>