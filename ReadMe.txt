This is understanding the Parllel Programming project.

Also be Using Parameterized Builds in Jenkins.

How to trigger multiple build job to run concurrently.

waiting for parallel execution to complete and then proceed further with passing 
the results to next stage.

This project demonstrates how to set up and manage parallel execution of tasks in a build pipeline.

The key features include:
- Defining multiple jobs to run in parallel.
- Synchronizing the completion of parallel tasks.
- Passing results from parallel tasks to subsequent stages.

DEV  →  QA1 & QA2 (parallel)  →  PROD
To set up this project, follow these steps:

Design idea (simple words)

DEV runs first

QA1 and QA2 run at the same time
PROD runs only after QA1 & QA2 both succeed
You control behavior using parameters

Everything is inside one Jenkinsfile