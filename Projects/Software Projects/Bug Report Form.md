1.  We want to know the goals - what is this supposed to accomplish? - this list will inform how we look at how the rest will work
    1.  Keep issues from getting lost in email
    2.  Guide users to give the most useful information easily
    3.  Let people see where the bug is in the process of fixing it
    4.  Keep record of the efforts
    5.  Keep record of how often problems occur
    6.  Keep record of solution
    7.  Keep people up to date on when things are finished automatically
    8.  Anything else?

2.  What do we want it to do (pseudocode) - This will be the main part of discussion - and this should hopefully accomplish the points in the goals

    1.  When someone has a problem, they open up the form (will have a link on the desktop to the form), and enter information

        1.  About the bug itself
        2.  Information to guide prioritization (how bad is it)
        3.  About themselves (who is it?)
            1.  User's email (not just [monday.com](http://monday.com/) automatically detecting, as this will be whoever last logged in, if anyone. On equipment, that likely won't be whoever is using the equipment)
            2.  **(add user email input)**
        4.  Who should be informed about the progress for that project?
            1.  Team lead's email **(add team lead email input)**
        5.  If the error is imminently a problem, who to tell? **(Add instructions on the form at the top for this.. eg "If this error is immediately stopping time critical work, please call XXX, and then fill out the form.")**

2.  When they submit, the form will go into Monday.com
    1.  The task will go to what group on the sheet? ("Active", "newly submitted", etc?)
    2.  automation will Inform them and the team lead of submission
    3.  Let (who, or multiple whos?) know on the engineering side that there is a new bug
    4.  Someone (who?) will assess the task
        1.  Check immediacy
            1.  If immediate, communicate directly with whoever will fix it
        2.  Assign an engineer
        3.  Assign a manager (eg Engineering team lead)? (probably... is this Narsi or Kevin or Chris)?
            1.  **(Add manager column)**
        4.  Assign priority **(Add priority column)**
    5.  When assigned, tag will change to "assigned to engineer" (is this automatic, or toggled?)

3.  The engineer and engineering team lead will get an email telling them they are assigned - and the piece of equipment, etc involved
4.  When the engineer assigned sees it and starts working on it, they will change the tag to "engineer active"
5.  During bug fixes, notes will be added in the "updates" (the speech balloons)

6.  If a problem arises
    1.  a note will be added in the "updates"
    2.  engineer will toggle to "engineer on hold"
    3.  Email to engineer manager, engineer assigned, team lead, and user with the update will be sent
    4.  Does this move to another group?

7.  When the error is resolved -
    1.  Engineer will tell engineering manager (maybe in "updates")
    2.  Any change in status for the engineering manager to confirm?
    3.  Note will be added into the "updates"
    4.  Engineering manager (or engineer??) will toggle to "error resolved"
    5.  Email to engineer manager, engineer assigned, team lead, and user with the update will be sent
    6.  Will stay in "active" until notes completed

8.  Notes about the bug will be added by engineer, especially the solution
9.  When notes complete and the report added (uploaded to the [monday.com](http://monday.com/) pulse)
    1.  Engineer (or engineering manager??) will click "report finished"
    2.  The board will send the report to the engineer, engineer manager, team lead, and user
    3.  The board will move the task to the "resolved"

# Features to Add
- [x] Add emails of all people to include in this item
- [x] Automation to convert emails to people column
    - Do **not** automatically assign the creator because the creator account might not be the person who created the report
    - Don't need to do this
- [ ] Add priority column?
    - This should be decided on by engineers, the user can already indicate the severity of the bug
- [x] Add a group for requests that have not yet been handled?
- [x] Send an email/notification when an update is created to all parties assigned
- [x] Add group for pending resolution?
- [x] Make name column into a combo of equipment and severity (and maybe some other columns)
    - Name column cannot be modified with automations. Define a naming scheme in the description instead
- [x] Add Kevin to automated engineer assigned
- [x] Add column for adding emails of supervisors to notify
- [x] Add severity column that is of status type that restricts the options to a single column
- [x] When engineer is assigned, update status column
- [x] Add engineer manager column to separate the assigned engineer functionality

# Update Texts
<b>Engineer Investigation</b>
Steps engineer took to understand the problem

<b>Root Causes</b>
What is the actual source of the error?

<b>Fix</b>
What did the Engineer do to resolve the issue?

<b>Validation</b>
How did the engineer confirm that the problem is fixed?

<b>Preventative Steps</b>
What can be done to prevent further related problems?

# Tests 07/20/22
## Notes
- Adding engineer manager emails in any email box will result in double emails when an item is created
- Do all engineer managers need to be notified for every status change? Can this be changed to just the person in the Engineer Manager column?

## Changes
- Added form labels back after they were accidentally deleted
- Engineer log Updates will be created when
    - (Either of these options)
        1. status changes Engineer Assigned -> Engineer Active
        2. Engineer Log Created box checked (preferred option)
- Email person when added to Engineer Assigned

# Flow
1. **User** submits form
2. **Engineer managers** and all email addresses provided get an email update that a new item has been created
    - Updates will also be sent every time the Status column updates
3. **Engineer managers** decide who will be responsible for fixing the issue and assign responsible engineer and one manager to the item. All engineer managers will continue to get status updates.
4. **Engineer assigned** will set status to "Engineer active" when they are ready to work on it which will send the item to the Active column.
5. **Engineer assigned** will click "Engineer Log Created" checkbox to create the log updates and will then pin them to the top
    - Time dedicated to filling out the log during active work is at the engineer's discretion and will depend on the error.
6. **Engineer assigned** will set the status to "Engineer on hold" if they are blocked from completing the task. They will set the status back to "Engineer active" when the block has been removed.
7. **Engineer assigned** will set the status to "Error resolved" when the issue has been fixed. This will send the item to the "Error Resolved" group.
8. **Engineer assigned** will complete the rest of the log at this time and will change the status "report finished" when this has been done.