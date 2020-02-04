# Synchronous

The quality product can be implemented synchronously.

| Quality Product                                   |
| ------------------------------------------------- |
|  0. Handle interruptions.                         |
|  1. Read credentials.                             |
|  2. Stream property title records.                |
|  3. Read output file for processed records.       |
|  4. Start progress bar.                           |
| **For each record:**                              |
|  5. Rate limit requests -- don't overload server. |
|  6. Authenticate with server if necessary.        |
|  7. Retrieve information.                         |
|  8. Augment record.                               |
|  9. Output record to file.                        |
| 10. Update progress bar.                          |
| **When interrupted, or done:**                    |
| 11. Output execution report.                      |

In code ([source](https://github.com/azriel91/cli_async/blob/sync/src/main.rs#L119)):

```rust,ignore
fn main() {
    // ..
    let interrupt_rx =         t00_setup_interrupt_handler();
    let credentials =          t01_read_credentials();
    let records =              t02_stream_property_title_records(record_count);
    let records_precompleted = t03_read_output_file(skip);

    t04_start_progress_bar(&mut reporter);

    let _result = records
        // ..
        .map(|(n, record)| {
            t05_rate_limit_requests(delay_rate_limit);
            t06_authenticate_with_server(n == 0, credentials, delay_auth);
            t07_retrieve_information(n, record, delay_retrieve);
            t08_augment_record(record, info)
        })
        .try_for_each(|property_record_populated| {
            t09_output_record_to_file(property_record_populated);
            t10_update_progress_bar(&mut reporter);
        });

    t11_output_execution_report(&reporter);
}
```

Demo:

```bash
git clone git@github.com:azriel91/cli_async.git
cd cli_async
git worktree add ./sync sync
cd sync
cargo build --release
time ./target/release/cli_async -c 100 --delay-rate-limit 0 --delay-retrieve 1
```
