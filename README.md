# Hospital_DB_Management_System
___
<img src="https://blog.decos.com/hubfs/Hero%20blog%20data%20analytics.png#keepProtocol" alt="Image" width="1000" height="400">


## Abstract

The Hospital Management System is a comprehensive software solution designed to automate and digitize the operations of a healthcare facility. The system includes modules for managing patient entries, doctor appointments, diagnosis and treatment records, patient admissions and discharges, and financial transactions. It consists of several interconnected tables storing relevant data, allowing hospital staff to easily access and update patient records, schedule appointments, assign doctors to specific departments, and manage financial transactions.

## Objective

The objective of the Hospital Management System project is to create an efficient and organized system to manage various aspects of a hospital, including patient information, doctor schedules, departmental operations, and financial transactions. The system aims to streamline hospital processes, improve patient care, enhance communication between staff members, and facilitate accurate record-keeping for administrative purposes.

## Purpose

The purpose of the Hospital Management System project is to streamline and digitize hospital operations, thereby improving efficiency, reducing errors, and enhancing patient care. It incorporates modules for managing patient records, doctor appointments, and financial transactions. By centralizing these processes, it provides valuable insights for administrators to optimize resource utilization and hospital performance.

## Requirements 
- mysql-connector-python==8.0.27
- pandas==1.3.4
- matplotlib==3.4.3
- seaborn==0.11.2
- numpy==1.21.2
- gapminder==0.1

## Features and Functionalities

### Patient Management
- **Patient Entries**: Manage patient registration and store patient details.
- **Patient Admissions**: Track patient admissions and discharges.
- **Diagnosis and Treatment**: Record patient diagnosis and treatment details.

### Doctor Management
- **Doctor Appointments**: Schedule and manage doctor appointments.
- **Doctor Information**: Store doctor details, including specializations and departmental assignments.
- **Doctor Schedules**: Manage doctor schedules and duty assignments.

### Department Management
- **Departmental Operations**: Track departmental operations and assignments.
- **Medical Procedures**: Record and track medical procedures performed in each department.

### Financial Management
- **Financial Transactions**: Manage billing and payment transactions.
- **Payment Methods**: Track various payment methods used by patients.

## Data Analysis and Visualization

### Doctor Specializations
- Query to get the distribution of doctor specializations and visualize it using a pie chart.

