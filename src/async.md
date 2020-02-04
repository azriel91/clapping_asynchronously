# Async

The quality product can be implemented *async*hronously.

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

In code ([source](https://github.com/azriel91/cli_async/blob/async_one/src/main.rs#L116)):

> **⚠️ DO NOT USE THIS AS A REFERENCE IMPLEMENTATION**

* Add `async` to functions and blocks.
* Add `.await` to function calls.
* Spawn tasks.

```rust,ignore
#[tokio::main]
async fn main() -> Result<(), ()> {
    // ..

    let (ctrl_c_future, interrupt_rx) = t00_setup_interrupt_handler();
    let credentials =                   t01_read_credentials();
    let records =                       t02_stream_property_title_records(record_count);
    let records_precompleted =          t03_read_output_file(skip);

    t04_start_progress_bar(&mut reporter);

    let reporter_future = async move {
        t10_update_progress_bar(&mut reporter).await;
        t11_output_execution_report(&reporter);
    };

    let processing_future = async move {
        tokio::stream::iter(records)
            .then(move |(n, record)| async move {
                t05_rate_limit_requests(delay_rate_limit).await;
                t06_authenticate_with_server(n == 0, credentials, delay_auth).await;
                t07_retrieve_information(n, record, delay_retrieve).await;
                t08_augment_record(record, info)
            })
            .try_for_each_concurrent(10, move |property_record_populated| async move {
                t09_output_record_to_file(property_record_populated).await;
            })
            .await
    };

    let reporter_handle =   tokio::spawn(reporter_future);

    let ctrl_c_handle =     tokio::spawn(ctrl_c_future);
    let processing_handle = tokio::spawn(processing_future);

    let processed_or_interrupted = async {
        tokio::select! {
            _ = ctrl_c_handle => {}
            _ = processing_handle => {}
        }
    };

    let (_, _) = tokio::join!(reporter_handle, processed_or_interrupted);
}
```

Demo:

```bash
git clone git@github.com:azriel91/cli_async.git
cd cli_async
git worktree add ./async_one async_one
cd async_one
cargo build --release
time ./target/release/cli_async -c 100 --delay-rate-limit 0 --delay-retrieve 1
```

