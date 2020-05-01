# Implement a Data Streaming Solution with Azure Streaming Analytics

There are two approaches to processing data streams: **on-demand and live.**

- **Event producer**: Examples include sensors or processes that generate data continuously, such as a heart rate monitor or a highway toll lane sensor.
- **Event processor**: An engine to consume event data streams and derive insights from them. Depending on the problem space, event processors either process one incoming event at a time, such as a heart rate monitor, or process multiple events at a time, such as Azure Stream Analytics processing the highway toll lane sensor data.
- **Event consumer**: An application that consumes the data and takes specific action based on the insights. Examples of event consumers include alert generation, dashboards, or even sending data to another event processing engine.

![Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/3-event-processing-engine.png](Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/3-event-processing-engine.png)

![Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/4-stream-analytics-components.png](Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/4-stream-analytics-components.png)

## **Operational aspects**

Stream Analytics guarantees *exactly once* event processing and *at-least-once* event delivery, so events are never lost. It has built-in recovery capabilities in case the delivery of an event fails. Also, Stream Analytics provides built-in checkpointing to maintain the state of your job and produces repeatable results.

In Azure Stream Analytics, a *job* is a unit of execution. A Stream Analytics job pipeline consists of three parts:

- An **input** that provides the source of the data stream.
- A **transformation query** that acts on the input. For example, a transformation query could aggregate the data.
- An **output** that identifies the destination of the transformed data.

    ![Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/2-stream-analytics-pipeline.png](Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/2-stream-analytics-pipeline.png)

- **Data Processing Challenges**
    - Has to support high volume data ingestion
    - enough processing power
    - output storage has to have high bandwidht
- **Stack**
    - HDInsight with Spark Streaming
    - Apache Spark in Azure Databricks
    - HDInsight with Storm
    - Azure Functions
    - WebJobs
    - Azure Stream Analytics
- **Azure Stream Analytics**
    - managed service
    - Event Hubs / IoT Hub / Blob Store / Reference Data  → Azure Stream Analytics /  Azure ML → Azure Blobs, Data Lakes Gen 01 or 02 / Azure SQL DB and DW / Event Hubs / Power BI

- **Time Windowing**
    - each data has a timestamp
    - Tumbling Window

        ```sql
        SELECT COUNT(*)
        FROM Input
        GROUP BY TumblingWindow(second, 5)
        # windows size
        ```

    - Hopping Window

        ```sql
        SELECT COUNT(*)
        FROM Input
        GROUP BY TumblingWindow(second, 10, 5)
        # unit of time, size of window, size of hopping
        ```

        ![Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/Screen_Shot_2020-03-30_at_7.29.44_PM.png](Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/Screen_Shot_2020-03-30_at_7.29.44_PM.png)

    - Sliding Window

        ![Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/Screen_Shot_2020-03-30_at_7.31.09_PM.png](Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/Screen_Shot_2020-03-30_at_7.31.09_PM.png)

    - Session Window

        []()

- **Arrival Time and Event Time**
    - default time arrival time
    - can use Event time functions to define the actual event time
- **Event Ordering Policies**
    - Later arrival policy - accept events within the policy window
    - Out of order policy -

        ![Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/Screen_Shot_2020-03-30_at_8.00.12_PM.png](Implement%20a%20Data%20Streaming%20Solution%20with%20Azure%20Str/Screen_Shot_2020-03-30_at_8.00.12_PM.png)