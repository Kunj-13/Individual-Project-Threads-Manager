# Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing
# Project Overview & Implementations

For this project, I developed a comprehensive operating system simulation project. This endeavor aims to bridge theoretical knowledge with practical skills in managing processes and threads, implementing inter-process communication (IPC), and exploring parallel computing concepts. The key facets of this project include:

- **Multi-Process and Thread Management**: I built a system capable of managing multiple processes and threads. This component allows for a granular view of system resource utilization, displaying intricate details such as process IDs, thread statuses, and associated Light-Weight Processes (LWPs).
  
- **Inter-Process Communication (IPC)**: The project simulates IPC through two main mechanisms: shared memory and message passing. This simulation is conducted both within and across processes and threads, offering a deep dive into the nuances of process communication.
  
- **Parallel Text File Processing**: A significant part of the project is devoted to the parallel processing of text files. This system efficiently handles character transformation and frequency counting tasks by leveraging multiple threads or processes. The design focuses on minimizing execution time while maximizing resource utilization.

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

<img width="443" alt="Screenshot 2024-03-09 at 2 05 11 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/96bc5867-99af-4d2c-90f1-32ab138fabfb">

<img width="454" alt="Screenshot 2024-03-09 at 2 05 32 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/71cde65b-e525-47bc-9bb0-0bab3960c763">

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


7. **Create a new process**
### Test Procedure:
8. **Create a new process**
### Test Procedure:
9. **Create a new process**
### Test Procedure:



























3. 
<img width="441" alt="Screenshot 2024-03-09 at 1 48 37 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/a65b796f-a804-4e07-a423-7782c4ed5e39">
<img width="441" alt="Screenshot 2024-03-09 at 1 49 18 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/d738e75a-5c35-4680-a09b-1e8a3450c011">

<img width="441" alt="Screenshot 2024-03-09 at 1 53 23 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/ad732391-e95b-4c4a-9803-bca3bac6c6fb">
<img width="441" alt="Screenshot 2024-03-09 at 1 52 51 AM" src="https://github.com/Kunj-13/Multi-Process-Threads-Manager-with-IPC-and-Parallel-Text-File-Processing/assets/143433713/d5c03698-fe14-4ebc-9792-fd3504bf67c5">

