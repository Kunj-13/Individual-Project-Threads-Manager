# Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing
# Project Overview & Implementations

For this project, I developed a comprehensive operating system simulation project. This endeavor aims to bridge theoretical knowledge with practical skills in managing processes and threads, implementing inter-process communication (IPC), and exploring parallel computing concepts. The key facets of this project include:

- **Multi-Process and Thread Management**: I built a system capable of managing multiple processes and threads. This component allows for a granular view of system resource utilization, displaying intricate details such as process IDs, thread statuses, and associated Light-Weight Processes (LWPs).
  
- **Inter-Process Communication (IPC)**: The project simulates IPC through two main mechanisms: shared memory and message passing. This simulation is conducted both within and across processes and threads, offering a deep dive into the nuances of process communication.
  
- **Parallel Text File Processing**: A significant part of the project is devoted to the parallel processing of text files. This system efficiently handles character transformation and frequency counting tasks by leveraging multiple threads or processes. The design focuses on minimizing execution time while maximizing resource utilization.

# Installation requirements and directions

1. You need Python 3.6 or later installed on your device if you dont have one download from this link:- https://www.python.org/downloads/.

2. Use the command below to clone from a repository:

```git clone https://github.com/Kunj-13/Individual-Project-Threads-Manager.git```

3. Navigate to the Project Directory:
```cd Individual-Project-Threads-Manager.git```

4. Run my Simulator:
```python3 main.py```

# Code structure 
- I will demonstrate and illustrate some key code structures for each main function below. They are snippits of code since it would be difficult to explain the whole code in the report.
  
## Multi-Process and Thread Manager
The multi-process and thread manager is implemented using several Python classes and functions that provide the capability to create, suspend, resume, view, and terminate processes and threads.

## Key Code Structures:
### Process Management:

- create_process(self, process_name): Forks a new process and runs a given program within it.
- list_processes(self): Lists all running processes with their PID, name, state, and parent PID.
- suspend_process(self, pid): Suspends a process using the SIGSTOP signal.
- resume_process(self, pid): Resumes a process using the SIGCONT signal.
- terminate_process(self, pid): Terminates a process using the SIGTERM signal.

### Thread Management:

- ControlledThread: A class that extends Python's threading.Thread to include pause and resume functionality.
- create_thread(self, thread_name): Creates and starts a new thread within the current process.
  
Code Snippet:

``` # Process suspension and resumption
def suspend_process(self, pid):
    os.kill(pid, signal.SIGSTOP)
    self.running_processes[pid]['state'] = ProcessState.SLEEPING 

def resume_process(self, pid):
    os.kill(pid, signal.SIGCONT)
    self.running_processes[pid]['state'] = ProcessState.RUNNING

# Thread creation within a process
def create_thread(self, thread_name):
    controlled_thread = ControlledThread(target=thread_func, name=thread_name)
    self.threads.append(controlled_thread)
    controlled_thread.start()
```

## Inter-Process Communication (IPC) Mechanisms
IPC mechanisms are used for processes and threads to communicate with each other. This involves sending and receiving messages, which can be either many short messages or a few long messages to evaluate performance.

### Key Code Structures:
**IPC with Pipes:**
- send_message(self): Sends a message to a child process via a pipe.
- receive_message(self, timeout): Receives a message from a child process with a specified timeout.
  
**IPC with Queues:**
- send_data(self): Sends data using a shared queue.
- receive_data(self): Receives data from a shared queue.

Code Snippet:

``` # Sending and receiving a message through IPC pipes
def send_message(self):
    self.pipe_conn.send(message)

def receive_message(self, timeout):
    if self.child_conn.poll(timeout):
        received_message = self.child_conn.recv()
```
## Parallel Text File Processing
Parallel text file processing involves reading a large text file, dividing it into segments, and using multiple processes to process each segment concurrently.

### Key Code Structures:
**Parallel Processing:**
- parallel_text_processing(self, file_path, num_processes): Divides the text file into segments and creates a process for each segment.
- process_file_segment(self, file_path, start, end, shared_counter): Processes a segment of the file, counts character occurrences, and converts characters to uppercase.
  
Code Snippet:

``` # Parallel text file processing across multiple processes
def parallel_text_processing(self, file_path, num_processes):
    # Divide file and distribute work among processes
    segment_size = file_size // num_processes
    for i in range(num_processes):
        p = multiprocessing.Process(target=self.process_file_segment, args=(...))
        processes.append(p)
        p.start()
    for p in processes:
        p.join()
```

# Full Functionality Test and Verficiation
- I divided all 12 functions into 3 parts for the report so it is easier to understand.

