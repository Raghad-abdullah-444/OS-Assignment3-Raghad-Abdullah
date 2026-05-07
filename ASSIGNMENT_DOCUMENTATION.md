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

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java
// Paste your implementation here
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: 
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)

**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 

---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 

**Results**: 

**What I learned**: 

---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 

**Example 2**: 

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
