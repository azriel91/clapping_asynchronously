# Categorization By Resource Use

Steps are grouped into categories based on the data / resources it accesses. Each category is a kind of task.

| Quality Product                                   | Input | Processing | Web | Reporting |
| ------------------------------------------------- | :---: | :--------: | :-: | :-------: |
|  0. Handle interruptions.                         |   ✔️  |            |     |           |
|  1. Read credentials.                             |       |            |  ✔️ |           |
|  2. Stream property title records.                |       |     ✔️     |     |           |
|  3. Read output file for processed records.       |       |     ✔️     |     |           |
|  4. Start progress bar.                           |       |            |     |     ✔️    |
| **For each record:**                              |       |            |     |           |
|  5. Rate limit requests -- don't overload server. |       |            |  ✔️ |           |
|  6. Authenticate with server if necessary.        |       |            |  ✔️ |           |
|  7. Retrieve information.                         |       |            |  ✔️ |           |
|  8. Augment record.                               |       |     ✔️     |     |           |
|  9. Output record to file.                        |       |     ✔️     |     |           |
| 10. Update progress bar.                          |       |            |     |     ✔️    |
| **When interrupted, or done:**                    |       |            |     |           |
| 11. Output execution report.                      |       |            |     |     ✔️    |
