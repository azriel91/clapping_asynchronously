# Scenario

A client has a list of property title numbers and owners. Each title number is associated with additional information -- address, sales information, and phone number.

However, the additional information is stored in a separate place, and requires authentication to access.

Your job is to merge all of the information into the same list.

Concretely, turn this:

| Property Title | Owners                 | Address                            | Phone Number | Last Sale |
| -------------- | ---------------------- | ---------------------------------- | ------------ | --------- |
| `AA1234/01`    | John Smith, Mary Smith |                                    |              |           |
| `BB5678/02`    | John Doe               |                                    |              |           |
| `..`           | ..                     |                                    |              |           |

Into this:

| Property Title | Owners                 | Address                            | Phone Number | Last Sale |
| -------------- | ---------------------- | ---------------------------------- | ------------ | --------- |
| `AA1234/01`    | John Smith, Mary Smith | 123 Something Street, Somewhere    | 123-4567     | 1997      |
| `BB5678/02`    | John Doe               | 456 Another Street, Somewhere Else | *missing*    | 2002      |
| `..`           | ..                     | ..                                 | ..           | ..        |
