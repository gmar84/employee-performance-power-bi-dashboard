### Employee Performance Dashboard with Power BI

---

### Description

This is a Power BI Dashboard I created for my current role. It loads data straight from a MySQL server which is hosted on AWS, so the data is always up to date. It uses various visuals to show employee performance in clinical documentation and is primarily used by supervisors.

## Files

`README.md`- This file

[Dashboard.pdf](Dashboard.pdf) - Preview of the Dashboard

### FILTERS

Date Select Slicer: Allows supervisors to filter data table and visuals by Year and Month. 

Caregiver Select Slicer: Allows supervisors to select a specific caregiver for performance review.

### VISUALS

Cards - KPIs which include Total, Late, Unbillable notes and a performance card which shows on time Notes vs Total Notes.

Data Table - Shows employee, Total Notes, On Time notes, Late Notes, Unbillable Notes and a percentage of Late Notes which is calculated as Late Notes / Total Notes.

Treemaps - Top 5 counts of Caregivers with Unbillables and another with Late Notes

Area Charts - Shows Total Late Notes and Unbillables in the past 3 months with a trend line. The slicers at the top do not interact with these visuals, as they are meant to always provide an overall trend of performance.

### DAX formulas used

Column added to the data set which determines if the note is Late or On Time:

`NoteStatus = IF('notes'[days_to_sign] > 2, "Late", "On Time")`

 A Measure which Counts a note as as unbillable if its over 6 days old OR there is no signature:

`CountUnbillables = CALCULATE(COUNTROWS('notes'), 'notes'[days_to_sign] > 6 || 'notes'[staff_sign_date] = BLANK())`

Returns the month from the date:

`month = FORMAT('notes'[date_of_service],"mmmm")`

Returns the month number from the date:

`MonthNumber = FORMAT('notes'[date_of_service],"mm")`

Used to concatenate the first 3 letters of first name + first two letters of last name:

`supervisor_code = CONCATENATE(UPPER(LEFT(notes[client_supervisor], 3) ), UPPER(MID(notes[client_supervisor], FIND(" ", notes[client_supervisor], 2) + 1, 2 ) ) )`
