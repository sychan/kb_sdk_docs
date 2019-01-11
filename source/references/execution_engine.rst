Job Execution
====================
Your app runs in a docker container an a shared worker node and if it misbehaves it can affect other jobs on the machine.
For the regular queue, we ask you to limit to your app to 4 cores to allow more jobs to run on a machine but you can use a large amount of memory, up to 20 gb.
Higher memory is potentially available by contacting us and requesting your app run in a high resource queue.

Job Queues
-----------------------
Currently resources are controlled for apps by specifying which queue a job may run in:

- Regular Queue - 16 core machines with 100Gb Memory
- Large Memory Queue - 32 core machines with 256Gb Memory
- Very Large Memory Queue - 6TB and 320 Cores


