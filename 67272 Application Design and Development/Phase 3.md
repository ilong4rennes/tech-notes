# Specifications for Phase 3 Models

  

As part of phase 3, you will have to create models for the remaining entities and write unit tests for these models. To make sure you are able to build these models and tests, below are some specs for each of these models.

  

**Please read this file online at GitHub or using a Markdown preview browser** (built into VS Code or stand-alone app like [MacDown](https://macdown.uranusjr.com/)) so there are no mistakes with escape characters (i.e., backslashes)

  

---

  

_Ability must:_

  

1. be written such that it passes the tests we have given you in the starter code (/test/models/ability.rb)

  

---

  
_Criminals must:_


1. have all proper relationships specified

2. have a first and last name

3. be destroyed only if they meet three conditions. First, they cannot be a convicted felon. Second, they cannot have enhanced powers. Third, they must not be a current suspect in an open investigation. If any of these hold, an appropriate (and different) error message must be given and the destroy command aborted.

4. have the following scopes:

- 'alphabetical' -- returns all criminals in alphabetical order by last name, first name

- 'prior_record' -- returns all criminals that are are convicted felons

- 'enhanced' -- returns all criminals that have enhanced powers

- 'search' -- returns all criminals where the start of the first name, last name, or aka matches a given string

5. have the following methods:

- 'name' -- which returns the criminal name as a string "last_name, first_name" in that order

- 'proper_name' -- which returns the criminal name as a string "first_name last_name" in that order with a space between them

  

---

  

_Suspects must:_

  

1. have all proper relationships specified

2. have all foreign key fields present

3. have legitimate dates for both `added_on` and `dropped_on`. The `added_on` date must not be in the future. Additionally, the `dropped_on` date, if not blank, must be on or after the `added_on` date.

4. have the following scopes:

- 'current' -- returns all suspects who have not been dropped as suspects

- 'chronological' -- returns all suspects ordered by added_on date

- 'alphabetical' -- orders results alphabetically by a criminal's last, then first, name

  

---

  

_InvestigationNotes must:_

  

1. have all proper relationships specified

2. have all foreign key fields present

3. have some content (no blank notes)

4. be for an investigation that is currently open

5. be written by an officer who is currently assigned to the investigation

6. automatically set the note's date to the current date

7. not be destroyed so as to preserve the integrity of the investigation history

8. have the following scopes:

- 'by_officer' -- orders results alphabetically by the last and then first names of the officers writing notes

- 'chronological' -- orders results by the note date

- 'for_officer' -- which returns all the investigation notes for a given officer (parameter: officer object)

  

---

  

_CrimeInvestigations must:_

  

1. have all proper relationships specified

2. have all foreign key fields present

3. be for a crime that is active in the system

4. be for an investigation that is currently open

5. have the following scopes:

- 'for_crime' -- which returns all the crime investigation records for a given crime (parameter: crime object)

- 'for_investigation' -- which returns all the crime investigation records for a given investigation (parameter: investigation object)

  

---

  

_Crimes, in addition to previous requirements, must:_

1. make all appropriate updates to match the revised data model
2. not be destroyed, only made inactive

---

  

_Units, in addition to previous requirements, must:_

1. make all appropriate updates to match the revised data model
2. be destroyed only if there are no officers assigned to the unit

---

_Officers, in addition to previous requirements, must:_

  

1. make all appropriate updates to match the revised data model

2. be destroyed only if the officer has no, and has never had any, assignments

3. end all associated assignments when the officer is made inactive

4. have a role that is either officer, chief, or commish using an enum. In the enum, officers should be listed as 1, chiefs as 2, and commish as 3.

5. have the following new scopes:

- officer_role -- which returns all the officers who have the role of officer
- chief_role -- which returns all the officers who have the role of chief
- commish_role -- which returns all the officers who have the role of commish

6. have the following for password management:

- virtual attributes for `password` and `password_confirmation`
- on creation, those two fields are mandatory (i.e., required)
- those fields must match each other when a new officer is being created
- a password must have a minimum length of four characters

7. have usernames that are required and must be unique (case-insensitive) in the system

8. have an instance method called authenticate which will compare the password provided with the password_digest stored in the database and return an officer object if they match, or false if they do not

9. have a class method called authenticate which takes both a username and password parameter and compare the password provided with the password_digest stored in the database and return an officer object if they match, or false if they do not

  

---

  

_Assignments, in addition to previous requirements, must:_

  

1. make all appropriate updates to match the revised data model

2. not be destroyed

  

---

  

_Investigations, in addition to previous requirements, must:_

  

1. make all appropriate updates to match the revised data model

2. not be associated with Monty Python in any way

3. cannot be destroyed for any reason

4. have a method called `close` which sets the date_closed date to the current date, as well as terminating any remaining assignments to the investigation

5. have the following additional scopes:

- 'unassigned' -- which returns all the investigations that have no assignments associated with it

- 'title_search' -- which returns all the investigations that contain a given term in its title (parameter: term)

6. if for any reason investigations are associated with Monty Python, you can [expect the unexpected](https://www.youtube.com/watch?v=Cj8n4MfhjUc).