## Process and Thread Management
- The following tests below ensure that the operating system simulator can handle the creation, suspension, and termination of processes and threads, as well as display all current processes.

## 1. **Create a new process**
### Test Procedure:
- Start the Operating System Simulator by running the main script.
- To create a new process, select "1. Create a new process" from the main menu by simply typing "1" and hitting enter.
- Then, you will get the prompt to enter the name of the new process.
  
<img width="477" alt="Screenshot 2024-03-09 at 1 57 43 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/cfa5a079-a095-49b2-8011-9828e574b927">
<img width="696" alt="Screenshot 2024-03-09 at 1 59 01 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/1e47ea85-a00a-44ed-ad3b-013f5a007898">
<img width="454" alt="Screenshot 2024-03-09 at 1 59 43 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/a699a069-6a10-4e7b-8adc-f2601fd62cc1">  

- Expected Result: A new child process is created.
- Backend Operation: The **create_process** function is called, which uses **os.fork()** to create a new process, then attempts to run a specified command within that new process. Details are logged and state is managed within the **running_processes** dictionary.  

  
## 2. **Suspend a process**
### Test Procedure:
- To suspend a process, select "2. Suspend a process" from the main menu by simply typing "2" and hitting enter.
- Then, you will get the prompt to enter the process id for whatever process you want to suspend.
  
<img width="457" alt="Screenshot 2024-03-09 at 2 00 49 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/b835c133-b9a4-45b1-ac20-7579ff675093">
<img width="461" alt="Screenshot 2024-03-09 at 2 01 49 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/6061e6f9-32e4-47c8-9239-01a51abc4140">  

- Expected Result: The specified process is suspended.
- Backend Operation: The **suspend_process** function sends a **SIGSTOP** signal to the given process ID, changing its state to **SLEEPING** within the program's tracking structures.


## 3. **Resume a process**
### Test Procedure: 
- To resume a process, select "3. Resume a process" from the main menu by simply typing "3" and hitting enter.
- Then, you will get the prompt to enter the process id for whatever process you want to resume.
  
<img width="389" alt="Screenshot 2024-03-09 at 2 02 54 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/d34069c6-1692-41ac-9512-5f2e0aa99bea">
<img width="470" alt="Screenshot 2024-03-09 at 2 03 09 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/90978a5c-4456-4441-98d4-4fe8194598ae">

- Expected Result: The process which you entered the process id for is resumed.
- Backend Operation: The **resume_process** function sends a **SIGCONT** signal to the suspended process, updating its state to **RUNNING** again.

## 4. **View all processes**
### Test Procedure: 
- To view all processes, select "4. View all processes" from the main menu by simply typing "4" and hitting enter.
  
<img width="470" alt="Screenshot 2024-03-09 at 2 03 09 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/90978a5c-4456-4441-98d4-4fe8194598ae">

- Expected Result: A list of all processes and their states is displayed.
- Backend Operation: The **list_processes** function iterates through the **running_processes** dictionary, printing out details for each tracked process.


## 5. **Terminate a process**
### Test Procedure: 
- To terminate a process, select "5. terminate a process" from the main menu by simply typing "5" and hitting enter.
- Then, you will get the prompt to enter the process id for whatever process you want to terminate.

<img width="443" alt="Screenshot 2024-03-09 at 2 05 11 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/96bc5867-99af-4d2c-90f1-32ab138fabfb">

<img width="454" alt="Screenshot 2024-03-09 at 2 05 32 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/71cde65b-e525-47bc-9bb0-0bab3960c763">

- Expected Result: The process which you entered the process id for is terminated.
- Backend Operation: The **terminate_process** function sends a **SIGTERM** signal to the specified process, removing it from the **running_processes** tracking.


## 6. **Initiate a new thread**
### Test Procedure:
- To create a new thread, select "6. Initiate a new thread" from the main menu by simply typing "6" and hitting enter.
- Then, you will get the prompt to enter the name for a new thread you want to create.

![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/b7a30507-35c7-435f-be39-e268206dc92e)

![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/5918695c-f22c-4768-bd20-df976be19a43)

- **Expected Result**: A new thread is initiated within the current process.
- **Backend Operation**: The **create_thread** function instantiates an instance of **ControlledThread**, which starts a new thread running the specified target function.

## Inter-Process Communication (IPC)
### Test Description:
This test will assess the IPC capabilities of the Process Manager to send and receive messages and data between processes.

## 7. **IPC: Send Message to child processes**
### Test Procedure:
- To send message to child processes, select "7. IPC: Send message to child processes" from the main menu by simply typing "7" and hitting enter.
- Then, you will get the prompt to enter the message you want to send to child processes.

