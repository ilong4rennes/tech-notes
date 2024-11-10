# Specifications for Phase 2  
  
As part of phase 2, you will have to create models for `Unit`, `Crime`, `Investigation`, `Officer` and `Assignment` and write unit tests for these models. To make sure you are able to build these models and tests properly, below are some specs for each of these models.  
  
One thing to note is that in this phase, we are assuming that for each investigation there is one and only one crime committed.  
  
---  
  
_Units must:_  
  
1. have all proper relationships specified  
2. have a name and it must be unique (case-insensitive)  
3. have the following scopes:  
   - 'active' -- returns only active units  
   - 'inactive' -- returns all inactive units  
   - 'alphabetical' -- orders results alphabetically by unit name  
4. have the following methods:  
   - 'make_active' -- which flips an active boolean from inactive to active and updates the record in the database  
   - 'make_inactive' -- which flips an active boolean from active to inactive and updates the record in the database  
   - 'number_of_active_officers' -- which returns the number of active officers currently assigned to the unit  
   - 'number_of_unique_open_investigations' -- which returns the number of open investigations the unit is part of (no double counting)  
  
---  
  
_Crimes must:_  
  
1. have all proper relationships specified  
2. have a name and it must be unique (case-insensitive)  
3. have the following scopes:  
   - 'active' -- returns only active crimes  
   - 'inactive' -- returns all inactive crimes  
   - 'alphabetical' -- orders results alphabetically by crime name  
   - 'felonies' -- returns crimes that are marked as felony:true  
   - 'misdemeanors' -- returns crimes that are marked as felony:false  
4. have the following methods:  
   - 'make_active' -- which flips an active boolean from inactive to active and updates the record in the database  
   - 'make_inactive' -- which flips an active boolean from active to inactive and updates the record in the database  
  
---  
  
_Investigations must:_  
  
1. have all proper relationships specified  
2. have values which are the proper data type and within proper ranges. Remembering the 2nd Rule of Software Development, that means:  
   - have a date_opened (must be present) that is a proper date and is set to either the current date or some date in the past (i.e., cannot be in the future)  
   - have a date_closed that, if it is set, is a proper date and is set to the date_opened or sometime after that date  
   - an investigation title and opening date are required  
   - have a crime_id and it must be restricted to crimes which exist and are active in the system  
3. have the following scopes:  
   - 'is_open' -- returns only investigations without an end date set  
   - 'is_closed' -- returns only investigations with an end date set  
   - 'alphabetical' -- orders results alphabetically by title  
   - 'chronological' -- orders results chronologically by date_opened  
   - 'open_on_date' -- returns all investigations that were open on a given date (\_parameter: date\*)  
   - 'with_batman' -- returns investigations that Batman was/is involved in  
   - 'was_solved' -- returns investigations that were marked solved  
   - 'unsolved' -- returns all investigations that are unsolved  
4. have a callback which terminates any current assignments of officers to an investigation that has been closed  
  
---  
  
_Officers must:_  
  
1. have all proper relationships specified  
2. have a first and last name (no other validation required)  
3. have a unit_id and that unit_id must be one that is not only in the system, but listed as active  
4. have values which are the proper data type and within proper ranges. Remembering the 2nd Rule of Software Development, that means:  
   - social security number must be present and a 9 digit number, but the user may input values with dashes, spaces or other common formats ‚Äì e.g., 999-99-9999; 999 99 9999; 999999999 are all acceptable  
   - ranks are restricted to the following list: Officer, Sergeant, Detective, Detective Sergeant, Lieutenant, Captain, Commissioner  
5. have social security numbers which are unique in the system.  
6. have social security number values that are saved in the system as a string of 10 digits only, regardless of the format the user inputs.  
7. have the following scopes:  
   - 'active' -- returns only active officers  
   - 'inactive' -- returns all inactive officers  
   - 'alphabetical' -- orders results alphabetically by last name, first name  
   - 'for_unit' -- returns all officers for a given unit (parameter: unit_id)  
   - 'detectives' -- returns all officers who have the rank of detective  
   - 'search' -- returns all officers whose last or first name match or partially match a given term (_parameter: term_)  
8. have the following methods:  
   - 'name' -- which returns the employee name as a string "last_name, first_name" in that order  
   - 'proper_name' -- which returns the employee name as a string "first_name last_name" in that order with a space between them  
   - 'current_assignments' -- which returns the officer's current assignments or nil if the officer does not have a current assignment  
   - 'make_active' -- which flips an active boolean from inactive to active and updates the record in the database  
   - 'make_inactive' -- which flips an active boolean from active to inactive and updates the record in the database  
  
---  
  
_Assignments must:_  
  
1. have all proper relationships specified  
2. have an officer, investigation, and a start date  
3. have start and end dates that are proper dates, if given. In addition:  
   - every assignment must have a start date and it must be on or before the present date  
   - an assignment's end date (if it exists) must be after its start date  
4. have an officer_id and it must be restricted to officers who exist and are active in the system  
5. restrict investigation_ids to investigations that are presently open in the system  
6. should not allow duplicates (same officer assigned an investigation they are already assigned to) to be recorded in the system. It should, however, allow an officer who was previously assigned and then removed, to be reassigned.  
7. have the following scopes:  
   - 'current' -- which returns all the assignments that are considered current  
   - 'past' -- which returns all the assignments that have terminated  
   - 'by_officer' -- which orders assignments by officer name (last, first)  
   - 'chronological' -- which orders assignments chronologically with the most recent assignments listed first  
   - 'for_date' ‚Äì- which returns all assignments that were/are active on that particular date (parameter: date)  
  
---  
  
NOTE: In this phase we will _not_ validate that any `active` field is actually a boolean.