### Employee Performance Dashboard with Power BI

---

### Description

This is a Power BI Dashboard I created for my current role. It loads data straight from a MySQL server which is hosted on AWS, so the data is always up to date. It uses various visuals to show employee performance in clinical documentation and is primarily used by supervisors.

### Overview

The first page is the welcome page, showing the company logo and the reports available for selection. The Overview page displays notes for all staff over the past 3 months, providing counts, trend charts and caregivers with the highest late and unbillable notes. The last page is the Caregiver Details page. This page provides supervisors with a date and caregiver slicer for filtering. There's 3 gauges - one for each note status, and a bar chart, which shows the same data over a chosen length of time.

## Files

`README.md`- This file

[Dashboard.pdf](Dashboard.pdf) - Preview of the Dashboard

### Filters

Date Select Slicer: Allows supervisors to filter data table and visuals by Year and Month. 

Caregiver Select Slicer: Allows supervisors to select a specific caregiver for performance review.

### Visuals

Cards - KPIs which include Total, Late, Unbillable notes and a performance card which shows on time Notes vs Total Notes.

Gauges - Shows counts for Total, Late and Unbillable notes as a measure for performance

Treemaps - Displays the top several Caregivers with the highest count of Late notes and Unbillable notes

Area Charts - Shows Total Late Notes and Unbillables in the past 3 months with a trend line 

Bar Chart - Grouped by Year, Month and Note Status, this gives the viewer an overview of any particular staff for the chosen time frame

### DAX Formulas Used

1. Column added to the data set which determines if the note is On Time, Late, or Unbillable by the use of a SWITCH statement:

- `NoteStatus = SWITCH(TRUE, notes[days_to_sign] < 3 && NOT(ISBLANK(notes[staff_sign_date])), "On Time", notes[days_to_sign] > 2 && notes[days_to_sign] < 8, "Late", notes[days_to_sign] > 7 || ISBLANK(notes[staff_sign_date]), "Unbillable")`

 2. A Measure which Counts Late notes with an IF statement for detecting blanks as zero counts:

- `CountLateNotesGauge = IF(ISBLANK(CALCULATE(COUNTROWS('notes'), 'notes'[NoteStatus] = "Late")), 0, CALCULATE(COUNTROWS('notes'), 'notes'[NoteStatus] = "Late"))`

 3. A Measure which Counts Unbillable notes with an IF statement for detecting blanks as zero counts:

- `CountUnbillablesGauge = IF(ISBLANK(CALCULATE(COUNTROWS('notes'), 'notes'[NoteStatus] = "Unbillable")), 0, CALCULATE(COUNTROWS('notes'), 'notes'[NoteStatus] = "Unbillable"))`

4. Returns the month from the date:

- `month = FORMAT('notes'[date_of_service],"mmmm")`

5. Returns the month number from the date:

- `MonthNumber = FORMAT('notes'[date_of_service],"mm")`

6. Used to encode names for privacy purposes:

- `supervisor_code = CONCATENATE(UPPER(LEFT(notes[client_supervisor], 3) ), UPPER(MID(notes[client_supervisor], FIND(" ", notes[client_supervisor], 2) + 1, 2 ) ) )`

7. Used to create a new column which adds another hierarchy for Dates called Billing Cycles, so that data can be viewed for each cycle, if desired.

- `BillingCycle = IF(notes[date_of_service].[Day] > 0 && notes[date_of_service].[Day] < 16, "1-15", "16-eom")`