## Short Message
![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/fdcbfe62-bb58-4e4f-ab91-ad85a3471cab)

## Long Message
![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/e1c2ed24-f1af-421c-8f33-330ccf8e502d)

-**Expected Result**: A message is sent to child processes.
-**Backend Operation**: The **send_message** function writes a message to an IPC pipe, which can be read by a child process on the other end of the pipe.

## **Performance Evaluation** 
### Short Message Performance:

- **Message**: "hi its kunj patel and i'm just testing to see how different the performance is between short messages and really long."
- **Time Taken**: 30.07318353652954 seconds
- **CPU Usage Change**: 37.6%
- **Memory Usage Change**: 1.0%
  
### Long Message Performance:

- **Message**: "Hi this is Kunj again and now I just want to test if I sent very long message does it change the performance. i am vry curious . so during the break i watchedthe movie dune which was very amazing and i want to know what did you do during the break?"
- **Time Taken**: 80.29867553710938 seconds
-** CPU Usage Change**: Approximately 8.8%
-** Memory Usage Change**: 15.5%

### Analysis:
From the data, I observed a significant increase in both the time taken and memory usage when processing the long message compared to the short message. The long message resulted in nearly double the processing time and a more substantial increase in memory usage. This could indicate that the system's performance scales with the size of the message, which is typical behavior as larger data sizes require more processing power and memory to handle. However, the CPU usage change seems to be less with the long message. This might be due to the nature of the processing taking place, where CPU usage may not scale linearly with message size or due to other concurrent processes affecting CPU usage during the test.


## 8. **IPC: Fetch Message from child processes**
### Test Procedure:
- To fetch message from child processes, select "8. IPC: Fetch message from child processes" from the main menu by simply typing "8" and hitting enter.
  

![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/fa80700f-756e-4916-9f2b-6bcf5ff51052)

- **Expected Result**: A latest message is received from a child process.

- **Backend Operation**: The **receive_message** function polls the IPC pipe for messages from child processes, printing any messages that are received.

## 9. **IPC: Queue - Send data**
### Test Procedure:
- To send data using the queue system, select "9. IPC: Queue - Send data" from the main menu by simply typing "9" and hitting enter.
  
![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/44f1bc4f-653e-4765-b6a2-40d8314111d7)

- **Expected Result**: Data is placed in a queue for IPC.
- **Backend Operation**: The **send_data** function uses a multiprocessing queue to send a piece of data, which is randomly generated within the function.
  
## 10. **IPC: Queue - Receive data**
### Test Procedure:
- To fetch message from child processes, select "10. IPC: Queue - Receive data" from the main menu by simply typing "10" and hitting enter
  
![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/a98c5a30-0309-43a8-9083-a1fcda9a6fac)

- **Expected Result**: Data is received from the IPC queue.
- **Backend Operation**: The **receive_data** function checks a multiprocessing queue for any data sent by other processes and retrieves it.

### Evaluation Performance:-
- The CPU usage for sending data is slightly lower than for receiving data. This indicates that the operation of putting data into the queue is less intensive than taking data out, which may involve additional overhead for synchronization and possibly waking up sleeping processes.
- Memory usage for sending data showed a small increase, while receiving data showed no change. This might be due to memory allocation when sending data and simply reading the data when receiving, without allocating new memory.
- The differences in CPU usage are relatively close, suggesting that the queue operations have been optimized for similar performance characteristics, regardless of whether data is being sent or received.

## Parallel Text File Processing
### Test Description:
The test checks the Process Manager's ability to efficiently perform parallel text file processing between big file vs short file.

## 11. **Parallel Text File Processing**
### Test Procedure:
- First of all, import the text file you want to process into your main project.
- To process the content of a text file, select "11. Parallel Text File Processing" from the main menu by simply typing "7" and hitting enter.
- Then, you will get the prompt to enter the file path. After entering the valid file path, the results will be displayed. 

## Small Text File
<img width="441" alt="Screenshot 2024-03-09 at 1 53 23 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/ad732391-e95b-4c4a-9803-bca3bac6c6fb">
<img width="441" alt="Screenshot 2024-03-09 at 1 52 51 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/d5c03698-fe14-4ebc-9792-fd3504bf67c5">


## Large Text File
<img width="441" alt="Screenshot 2024-03-09 at 1 48 37 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/a65b796f-a804-4e07-a423-7782c4ed5e39">
<img width="441" alt="Screenshot 2024-03-09 at 1 49 18 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/d738e75a-5c35-4680-a09b-1e8a3450c011">


