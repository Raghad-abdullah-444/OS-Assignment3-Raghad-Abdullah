# Assignment 3 - Complete Documentation

**Student Name**: Raghad Abdullah Alharbi

**Student ID**: 444052811

**Date Submitted**: 4 may 2026

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Date 4 May, Time 3:30]
**What I implemented**: open github and take the code to run on VS code

**Challenges encountered**: I thought that I will use the same code after updated

**How I solved it**: I read again the assginmnent requirment 

**Testing approach**: --

**Time spent**: half hour

---

### Entry 2 - [Date 4 May , Time4:00]
**What I implemented**: add all locks with its name to manage access  

**Challenges encountered**:take some time

**How I solved it**: try to solve it quikly

**Testing approach**: try to put each lock and understand how to put them

**Time spent**: 1hour

---

### Entry 3 - [Date 4 May, Time 5:00]
**What I implemented**: I complete to put or to test locks by putting try and catch for each responsible method in the code 

**Challenges encountered**: take time to to correct error and foucs on bracktes

**How I solved it**: by read the code again to detecte error and ensure to solve it

**Testing approach**: by using run and show the error

**Time spent**:hour and hlaf

---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem? 
- What incorrect behavior could occur? 

**Your Answer**:

the resource ContextSwitchCount is affected ,the concurrent access make big problem because if we let multiple thread to access concurrently without manage them or without coordinate betweenn them that will give/make wrong data because of thre race condition .
this text is from my code after puting locks ,but to be clear that crirical section is always will effected if we do not manage the access


 public static void incrementContextSwitch() {
        contextSwitchLock.lock();// entry section
        try{
        contextSwitchCount++;//critical section
        }finally{
            contextSwitchLock.unlock();//exit section
        }
    }

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

ReentrantLock use boolean datatype while Semaphre use integer.
*Semaphre 
-A theard calls acquire()-> permit count decreases.
-A thread calls release()-> → permit count increases
I use it with run() method 

*ReentrantLock 
 -thread calls unlock()
 -The same thread can lock it multiple times without blocking itself.
 I use it with incrementCompletedProcess() method

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

is situation where two or more threads are permanently blocked because each one is waiting for a resource held by another. 
here in my code: SharedResources.cpuSemaphore.acquire();
-The deadlock is caused by acquire() without a guaranteed release(), especially if an exception happens before releasing.


---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

For Task 1, I used fine-grained locking with a separate lock for each counter.I  made this choice because the counters are independent, so locking them individually prevents unnecessary delays. The main trade-off is that fine-grained locking is more complex to code but significantly improves performance. This approach provides better concurrency because it allows multiple threads to update different counters at the same time. Therefore, it is the most efficient design for handling independent shared resources

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: contextSwitchCount ,completedProcessCount and totalWaitingTime

**Why they need protection**: Because they are shared resources accessed by multiple threads.Without protection, a Race Condition could occur, leading to incorrect calculations and data inconsistency

**Synchronization mechanism used**: Fine-grained locking using ReentrantLock for each variable

**Code snippet**:
```java
 public static void incrementCompletedProcess() {
      completedProcessLock.lock();// entry section,the lock is for complete process
        try{
        completedProcessCount++;//critical section
        }finally{
            completedProcessLock.unlock();//exit section
        }
    }
```

**Justification**: 
Using separate locks for each counter ensures that threads can update different variables simultaneously. This maximizes concurrency and prevents performance bottlenecks that would occur if a single global lock were used

---

### Critical Section #2: Execution Log

**What resource**: The executionLog which is an ArrayList<String>

**Why it needs protection**: The ArrayList class in Java is not thread-safe. If multiple threads try to add messages to the log simultaneously, it can lead to ConcurrentModificationException, data loss, or corrupted log entries

**Synchronization mechanism used**: Explicit locking using a ReentrantLock (specifically logLock)

**Code snippet**:
```java
public static void logExecution(String message) {
      logLock.lock();//add entry section, the lock is for log execution
      try{
        executionLog.add(message);//critical section
      }finally{
        logLock.unlock();//add exit section
      }
}
```

**Justification**: 
using a dedicated lock for the log ensures that only one thread can modify the list at any given time. This guarantees thread safety and preserves the correct chronological order of the execution messages without interfering with other independent counters.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: To control and limit the number of threads that can access the CPU resources at the same time, ensuring synchronized task execution

**Number of permits and why**: 1 permit. It acts as a Binary Semaphore to ensure that only one process can execute its critical section at a time, simulating a single-core CPU environment and preventing execution overlap

**Where implemented**: Defined in the SharedResources class and implemented within the runToCompletion() method of the Process class