\`\`\`python
query1 = "SELECT SPECIALIZATION, COUNT(*) as COUNT FROM DOCTORS GROUP BY SPECIALIZATION"
mycursor.execute(query1)
results1 = mycursor.fetchall()
df1 = pd.DataFrame(results1, columns=["Specialization", "Count"])
plt.figure(figsize=(10,8))
plt.title("Distribution of Doctor Specializations")
plt.pie(x=df1['Count'], labels=df1['Specialization'], autopct='%1.1f%%')
plt.show()
\`\`\`

### Patient Counts by Department
- Query to get the distribution of patient counts by department and visualize it using a bar chart.

\`\`\`python
query2 = "SELECT DEPT_NAME, COUNT(*) FROM PATIENT_ENTRY JOIN DEPARTMENTS ON PATIENT_ENTRY.DEPT_ID = DEPARTMENTS.DEPT_ID GROUP BY DEPT_NAME;"
df2 = pd.read_sql(query2, mydb)
sns.set_style('whitegrid')
plt.figure(figsize=(15, 5))
plt.title("Number of Patients Admitted to Each Department")
sns.barplot(x='DEPT_NAME', y='COUNT(*)', data=df2)
plt.xlabel("Department")
plt.ylabel("Number of Patients")
plt.xticks(rotation=45)
plt.show()
\`\`\`

### Patients Diagnosed by Doctors in Each Specialization
- Query to get the number of patients diagnosed by doctors in each specialization and visualize it using a scatter plot.

\`\`\`python
query3 = "SELECT DOCTORS.SPECIALIZATION, COUNT(*) AS NUM_PATIENTS FROM PATIENT_DIAGNOSE JOIN DOCTORS ON PATIENT_DIAGNOSE.DOC_ID = DOCTORS.DOC_ID GROUP BY DOCTORS.SPECIALIZATION ORDER BY NUM_PATIENTS DESC"
mycursor.execute(query3)
data3 = mycursor.fetchall()
df3 = pd.DataFrame(data3, columns=['Specialization', 'Num_Patients'])
plt.figure(figsize=(14, 9))
sns.scatterplot(data=df3, x="Specialization", y="Num_Patients", s=df3['Num_Patients']*300, hue="Specialization", palette="bright")
plt.title("Number of Patients Diagnosed by Doctors in Each Specialization")
plt.xlabel("Specialization")
plt.ylabel("Number of Patients")
plt.xticks(rotation=45)
plt.show()
\`\`\`

### Most Common Diagnoses
- Query to get the most common diagnoses and visualize it using a bar plot.

\`\`\`python
query4 = "SELECT DIAGNOSIS, COUNT(*) AS NUM_PATIENTS FROM PATIENT_DIAGNOSE GROUP BY DIAGNOSIS ORDER BY NUM_PATIENTS DESC LIMIT 5"
mycursor.execute(query4)
data4 = mycursor.fetchall()
df4 = pd.DataFrame(data4, columns=['Diagnosis', 'Number of Patients'])
plt.figure(figsize=(8, 6))
sns.set_style('darkgrid')
sns.barplot(x='Number of Patients', y='Diagnosis', data=df4, orient='h')
plt.xlabel('Number of Patients')
plt.ylabel('Diagnosis')
plt.title('Top 5 Diagnoses by Number of Patients')
plt.xticks(rotation=45)
plt.show()
\`\`\`

### Monthly Patient Discharges
- Query to get the number of patient discharges by month and visualize it using a line graph.

\`\`\`python
query = "SELECT DISCHARGE_DATE FROM patient_discharge"
mycursor.execute(query)
results = mycursor.fetchall()
df = pd.DataFrame(results, columns=["Discharge_Date"])
df['Discharge_Date'] = pd.to_datetime(df['Discharge_Date'])
df['Year_Month'] = df['Discharge_Date'].dt.to_period('M')
discharges_by_month = df.groupby('Year_Month').size().reset_index(name='Discharges')
discharges_by_month['Year_Month'] = discharges_by_month['Year_Month'].astype(str)
plt.figure(figsize=(12, 6))
plt.plot(discharges_by_month['Year_Month'], discharges_by_month['Discharges'], marker='o', color='red', linestyle='-')
plt.title('Monthly Patient Discharges')
plt.xlabel('Year-Month')
plt.ylabel('Number of Discharges')
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()
\`\`\`

### Average Salary of Doctors
- Query to get the distribution of doctor salaries and visualize it using a pie chart.

\`\`\`python
query = "SELECT CASE WHEN SALARY BETWEEN 0 AND 50000 THEN '0-50,000' WHEN SALARY BETWEEN 50001 AND 100000 THEN '50,001-100,000' WHEN SALARY BETWEEN 100001 AND 150000 THEN '100,001-150,000' WHEN SALARY BETWEEN 150001 AND 200000 THEN '150,001-200,000' ELSE '200,001+' END as Salary_Range, COUNT(*) as COUNT FROM duty_doc GROUP BY Salary_Range"
mycursor.execute(query)
results = mycursor.fetchall()
df = pd.DataFrame(results, columns=["Salary_Range", "Count"])
plt.figure(figsize=(10,8))
plt.title("Distribution of Doctors by Salary Range")
plt.pie(x=df['Count'], labels=df['Salary_Range'], autopct='%1.1f%%', startangle=140)
plt.axis('equal')
plt.show()
\`\`\`

### Department Location with Department ID
- Query to get the relationship between department locations and department IDs and visualize it using a scatter plot.

\`\`\`python
query = "SELECT DEPT_ID, DEPT_LOCATION, DEPT_NAME FROM departments"
mycursor.execute(query)
results = mycursor.fetchall()
df = pd.DataFrame(results, columns=["Dept_ID", "Dept_Location", "Dept_Name"])
plt.figure(figsize=(12, 8))
sns.scatterplot(x="Dept_ID", y="Dept_Location", hue="Dept_Name", palette="deep", data=df, s=100, legend=False)
plt.title("Department Location with Department ID")
plt.xlabel("Department ID")
plt.ylabel("Department Location")
plt.grid(True)
for i in range(df.shape[0]):
    plt.text(x=df.Dept_ID[i], y=df.Dept_Location[i], s=df.Dept_Name[i], 
             fontdict=dict(color='black', size=10),
             bbox=dict(facecolor='yellow', alpha=0.5))
plt.show()
\`\`\`

### Patient Condition
- Query to get the patient condition by patient ID and visualize it using an area plot.

\`\`\`python
mycursor.execute("SELECT PATIENT_id, PATIENT_CONDITION FROM patient_operated")
results = mycursor.fetchall()
patient_data = pd.DataFrame(results, columns=["Patient_ID", "Patient_Condition"])
grouped_data = patient_data.groupby(['Patient_ID', 'Patient_Condition']).size().unstack(fill_value=0)
grouped_data.plot(kind='area', stacked=True)
plt.xlabel('Patient ID')
plt.ylabel('Number of Patients')
plt.title('Patient Condition by Patient ID')
plt.show()
\`\`\`

### Monthly Patient Admissions
- Query to get the number of patient admissions by month and visualize it using a line graph.

\`\`\`python
query = "SELECT ADMITTED_DATE FROM patient_admit"
mycursor.execute(query)
results = mycursor.fetchall()
df = pd.DataFrame(results, columns=["Admit_Date"])
df['Admit_Date'] = pd.to_datetime(df['Admit_Date'])
df['Year_Month'] = df['Admit_Date'].dt.to_period('M')
admissions_by_month = df.groupby('Year_Month').size().reset_index(name='Admissions')
admissions_by_month['Year_Month'] = admissions_by_month['Year_Month'].astype(str)
plt.figure(figsize=(12, 6))
plt.plot(admissions_by_month['Year_Month'], admissions_by_month['Admissions'], marker='o', color='blue', linestyle='-')
plt.title('Monthly Patient Admissions')
plt.xlabel('Year-Month')
plt.ylabel('Number of Admissions')
plt.xticks(rotation=45)
plt.grid(True)
plt.tight_layout()
plt.show()
\`\`\`

### Gender Distribution of Patients
-Query to get the gender distribution of patients and visualize it using a bar plot.

\'\'\'python
query = "SELECT GENDER, COUNT(*) AS NUM_PATIENTS FROM patient_entry GROUP BY GENDER"
mycursor.execute(query)
results = mycursor.fetchall()
df = pd.DataFrame(results, columns=["Gender", "Number of Patients"])
plt.figure(figsize=(8, 6))
sns.barplot(x='Gender', y='Number of Patients', data=df, palette="coolwarm")
plt.title('Gender Distribution of Patients')
plt.xlabel('Gender')
plt.ylabel('Number of Patients')
plt.show()
\'\'\'

### Payment Method Distribution
-Query to get the distribution of payment methods used by patients and visualize it using a pie chart.

\'\'\'python
query = "SELECT PAYMENT_METHOD, COUNT(*) AS NUM_PATIENTS FROM patient_payment GROUP BY PAYMENT_METHOD"
mycursor.execute(query)
results = mycursor.fetchall()
df = pd.DataFrame(results, columns=["Payment Method", "Number of Patients"])
plt.figure(figsize=(10,8))
plt.pie(df["Number of Patients"], labels=df["Payment Method"], autopct='%1.1f%%', startangle=140)
plt.title("Distribution of Payment Methods Used by Patients")
plt.show()
\'\'\'


## Table Structure

### Doctors Table
- **Columns**: DOC_ID (INT), NAME (VARCHAR), SPECIALIZATION (VARCHAR), DEPT_ID (INT)
- **Description**: Stores information about doctors including their unique ID, name, specialization, and department ID.

### Patients Table
- **Columns**: PATIENT_ID (INT), NAME (VARCHAR), AGE (INT), GENDER (VARCHAR), ADDRESS (VARCHAR), CONTACT (VARCHAR)
- **Description**: Stores patient information including their unique ID, name, age, gender, address, and contact details.

### Patient Entry Table
- **Columns**: ENTRY_ID (INT), PATIENT_ID (INT), DEPT_ID (INT), ADMISSION_DATE (DATE)
- **Description**: Records the entry of patients into the hospital, including the department they are admitted to and the date of admission.

### Patient Diagnose Table
- **Columns**: DIAGNOSE_ID (INT), PATIENT_ID (INT), DOC_ID (INT), DIAGNOSIS (VARCHAR)
- **Description**: Stores diagnosis details including the patient ID, doctor ID, and the diagnosis made.

### Patient Operated Table
- **Columns**: OPERATION_ID (INT), PATIENT_ID (INT), DOC_ID (INT), OPERATION_DATE (DATE), PATIENT_CONDITION (VARCHAR)
- **Description**: Records operations performed on patients, including the doctor ID, date of operation, and the patient's condition post-operation.

### Departments Table
- **Columns**: DEPT_ID (INT), DEPT_NAME (VARCHAR), DEPT_LOCATION (VARCHAR)
- **Description**: Stores information about hospital departments including their unique ID, name, and location.

### Patient Discharge Table
- **Columns**: DISCHARGE_ID (INT), PATIENT_ID (INT), DISCHARGE_DATE (DATE)
- **Description**: Records discharge details of patients, including the date of discharge.

### Duty Doctors Table
- **Columns**: DUTY_ID (INT), DOC_ID (INT), DEPT_ID (INT), DUTY_DATE (DATE), SALARY (FLOAT)
- **Description**: Stores information about doctors on duty, including their department, date of duty, and salary.

## Installation and Setup

To set up and run the Hospital Management System, follow these steps:

1. **Clone the repository**:
   \`\`\`bash
   git clone https://github.com/yourusername/hospital-management-system.git
   \`\`\`

2. **Navigate to the project directory**:
   \`\`\`bash
   cd hospital-management-system
   \`\`\`

3. **Install the required dependencies**:
   \`\`\`bash
   pip install -r requirements.txt
   \`\`\`

4. **Set up the database**:
   - Ensure you have MySQL installed and running.
   - Create a new database and import the provided SQL script to set up the tables and initial data:
     \`\`\`bash
     mysql -u yourusername -p yourpassword hospital_db < hospital_management_system.sql
     \`\`\`

5. **Configure the database connection**:
   - Update the database connection settings in the `config.py` file with your MySQL credentials.

6. **Run the application**:
   \`\`\`bash
   python app.py
   \`\`\`

## Conclusion
The Hospital Management System project is a comprehensive software solution designed to streamline hospital operations. It simplifies tasks such as patient registration, doctor scheduling, diagnosis tracking, and financial management into one user-friendly platform. With its ability to generate reports and analyze data, the system empowers administrators to make informed decisions and ensure compliance with regulatory standards. Overall, it enhances efficiency, communication, and patient care within healthcare facilities.
___