- **Expected Result**: The text file is processed in parallel, characters are converted to uppercase, and a count of each character is provided. The system should display performance metrics, such as execution time and CPU/memory usage.
- **Backend Operation**: The **parallel_text_processing** function divides a file into segments and uses multiple processes to process each segment, with each process counting character occurrences using a shared manager dictionary.

## Evaluation Performance
### **Performance for Small Text File (small text smp.txt):**
- **Execution Time:** 2.1806066036224365 seconds
- **Initial CPU Usage:** 48.1%
- **Final CPU Usage:** 44.8%
- **Initial Memory Usage:** 17278.93 MB
- **Final Memory Usage:** 17284.30 MB
- **Memory Usage Change:** 5.37 MB

### **Performance for Large Text File (dune 3 test.txt):**
- **Execution Time:** 7.263701677322388 seconds
- **Initial CPU Usage:** 38.3%
- **Final CPU Usage:** 53.2%
- **Initial Memory Usage:** 16971.70 MB
- **Final Memory Usage:** 16979.49 MB
- **Memory Usage Change:** 7.79 MB

### **Analysis:**
- The execution time for processing the larger file is significantly higher, as expected due to the increased amount of data.
- CPU usage increased when processing the larger file, indicating more intensive computation required for larger datasets.
- Memory usage also saw a greater increase with the larger file, which is consistent with the need for additional memory to manage more data.
- The system shows good scalability, with the increase in execution time and resource usage being reasonable given the likely larger size of the "dune 3 test.txt" file compared to "small text smp.txt".

## 12. **Exit**
### Test Procedure:
- To exit, select "12. Exit" from the main menu by simply typing "12" and hitting enter.
- Then, you will exit from the program.

![image](https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/84eaf3b8-5db4-4fdc-9110-01a31f615fb4)

- **Expected Result:** The program exits.
- **Backend Operation**: The loop running the main menu is terminated, ending the program execution.

# **Processes.log**

I have also attached the processes.log file which is a record of the activities and events that occur within my Operating System Simulator. It's generated by the simulator to help user track the behavior of processes and threads, and it captures key actions performed during the operation of the simulator.

The log file contains timestamped entries for each significant event, such as:

- **Creation of a process or thread:** Records when a new process or thread is started, along with its identification details.
- **State changes**: Notes any changes in the state of a process, like transitioning from running to sleeping or vice versa.
- **Errors**: Captures any errors that occur, particularly during the creation or execution of a process or thread.
- **Termination**: Logs when a process or thread is terminated, either normally or due to an error.
- **IPC actions**: Any inter-process communication actions, like sending or receiving messages, are noted.

# Limitations, Challenges & Findings

## Findings
Throughout the course of developing this operating system simulator, I gained valuable insights into the complexity of managing processes and threads, the intricacies of inter-process communication, and the benefits as well as challenges of parallel computing. One of my key findings was the substantial impact of parallel processing on performance. By splitting a large text file into segments and processing each segment concurrently, the execution time was significantly reduced compared to sequential processing. However, this came with the increased complexity of managing multiple processes, ensuring data consistency, and dealing with the overhead of context switching.

## Challenges
During implementation, I faced several challenges:
- **Synchronizing Threads:** Implementing thread synchronization without causing deadlocks or race conditions was intricate. I had to carefully manage shared resources using locks and semaphores to maintain data integrity.
- **IPC Mechanisms:** Balancing the performance of message passing with shared memory was complex, especially when trying to optimize for different sizes of data. It was crucial to find a sweet spot where the system would not be bogged down by overhead or by the latency of data transfer.
- **Memory Management:** Keeping track of memory usage was crucial, particularly when handling large files. It was challenging to ensure that the memory was allocated and freed appropriately to prevent leaks.

## Limitations
Some limitations and areas for improvement include:
- **User Interface:** While functional, the CLI-based interface could be more intuitive. Incorporating a graphical user interface would make it easier for users to interact with the simulator and understand the processes and threads in real time.
- **Scalability:** While the simulator handles parallel processing well, further scalability tests are needed with larger datasets and more complex processing tasks to ensure that the system scales linearly.
- **Performance Profiling:** More detailed profiling could help identify bottlenecks in the system. This data would be invaluable for optimizing various components, especially the IPC mechanisms.
- **Extended Functionality:** Currently, the simulator operates on text files for parallel processing. Expanding this to other types of data and operations could broaden the learning experience.
  
This project not only solidified my understanding of operating systems but also improved my problem-solving and system design skills. The challenges I faced pushed me to explore and learn more about the underlying mechanics of an operating system. Moving forward, I plan to refine this project, making it more robust and user-friendly, and possibly extending its functionality to simulate more complex operating system tasks.
