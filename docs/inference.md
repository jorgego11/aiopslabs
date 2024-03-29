
# Log Anomaly Inference

## Recap

Lets recap what we have done so far:

* We have defined an integration to a Log Aggregator (EFK) that received historical application logs for a "normal" day of operations of a sample application called **qotd**.

* We have defined an integration to Instana which is observing the environment where the **qotd** application is running and we have pulled every resource topology information related to this application into CP4WAIOps.

* We have enabled one CP4WAIOps policy to create Stories.

* We have created a log anomaly model by training on one day of "normal" historical log data from the **qotd** application. We have deployed this model so we can actually test it with real time log data.  



## Start Log Anomaly Inference on real-time Logs

Now that our log anomaly trained model is successfully deployed, its ready to start doing log anomaly inference. That is, "scan" logs coming in real time from the sample application *qotd* and create alerts when the real time logs deviate from what is considered "normal" in the trained model. 
The final important point here is that, during the Lab execution, we are injecting real time anomalies into the sample application qotd, for example we are shutting down API services, increasing service responce latency, etc. This concept is similar to what the IT Ops team at Netflix call [Chaos Monkey](https://netflix.github.io/chaosmonkey/). These anomalies will be detected by the CP4WAIOps and you will see the new alerts and stories created.

We are going to modify our integration with the log aggregator to start consuming real-time logs instead historical logs as we did previously.

* From the Home page, under `Overview` clik on `Data and tool connections` on the left side of the page. 

* Click on the `ELK` connection type. 

* Click on the `EFK for QOTD` connection definition.

* Click the `Next` button twice to arrive to the `AI training and log data` page.

* In the `AI training and log data` page:

      * Make sure the Data flow is set to `On` (green).

      * Choose `Live data for continuous AI training and anomaly detection` as the option for training mode.

* The `AI training and log data` page should look like the screenshot below:

![Inference](./images/aiops-log-anomaly-training-15.png)

* Finally click on the `Save` button. 

After saving the configuration, make sure the ELK integration page shows the Data Flow Status as `Running` as shown below. **Note that it may take up to 1-2 minutes to show this message.**

![elk integration 7](./images/elk-integration-77.png "ELK integration 7")



---

## Verify Incident Story

After deploying the log anomaly model and enabling real-time log consumption, we need to wait a few minutes to see the new alerts and stories being created.

* From the Home page, under `Overview` clik on `Stories and alerts` on the left side of the page. 

* On the Stories tab, make sure you see one or more stories created, as shown below. Confirm that the story has been recently created. If you don't see any stories, just wait a few minutes. 

* As we have mentioned before during the definition of the terms, from an IT Operations point of view, we have `Events` where some will become `Alerts` that get correlated and will trigger a policy that will trigger the creation of `Stories`. 

![Inference](./images/aiops-log-anomaly-inference-1.png)

* **Alerts tab:** Click on the Alerts tab. Here you will see one or more alerts, as shown below:


![Inference](./images/aiops-log-anomaly-inference-2.png)


These are the Log Anomaly alerts that were created by the Log Anomaly model we just deployed. They were created because the real-time logs shown patterns that are not considered "normal" by the trained model. 

  * Click on the first alert under the Summary column to explore the alert details on the right side of the page. 

      * On the Alert details under Properties review the values of the different properties. 

      * Look at the value of the property `eventCount`, this is the number of events that were deduplicated into a single Alert. 


* **Stories tab:**  Click back on the Stories tab, these are the stories that were created based on the alerts that you just saw before. In a real production environment, a story can group and correlate different but related alerts into a single story page. For example, lets imagine that the storage of a key application database goes down. In this scenario, we could have alerts from the database monitoring platform, log anomaly alerts from the APIs that use the database and web page response alerts that shows database data. All these three alerts will be combined into a single story in order to help the IT Operations personnel find the root cause of the incident. 


      * Click on the story Description to look at the story details. You will see the story page. **Make sure to follow the pop-up page tour.**

![Inference](./images/aiops-log-anomaly-inference-3.png)

* **Story description:**

      * On the left side, we see the Top alerts that triggered this story. These alerts are ranked by probable cause showing the top one as the most probable root cause of the overall incident.

      * In the middle of the page, we see topology information that was provided by Instana with the problematic resource highlighted. 

      * The top right side of the page shows the StoryID, the story owner (Unassigned) and the priority. 
      * The `Change story settings` button on the top right allow us to change the priority, the status and assignee.  
      * The Alerts tab shows only the alerts related to this particular story.
      * The Topology tab shows the overall topology context where this incident happen.


Finally, go back to the Home page and see how the different information cards get populated with data, now that we have anomalies and stories created:

![Home Page](./images/home-page-90.png)

**CONGRATULATIONS, THIS IS THE END OF THE LAB**

---