**Code snippet**:
```java
public void run() {
       try{
           SharedResources.cpuSemaphore.acquire();
       
        try {
            if (startTime == -1) {
                startTime = System.currentTimeMillis();
            }
            
            // Increment context switch counter
            SharedResources.incrementContextSwitch();
            
            int runTime = Math.min(timeQuantum, remainingTime);
            
            String quantumBar = createProgressBar(0, 15);
            String message = "  ▶ " + name + " (Priority: " + priority + ") executing quantum [" + runTime + "ms]";
            System.out.println(Colors.BRIGHT_GREEN + message + Colors.RESET);
            
            // Log execution
            SharedResources.logExecution(name + " started quantum execution");
            
            try {
                int steps = 5;
                int stepTime = runTime / steps;
                
                for (int i = 1; i <= steps; i++) {
                    Thread.sleep(stepTime);
                    int quantumProgress = (i * 100) / steps;
                    quantumBar = createProgressBar(quantumProgress, 15);
                    System.out.print("\r  " + Colors.YELLOW + "⚡" + Colors.RESET + 
                                    " Quantum progress: " + quantumBar);
                }
                System.out.println();
                
            } catch (InterruptedException e) {
                System.out.println(Colors.RED + "\n  ✗ " + name + " was interrupted." + Colors.RESET);
            }
            
            remainingTime -= runTime;
            int overallProgress = (int) (((double)(burstTime - remainingTime) / burstTime) * 100);
            String overallProgressBar = createProgressBar(overallProgress, 20);
            
            System.out.println(Colors.YELLOW + "  ⏸ " + Colors.CYAN + name + Colors.RESET + 
                              " completed quantum " + Colors.BRIGHT_YELLOW + runTime + "ms" + Colors.RESET + 
                              " │ Overall progress: " + overallProgressBar);
            System.out.println(Colors.MAGENTA + "     Remaining time: " + remainingTime + "ms" + Colors.RESET);
            
            if (remainingTime > 0) {
                System.out.println(Colors.BLUE + "  ↻ " + Colors.CYAN + name + Colors.RESET + 
                                  " yields CPU for context switch" + Colors.RESET);
                SharedResources.logExecution(name + " yielded CPU");
            } else {
                completionTime = System.currentTimeMillis();
                long waitingTime = (completionTime - creationTime) - burstTime;
                SharedResources.addWaitingTime(waitingTime);
                SharedResources.incrementCompletedProcess();
                SharedResources.logExecution(name + " completed execution");
                System.out.println(Colors.BRIGHT_GREEN + "  ✓ " + Colors.BOLD + Colors.CYAN + name + 
                                  Colors.RESET + Colors.BRIGHT_GREEN + " finished execution!" + 
                                  Colors.RESET);
            }
            System.out.println();
            
        
        
        }finally {// release the CPU semaphore in the finally block
            SharedResources.cpuSemaphore.release();

```

**Effect on program behavior**: 
it ensures mutual exclusion during process execution, which maintains orderly console output and guarantees that timing calculations (like completion time) remain accurate.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
the output values for all counters remained identical across all five runs. There were no missing log entries, and the total execution time was calculated correctly every time.
**Why synchronization is necessary**: 
Synchronization is required to prevent Race Conditions. Without locks, multiple threads might update 'completedProcessCount' at the same exact time, causing some updates to be lost. Also, since 'ArrayList' is not thread-safe, protection is needed to avoid program crashes during logging

**Conclusion**: 
my implementation successfully ensures data consistency and thread safety using fine-grained locks
---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: I ran the simulation with a high number of processes to force multiple threads to write to the log at the same time. I then checked the console for any runtime errors or crashes.

**Results**: The program finished without any issues. No ConcurrentModificationException occurred

**What this proves**: It proves my logLock is doing its job. It keeps the ArrayList safe by making sure only one thread can add a message at a time, preventing the crashes that usually happen when threads collide.

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: The total number of completed processes and context switches that I calculated manually based on the input data.

**Actual values**: The final summary displayed in the terminal at the end of the simulation run.

**Analysis**: The actual results were exactly what I expected. This confirms that my locks didn't just prevent crashes, but also ensured that every single increment and calculation was recorded accurately without any data loss.

---

### Test 4: Different Scenarios
**Scenario tested**: Increasing the workload by running the simulation with a much larger number of processes (e.g., 100 processes).

**Purpose**: To see how the program handles high thread contention and to make sure that having many threads doesn't lead to performance lag or "deadlocks."

**Results**: The simulation handled the 100 processes perfectly. All counters were accurate at the end, and the program didn't slow down or freeze.

**What I learned**: I learned that fine-grained locking is very scalable. Because I used separate locks, the threads didn't spend much time waiting for each other, which allowed the system to stay fast even with a heavy workload.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

Synchronization is essential for preventing data corruption and race conditions in multi-threaded apps. I learned that choosing between fine-grained and coarse-grained locks is a balance between speed and simplicity. Using ReentrantLock and Semaphore ensures that threads cooperate without crashing the system. It taught me how to manage shared resources safely while maintaining high performance.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking Apps: To prevent double-spending from the same account.

**Example 2**: Booking Sites: To ensure a single seat isn't sold to two people.

---

### How I would explain synchronization to others:

it's like a Ride-Hailing App (like Uber). You have thousands of passengers (Threads) and limited cars (Resources). Synchronization is the 'system' that ensures each car takes only one passenger at a time. Without this system, two people might try to jump into the same car at once, causing chaos. The 'Lock' is the booking confirmation that prevents anyone else from using that resource until the trip is over.
---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Raghad-abdullah-444/OS-Assignment3-Raghad-Abdullah/edit/main/ASSIGNMENT_DOCUMENTATION.md

**Number of commits**: 43

**Commit messages**: 
1. add changes
2. ad static
3. delete
4. save changes

---

## Summary

**Total time spent on assignment**: 4 days 

**Key takeaways**: 
1. add the laibraries
2. add locks
3.run code multiple time and check 

**Most challenging aspect**: 
detect where locks acn work after run
**What I'm most proud of**: 
I  finally learad how I can manage the thread/process 
---

**End of Documentation**
