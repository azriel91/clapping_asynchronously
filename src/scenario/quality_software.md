# Quality Software

| Minimum Viable Product          | Quality Product                                   |
| ------------------------------- | ------------------------------------------------- |
|                                 |  0. Handle interruptions.                         |
| 1. Read credentials.            |  1. Read credentials.                             |
| 2. Read property title records. |  2. Stream property title records.                |
|                                 |  3. Read output file for processed records.       |
|                                 |  4. Start progress bar.                           |
| **For each record:**            | **For each record:**                              |
|                                 |  5. Rate limit requests -- don't overload server. |
| 3. Authenticate with server.    |  6. Authenticate with server if necessary.        |
| 4. Retrieve information.        |  7. Retrieve information.                         |
| 5. Augment record.              |  8. Augment record.                               |
| 6. Output record to file.       |  9. Output record to file.                        |
|                                 | 10. Update progress bar.                          |
|                                 | **When interrupted, or done:**                    |
|                                 | 11. Output execution report.                      |
