# Information Flow

```dot process Information Flow Graph
digraph {
    compound = true
    nodesep = 0.02
    ranksep = 0.3
    node [
        fontname="Helvetica"
        fontsize=10
        width=0.5
        shape=rectangle
        style="rounded, filled"
    ]

    t0_setup [
        label =
            < <table border="0" cellborder="0" cellspacing="0">
                <tr>
                    <td align="left"><b>T0</b></td>
                    <td align="left" balign="left">Setup<br/>Int. Handler</td>
                </tr>
            </table> >
        fillcolor = "#ffcc44"
        style = "rounded, filled, dotted"
    ]

    t0 [
        label =
            < <table border="0" cellborder="0" cellspacing="0">
                <tr>
                    <td align="left"><b>T0</b></td>
                    <td align="left" balign="left">Handle<br/>Interruptions</td>
                </tr>
            </table> >
        fillcolor = "#ffcc44"
    ]

    t1 [
        label =
            < <table border="0" cellborder="0" cellspacing="0">
                <tr>
                    <td align="left"><b>T1</b></td>
                    <td align="left" balign="left">Read<br/>Credentials</td>
                </tr>
            </table> >
        fillcolor = "#ff8888"
        style = "rounded, filled, dotted"
    ]

    subgraph cluster_processing_0 {
        # Processing 0
        penwidth = 0
        node [fillcolor = "#66aaff"]

        t2_setup [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T2</b></td>
                        <td align="left" balign="left">Setup<br/>Stream</td>
                    </tr>
                </table> >
            style = "rounded, filled, dotted"
        ]

        t2 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T2</b></td>
                        <td align="left" balign="left">Stream<br/>Records</td>
                    </tr>
                </table> >
        ]

        t3 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T3</b></td>
                        <td align="left" balign="left">Read<br/>Output</td>
                    </tr>
                </table> >
            style = "rounded, filled, dotted"
        ]
    }

    subgraph cluster_reporting_0 {
        # Reporting 0
        penwidth = 0
        node [fillcolor = "#66ff88"]

        t4 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T4</b></td>
                        <td align="left" balign="left">Start<br/>Progress Bar</td>
                    </tr>
                </table> >
            style = "rounded, filled, dotted"
        ]
    }

    subgraph cluster_web_1 {
        # Web
        penwidth = 0
        node [fillcolor = "#ff8888"]

        t5 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T5</b></td>
                        <td align="left" balign="left">Rate<br/>Limit</td>
                    </tr>
                </table> >
        ]

        t6 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T6</b></td>
                        <td align="left" balign="left">Authenticate</td>
                    </tr>
                </table> >
        ]

        t7 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T7</b></td>
                        <td align="left" balign="left">Retrieve<br/>Information</td>
                    </tr>
                </table> >
        ]
    }

    subgraph cluster_processing_1 {
        # Processing 1
        penwidth = 0
        node [fillcolor = "#66aaff"]

        t8 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T8</b></td>
                        <td align="left" balign="left">Augment<br/>Record</td>
                    </tr>
                </table> >
        ]

        t9 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T9</b></td>
                        <td align="left" balign="left">Output<br/>Record</td>
                    </tr>
                </table> >
        ]
    }

    subgraph cluster_reporting_1 {
        # Reporting 1
        penwidth = 0
        node [fillcolor = "#66ff88"]

        t10 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T10</b></td>
                        <td align="left" balign="left">Update<br/>Progress Bar</td>
                    </tr>
                </table> >
            fillcolor="#66ff88"
        ]

        t11 [
            label =
                < <table border="0" cellborder="0" cellspacing="0">
                    <tr>
                        <td align="left"><b>T11</b></td>
                        <td align="left" balign="left">Output<br/>Report</td>
                    </tr>
                </table> >
            fillcolor="#66ff88"
        ]
    }

    # Actual dependencies
    {
        edge [weight = 2]

        t0 -> t10 [arrowhead = ediamond, style = dashed, penwidth = 2, samehead = "progress_bar"]
        # t1 -> t5 [weight = 10]
        t2_setup -> t2 [weight = 10]
        t2 -> t5 [arrowhead = ediamond, style = dashed, penwidth = 2]
        t3 -> t4
        # t3 -> t9
        t5 -> t6 [weight = 10]
        t6 -> t7 [weight = 10]
        t7 -> t8 [arrowhead = ediamond, style = dashed, penwidth = 2]
        t8 -> t9 [weight = 10]
        t9 -> t10 [arrowhead = ediamond, style = dashed, penwidth = 2, samehead = "progress_bar"]
        t10 -> t11 [weight = 10]
    }

    {
        # Invisible edges for layout
        edge [ style = "invis" ]
        # t0_setup -> t0 [weight = 10]
        # t0_setup -> t4 [weight = 2]
        # t1 -> t2 [weight = 2]
        t1 -> t5 [weight = 10]
        # t2_setup -> t2 [ltail=cluster_processing_0, lhead=cluster_processing_1, weight = 10]
        t2 -> t4 [ltail=cluster_processing_0, lhead=cluster_reporting_0]
        t2 -> t8 [ltail=cluster_processing_0, lhead=cluster_processing_1, weight = 10]
        t4 -> t5 [ltail=cluster_reporting_0, lhead=cluster_web_1, weight = 4]
        t4 -> t10 [ltail=cluster_reporting_0, lhead=cluster_reporting_1, weight = 10, minlen = 3]
    }
}
```

* Step 1, 2, 3, and 4 are done synchronously, so are not executed concurrently / in parallel.
* The dashed lines indicate passing data to tasks in a different category. This may take the form of:

    - **`async` function call:**

        When a new instance of the task may be spawned each time.

        ```rust,edition2018,ignore
        pub struct Web(pub Credentials);
        impl Web {
            pub async fn retrieve_info(&self, _: &Record) -> PropertyInfo { .. }
        }

        // Invoked like so:
        web.retrieve_info(record).await;
        ```

    - **Channel Send/Receive:**

        When it is not possible to spawn multiple instances of the task, we use a long-lived task with a channel to receive data.

        ```rust,edition2018,ignore
        pub struct Reporter {
        #     pub progress_receiver: Receiver<ProgressUpdate>,
            pub report: Report,
        #     progress_overall: ProgressBar,
        }
        impl Reporter {
            pub async fn update(&mut self) {
                // When receiving a progress update from the channel.
        #         while let Some(process_result) = self.progress_receiver.recv().await {
        #             match process_result {
        #                 PropertyInfoResult::Success => {
                self.report.record_processed_successful_count += 1;
        #                 }
        #                 // ..
        #             }
        #             self.progress_overall.inc(1);
        #         }
            }
        }

        // Cannot do this per record because of `&mut self`:
        reporter.update(process_result).await;

        // Have to do this instead:
        progress_channel.send(process_result);
        ```
