## **Actor Documentation**
### Primary Actors
### **Student**

- **Description**: An individual enrolled in the institution, capable of accessing personal academic information and performing analyses to plan their academic journey.
- **Type**: Primary Actor
- **Relationships**: N/A
- **Roles**: Access personal records, perform 'What-If' analyses, view GPA, transcript, and course schedules.

### **Advisor**

- **Description**: A staff member responsible for guiding students academically, including course enrollment and academic planning.
- **Type**: Primary Actor
- **Relationships**: Advises multiple students, associated with specific departments.
- **Roles**: Enroll/drop students in courses, view student GPA summaries, perform 'What-If' analyses on behalf of students.

### **Instructor**

- **Description**: A faculty member teaching courses, able to access teaching schedules and student performance data.
- **Type**: Primary Actor
- **Relationships**: Assigned to courses within a department.
- **Roles**: View teaching schedules, student performance, and major distributions in courses taught.

### **Staff**

- **Description**: Department personnel managing departmental data, including courses, instructors, and students.
- **Type**: Primary Actor
- **Relationships**: Works within a specific department.
- **Roles**: Add/modify courses and personnel, assign courses to instructors, view departmental reports.

### Secondary Actors
TODO
---

## **Use Case Documents**

### **Use Case 1: View GPA**

- **ID**: UC01
- **Priority**: High
- **Actor**: Student
- **Description**: Allows a student to view their current GPA based on completed courses.
- **Trigger**: Student selects the "View GPA" option.
- **Preconditions**:
  - Student is authenticated and logged in.
  - Student has completed at least one course with a grade.
- **Normal Flow**:
  1. Student selects "View GPA."
  2. System retrieves completed courses and grades.
  3. System calculates GPA using the standard formula.
  4. System displays the GPA.
- **Alternative Flows**:
  - **A1**: If the student hasn't completed any courses, the system informs that GPA cannot be calculated.
- **Exceptions**:
  - **E1**: If data retrieval fails, the system displays an error message.
- **Postconditions**:
  - GPA is displayed to the student.

---

### **Use Case 2: View Transcript**

- **ID**: UC02
- **Priority**: High
- **Actor**: Student
- **Description**: Allows a student to view their academic transcript, including courses taken, grades, and credits earned.
- **Trigger**: Student selects the "View Transcript" option.
- **Preconditions**:
  - Student is authenticated and logged in.
- **Normal Flow**:
  1. Student selects "View Transcript."
  2. System retrieves academic records.
  3. System displays the transcript with course details.
- **Alternative Flows**:
  - **A1**: Student selects a specific semester to view.
- **Exceptions**:
  - **E1**: If data retrieval fails, an error message is displayed.
- **Postconditions**:
  - Transcript is displayed to the student.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 3: Perform 'What-If' Analysis**

- **ID**: UC03
- **Priority**: High
- **Actor**: Student
- **Description**: Enables a student to analyze how taking additional courses or aiming for a higher GPA affects their current GPA.
- **Trigger**: Student selects the "What-If Analysis" option.
- **Preconditions**:
  - Student is authenticated and logged in.
  - Student has a current GPA.
- **Normal Flow**:
  1. Student selects "What-If Analysis."
  2. System presents two scenarios:
     - Adding N courses.
     - Targeting a new GPA.
  3. Student selects a scenario and inputs required data.
  4. System performs calculations.
  5. System displays the projected GPA or plan.
- **Alternative Flows**:
  - **A1**: Student switches between scenarios.
- **Exceptions**:
  - **E1**: Invalid input data prompts an error message.
- **Postconditions**:
  - Analysis results are displayed.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 4: Enroll Student into a Course**

- **ID**: UC04
- **Priority**: High
- **Actor**: Advisor
- **Description**: Allows an advisor to enroll a student in a course, given both belong to the same department.
- **Trigger**: Advisor selects "Enroll Student" option.
- **Preconditions**:
  - Advisor is authenticated and logged in.
  - Student and advisor belong to the same department.
  - Course exists in the system.
- **Normal Flow**:
  1. Advisor selects "Enroll Student."
  2. System prompts for student ID.
  3. Advisor enters student ID.
  4. System verifies eligibility.
  5. System prompts for course selection.
  6. Advisor selects course.
  7. System enrolls student and confirms.
- **Alternative Flows**:
  - **A1**: Student is already enrolled; system notifies the advisor.
- **Exceptions**:
  - **E1**: Student or course does not exist; error message displayed.
- **Postconditions**:
  - Student is enrolled in the course.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 5: View GPA Summary of Advisor's Students**

- **ID**: UC05
- **Priority**: Medium
- **Actor**: Advisor
- **Description**: Allows an advisor to view GPA statistics of their advisees.
- **Trigger**: Advisor selects "View GPA Summary."
- **Preconditions**:
  - Advisor is authenticated and logged in.
  - Advisor has assigned students.
- **Normal Flow**:
  1. Advisor selects "View GPA Summary."
  2. System retrieves GPA data of advisees.
  3. System calculates highest, lowest, and average GPAs.
  4. System displays the summary.
- **Alternative Flows**:
  - **A1**: No advisees assigned; system informs the advisor.
- **Exceptions**:
  - **E1**: Data retrieval error; error message displayed.
- **Postconditions**:
  - GPA summary is displayed.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 6: Perform 'What-If' Analysis for Student**

