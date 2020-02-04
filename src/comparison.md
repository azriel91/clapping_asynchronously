# Comparison


| Item                            | Synchronous                     | Asynchronous                    |
| ------------------------------- | ------------------------------- | ------------------------------- |
| Logic Network                   | Simple network of complex tasks | Complex network of simple tasks |
| Effort For Efficiency           | Explicitly managed              | Natually enforced by the code   |
| Library Availability / Maturity | Generally exist                 | Generally do not exist          |

<p></p>

<details>

<summary><b>Logic Network</b></summary>

* **sync:** Simple network of complex tasks

    ```dot process Logic Network Synchronous
    digraph {
        compound = true
        nodesep = 0.02
        ranksep = 1.0
        node [
            fontname="Helvetica"
            fontsize=10
            width=0.5
            shape=rectangle
            style="rounded, filled, dotted"
        ]

        subgraph cluster_setup {
            # Setup
            label = < <b>Setup</b> >
            fontname="Helvetica"
            fontsize=10
            style = "rounded, dashed"
            node [fillcolor = "#66aaff"]

            t4 [label = < <b>T4</b> >, fillcolor = "#66ff88"]
            t3 [label = < <b>T3</b> >, fillcolor = "#66aaff"]
            t2 [label = < <b>T2</b> >, fillcolor = "#66aaff"]
            t1 [label = < <b>T1</b> >, fillcolor = "#ff8888"]
            t0 [label = < <b>T0</b> >, fillcolor = "#ffcc44"]
        }

        subgraph cluster_process {
            # Process
            label = < <b>Process</b> >
            fontname="Helvetica"
            fontsize=10
            style = "rounded, dashed"
            node [fillcolor = "#66aaff"]

            t10 [label = < <b>T10</b> >, fillcolor = "#66ff88"]
            t9 [label = < <b>T9</b> >, fillcolor = "#66aaff"]
            t8 [label = < <b>T8</b> >, fillcolor = "#66aaff"]
            t7 [label = < <b>T7</b> >, fillcolor = "#ff8888"]
            t6 [label = < <b>T6</b> >, fillcolor = "#ff8888"]
            t5 [label = < <b>T5</b> >, fillcolor = "#ff8888"]
        }

        subgraph cluster_report {
            # Report
            label = < <b>Report</b> >
            fontname="Helvetica"
            fontsize=10
            style = "rounded, dashed"
            node [fillcolor = "#66aaff"]

            t11 [label = < <b>T11</b> >, fillcolor = "#66ff88"]
        }

        t4 -> t5 [ltail=cluster_setup, lhead=cluster_process]
        t10 -> t11 [ltail=cluster_process, lhead=cluster_report]
    }
    ```

* **async:** Complex network of simple tasks

    ```dot process Logic Network Asynchronous
    digraph {
        compound = true
        nodesep = 0.02
        ranksep = 0.2
        node [
            fontname="Helvetica"
            fontsize=10
            width=0.5
            shape=rectangle
            style="rounded, filled"
        ]

        t0_setup [
            label =
                < <b>T0</b> >
            fillcolor = "#ffcc44"
            style = "rounded, filled, dotted"
        ]

        t0 [
            label =
                < <b>T0</b> >
            fillcolor = "#ffcc44"
        ]

        t1 [
            label =
                < <b>T1</b> >
            fillcolor = "#ff8888"
            style = "rounded, filled, dotted"
        ]

        subgraph cluster_processing_0 {
            # Processing 0
            penwidth = 0
            node [fillcolor = "#66aaff"]

            t2_setup [
                label =
                    < <b>T2</b> >
                style = "rounded, filled, dotted"
            ]

            t2 [
                label =
                    < <b>T2</b> >
            ]

            t3 [
                label =
                    < <b>T3</b> >
                style = "rounded, filled, dotted"
            ]
        }

        subgraph cluster_reporting_0 {
            # Reporting 0
            penwidth = 0
            node [fillcolor = "#66ff88"]

            t4 [
                label =
                    < <b>T4</b> >
                style = "rounded, filled, dotted"
            ]
        }

        subgraph cluster_web_1 {
            # Web
            penwidth = 0
            node [fillcolor = "#ff8888"]

            t5 [
                label =
                    < <b>T5</b> >
            ]

            t6 [
                label =
                    < <b>T6</b> >
            ]

            t7 [
                label =
                    < <b>T7</b> >
            ]
        }

        subgraph cluster_processing_1 {
            # Processing 1
            penwidth = 0
            node [fillcolor = "#66aaff"]

            t8 [
                label =
                    < <b>T8</b> >
            ]

            t9 [
                label =
                    < <b>T9</b> >
            ]
        }

        subgraph cluster_reporting_1 {
            # Reporting 1
            penwidth = 0
            node [fillcolor = "#66ff88"]

            t10 [
                label =
                    < <b>T10</b> >
                fillcolor="#66ff88"
            ]

            t11 [
                label =
                    < <b>T11</b> >
                fillcolor="#66ff88"
            ]
        }

        # Actual dependencies
        {
            t0 -> t10 [arrowhead = ediamond, style = dashed, penwidth = 2]
            t2_setup -> t2 [weight = 10]
            t2 -> t5 [arrowhead = ediamond, style = dashed, penwidth = 2]
            t3 -> t4
            t5 -> t6 [weight = 10]
            t6 -> t7 [weight = 10]
            t7 -> t8 [arrowhead = ediamond, style = dashed, penwidth = 2]
            t8 -> t9 [weight = 10]
            t9 -> t10 [arrowhead = ediamond, style = dashed, penwidth = 2]
            t10 -> t11 [weight = 10]
        }

        {
            # Invisible edges for layout
            edge [ style = "invis" ]
            t0_setup -> t0 [weight = 10]
            t1 -> t5 [weight = 10]
            t2 -> t4 [ltail=cluster_processing_0, lhead=cluster_reporting_0]
            t2 -> t8 [ltail=cluster_processing_0, lhead=cluster_processing_1, weight = 10]
            t4 -> t5 [ltail=cluster_reporting_0, lhead=cluster_web_1, weight = 4]
            t4 -> t10 [ltail=cluster_reporting_0, lhead=cluster_reporting_1, weight = 10, minlen = 3]
        }
    }
    ```

</details>
