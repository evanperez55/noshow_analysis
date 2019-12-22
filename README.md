# noshow_analysis
Analyzed dataset that collected information from 100k  medical appointments in Brazil and is focused on the question of whether or not patients show up for their appointment. A number of characteristics about the patient are included in each row. (20k no show, 80k show)

Question: What factors are important in determining if patients show up or not?


# Table Schema for import

CREATE TABLE noShow (
		PatientID NUMERIC,
		AppointmentID NUMERIC,
		Gender CHARCHAR(1),
		ScheduledDay DATE,
		AppointmentDay DATE,
		Age INT,
		Neighbourhood VARCHAR(50),
		Scholarship INT,
		Hipertension INT,
		Diabetes INT,
		Alcoholism INT,
		Handcap INT,
		SMS_received INT,
		NoShow VARCHAR(50)
		)



# Steps to take
1. Clean data
a. Check data for outliers/invalid data (Python)
b. Remove data outside parameters (SQL)
c. Change column names and values to be relative format (SQL)
2. Transform
a. Create table with biomarker counts (SQL)
     i. No Show and Show 
b. Create table of difference between when appointment was scheduled and the actual date of the appointment (SQL)
3. Model
a. Map frequency of disease rates against No Show and Show (Tableau)
b. Map out No Show and Show appointments and the difference between the scheduled and actual day of the appointment (Tableau)

# Cleaning Data

## Inspect data
df_no_show = pd.read_csv('no-show.csv') #load csv into dataframe
df_no_show.info()  # look for proper data types and null values
df_no_show.duplicated() # look for duplicate values
df_no_show.describe() # Look for outliers


## Updating data to relevant format
ALTER TABLE noShow RENAME COLUMN neighbourhood TO neighborhood;
ALTER TABLE noShow RENAME COLUMN hipertension TO hypertension;
ALTER TABLE noShow RENAME COLUMN handcap TO handicap;

## Removing invalid data
DELETE FROM noShow WHERE age < 0;
DELETE FROM noShow WHERE handicap > 1;

# Prepare data for modeling

## Biomarkers data
CREATE TABLE biomarkers (affliction VARCHAR(50), patients INT)

INSERT INTO biomarkers (affliction, patients)
VALUES ('hypertension-n', (SELECT COUNT(*) FROM noShow WHERE hypertension = 1 AND noShow = 'Yes'))

INSERT INTO biomarkers (affliction, patients)
VALUES ('hypertension-s', (SELECT COUNT(*) FROM noShow WHERE hypertension = 1 AND noShow = 'No'))

## Scheduling Data
	
SELECT  noShow, (AppointmentDay - ScheduledDay) AS Days, COUNT(*) INTO appointments 
FROM noShow 
____________________________________________________________________________
Data Used: https://www.kaggle.com/joniarroba/noshowappointments
Tools Used: PostgreSQL, Python, Tableau