- **ID**: UC06
- **Priority**: Medium
- **Actor**: Advisor
- **Description**: Enables an advisor to perform 'What-If' analysis on behalf of a student.
- **Trigger**: Advisor selects "Perform What-If Analysis."
- **Preconditions**:
  - Advisor is authenticated and logged in.
  - Student exists and is advised by the advisor.
- **Normal Flow**:
  1. Advisor selects "Perform What-If Analysis."
  2. System prompts for student ID.
  3. Advisor enters student ID.
  4. System verifies student.
  5. Advisor selects analysis scenario and inputs data.
  6. System performs calculations.
  7. System displays results to the advisor.
- **Alternative Flows**:
  - **A1**: Advisor inputs invalid data; system prompts correction.
- **Exceptions**:
  - **E1**: Student not advised by the advisor; error message displayed.
- **Postconditions**:
  - Analysis results are available to the advisor.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 7: View Teaching Schedule**

- **ID**: UC07
- **Priority**: High
- **Actor**: Instructor
- **Description**: Allows an instructor to view their teaching schedule for selected semesters.
- **Trigger**: Instructor selects "View Teaching Schedule."
- **Preconditions**:
  - Instructor is authenticated and logged in.
- **Normal Flow**:
  1. Instructor selects "View Teaching Schedule."
  2. System prompts for semester selection.
  3. Instructor selects a semester or all semesters.
  4. System retrieves teaching assignments.
  5. System displays the schedule.
- **Alternative Flows**:
  - **A1**: No assignments found; system notifies the instructor.
- **Exceptions**:
  - **E1**: Data retrieval error; error message displayed.
- **Postconditions**:
  - Teaching schedule is displayed.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 8: View Student Performance in a Course**

- **ID**: UC08
- **Priority**: Medium
- **Actor**: Instructor
- **Description**: Allows an instructor to view grades of students in a course they teach.
- **Trigger**: Instructor selects "View Student Performance."
- **Preconditions**:
  - Instructor is authenticated and logged in.
  - Instructor teaches the course.
- **Normal Flow**:
  1. Instructor selects "View Student Performance."
  2. System prompts for course selection.
  3. Instructor selects a course.
  4. System retrieves student grades.
  5. System displays performance data.
- **Alternative Flows**:
  - **A1**: No students enrolled; system notifies the instructor.
- **Exceptions**:
  - **E1**: Instructor not assigned to course; error message displayed.
- **Postconditions**:
  - Student performance is displayed.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 9: View Major Distribution in Courses Taught**

- **ID**: UC09
- **Priority**: Medium
- **Actor**: Instructor
- **Description**: Enables an instructor to see the distribution of student majors in their courses.
- **Trigger**: Instructor selects "View Major Distribution."
- **Preconditions**:
  - Instructor is authenticated and logged in.
- **Normal Flow**:
  1. Instructor selects "View Major Distribution."
  2. System retrieves enrollment data.
  3. System calculates distribution by major.
  4. System displays the distribution.
- **Alternative Flows**:
  - **A1**: No enrollment data; system informs the instructor.
- **Exceptions**:
  - **E1**: Data retrieval error; error message displayed.
- **Postconditions**:
  - Major distribution is displayed.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 10: Assign Course to Instructor**

- **ID**: UC10
- **Priority**: High
- **Actor**: Staff
- **Description**: Allows staff to assign a course to an instructor within the same department.
- **Trigger**: Staff selects "Assign Course to Instructor."
- **Preconditions**:
  - Staff is authenticated and logged in.
  - Course and instructor belong to the same department.
- **Normal Flow**:
  1. Staff selects "Assign Course to Instructor."
  2. System prompts for course selection.
  3. Staff selects a course.
  4. System prompts for instructor selection.
  5. Staff selects an instructor.
  6. System checks for conflicts.
  7. System assigns course and confirms.
- **Alternative Flows**:
  - **A1**: Instructor's schedule is full; system notifies the staff.
- **Exceptions**:
  - **E1**: Course or instructor not found; error message displayed.
- **Postconditions**:
  - Course is assigned to the instructor.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 11: Add New Student**

- **ID**: UC11
- **Priority**: High
- **Actor**: Staff
- **Description**: Enables staff to add a new student to the system, ensuring no duplication.
- **Trigger**: Staff selects "Add New Student."
- **Preconditions**:
  - Staff is authenticated and logged in.
- **Normal Flow**:
  1. Staff selects "Add New Student."
  2. System presents input form.
  3. Staff enters student details.
  4. System validates data and checks for duplicates.
  5. System adds student and confirms.
- **Alternative Flows**:
  - **A1**: Duplicate student found; system prevents addition.
- **Exceptions**:
  - **E1**: Invalid data; system prompts for correction.
- **Postconditions**:
  - New student is added to the system.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]

---

### **Use Case 12: View Department Summary Reports**

- **ID**: UC12
- **Priority**: Medium
- **Actor**: Staff
- **Description**: Allows staff to view summary reports specific to their department.
- **Trigger**: Staff selects "View Department Reports."
- **Preconditions**:
  - Staff is authenticated and logged in.
- **Normal Flow**:
  1. Staff selects "View Department Reports."
  2. System presents report options (GPA summaries, course enrollments, etc.).
  3. Staff selects a report.
  4. System retrieves and displays the report data.
- **Alternative Flows**:
  - **A1**: No data available; system informs the staff.
- **Exceptions**:
  - **E1**: Data retrieval error; error message displayed.
- **Postconditions**:
  - Selected report is displayed.
- **Author & History**:
  - **Author**: [Your Name]
  - **Date**: [Creation Date]