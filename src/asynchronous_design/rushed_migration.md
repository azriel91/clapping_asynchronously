# Rushed Migration

Hack approach (not necessarily recommended, but has its uses):

* Add `async` in front of `fn`s:

    ```rust,ignore
    pub fn t07_retrieve_information(..)
    pub async fn t07_retrieve_information(..)
    ```

* Change synchronous logic to asynchronous logic:

    ```rust,ignore
    std::thread::sleep(Duration::from_millis(delay));
    tokio::time::delay_for(Duration::from_millis(delay)).await
    ```

* Use types with asynchronous support:

    ```rust,ignore
    // synchronous -- will idly wait when there are no messages.
    let (tx, rx) = crossbeam::channel::unbounded::<_>();

    // asynchronous -- will switch to a different task when there are no messages.
    let (tx, rx) = tokio::sync::mpsc::unbounded_channel::<_>();
    ```

* Swap understandable errors for impossible ones ([tokio#1835](https://github.com/tokio-rs/tokio/issues/1835)):

    ```rust,ignore
        error[E0308]: mismatched types
      --> src/main.rs:12:9
       |
    12 |         tokio::spawn(async move {
       |         ^^^^^^^^^^^^ one type is more general than the other
       |
       = note: expected type `std::ops::FnOnce<(std::iter::Map<_> + std::marker::Send>>,)>`
                  found type `std::ops::FnOnce<(std::iter::Map<_> + std::marker::Send>>,)>`

    error: aborting due to previous error
    ```

And I show you a still more excellent way.
