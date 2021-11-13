## Overview
Airflow exposes a number of parameters that are closely related to DAG and task-level performance.  
These fall into 3 categories:

* Environment-level settings
* DAG-level settings
* Operator-level settings

### Environment-level Airflow Settings
|airflow.cfg name| Environment Variable | Default Value |
| --- | --- | --- |
|parallelism|AIRFLOW__CORE__PARALLELISM|32|
|dag_concurrency|AIRFLOW__CORE__DAG_CONCURRENCY|16|
|worker_concurrency|AIRFLOW__CELERY__WORKER_CONCURRENCY|16|
|parsing_processes|AIRFLOW__SCHEDULER__PARSING_PROCESSES|2|
|max_active_runs_per_dag|AIRFLOW__CORE__MAX_ACTIVE_RUNS_PER_DAG|16|

* **parallelism** is the maximum number of tasks that can run concurrently within a single Airflow environment. If this setting is set to 32, no more than 32 tasks can run at once across all DAGs. Think of this as "maximum active tasks anywhere." If you notice that tasks are stuck in a queue for extended periods of time, this is a value you may want to increase.

* **dag_concurrency** determines how many task instances the Airflow Scheduler is able to schedule at once per DAG. Think of this as "maximum tasks that can be scheduled at once, per DAG."

*If you increase the amount of resources available to Airflow (such as Celery Workers or Kubernetes Pods) and notice that tasks are still not running as expected, you might have to increase the values of both **parallelism** and **dag_concurrency**.*

* **worker_concurrency** is only applicable to users running the Celery Executor. It determines how many tasks each Celery Worker can run at any given time. If this value is not set, the Celery Executor will run a maximum of 16 tasks concurrently by default.

*This number is limited by **dag_concurrency**. If you have 1 Worker and want it to match your environment's capacity, worker_concurrency should be equal to parallelism. If you increase **worker_concurrency**, you might also need to provision additional CPU and/or memory for your Workers.*

* **parsing_processes** is the maximum number of threads that can run on the Airflow Scheduler at once. Increasing the value of parallelism results in additional strain on the Scheduler, so we recommend increasing **parsing_processes** as well. If you notice a delay in tasks being scheduled, you might need to increase this value or provision additional resources for your Scheduler.

* **max_active_runs_per_dag** determines the maximum number of active DAG Runs (per DAG) the Airflow Scheduler can handle at any given time. If this is set to 16, that means the Scheduler can handle up to 16 active DAG runs per DAG. In Airflow, a DAG Run represents an instantiation of a DAG in time, much like a task instance represents an instantiation of a task.

![3 major concurrency](https://i.stack.imgur.com/P9jbQ.png)

### DAG-level Airflow Settings
There are two primary DAG-level Airflow settings users can define in code:

* **max_active_runs** is the maximum number of active DAG Runs allowed for the DAG in question. Once this limit is hit, the Scheduler will not create new active DAG Runs. If this setting is not defined, the value of max_active_runs_per_dag (described above) is assumed.
```
# Allow a maximum of 3 active runs of this DAG at any given time
dag = DAG('my_dag_id', max_active_runs=3)
```
* **concurrency** is the maximum number of task instances allowed to run concurrently across all active DAG runs of the DAG for which this setting is defined. This allows you to set 1 DAG to be able to run 32 tasks at once, while another DAG might only be able to run 16 tasks at once. If this setting is not defined, the value of dag_concurrency (described above) is assumed.

```
# Allow a maximum of concurrent 10 tasks across a max of 3 active DAG runs
dag = DAG('my_dag_id', concurrency=10,  max_active_runs=3)
```
### Task-level Airflow Settings
There are two primary task-level Airflow settings users can define in code:


* **pool** is a way to limit the number of concurrent instances of a specific type of task. This is great if you have a lot of Workers or DAG Runs in parallel, but you want to avoid an API rate limit or otherwise don't want to overwhelm a data source or destination. For more information, read Pools in Airflow's documentation.
* **task_concurrency** is a limit to the amount of times the same task can execute across multiple DAG Runs.

```
t1 = PythonOperator(pool='my_custom_pool', task_concurrency=14)
```

### Airflow Pools Best Practices
Let's assume an Airflow environment with the Celery Executor uses the default *airflow.cfg* settings above. Let's also assume we have a DAG with 50 tasks that each pull data from a REST API. When the DAG starts, 16 Celery Workers will be accessing the API at once, which may result in throttling errors from the API. What if we want to limit the rate of tasks, but only for tasks that access this API?

Using pools, we can control how many tasks can run across a specific subset of DAGs. If we assign a single pool to multiple tasks, the pool ensures that no more than a given number of tasks between the DAGs are running at once. As you scale Airflow, you'll want to use pools to manage resource usage across your tasks.

