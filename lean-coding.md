
# Lean Coding: Strategies for Slashing Compute Costs Through Optimization

---

## Table of Contents

1. [Introduction: What is Lean Coding?](#1-introduction-what-is-lean-coding)
2. [Optimize Algorithmic Complexity](#2-optimize-algorithmic-complexity)
   - [2.1. Optimizing Loops](#21-optimizing-loops)
   - [2.2. Choosing the Right Data Structures](#22-choosing-the-right-data-structures)
   - [2.3. Avoiding Excessive Iterations](#23-avoiding-excessive-iterations)
   - [2.4. Divide and Conquer Strategies](#24-divide-and-conquer-strategies)
   - [2.5. Memoization](#25-memoization)
3. [Reduce Redundant Computations](#3-reduce-redundant-computations)
   - [3.1. Caching Intermediate Results](#31-caching-intermediate-results)
   - [3.2. Database Caching](#32-database-caching)
   - [3.3. Pre-Calculate Frequently Used Data](#33-pre-calculate-frequently-used-data)
4. [Use Asynchronous Processing](#4-use-asynchronous-processing)
   - [4.1. Async/Await in I/O Operations](#41-asyncawait-in-io-operations)
   - [4.2. Non-blocking Code](#42-non-blocking-code)
   - [4.3. Queue Background Tasks](#43-queue-background-tasks)
5. [Optimize Resource-Intensive Tasks](#5-optimize-resource-intensive-tasks)
   - [5.1. Batch Jobs and Scheduling](#51-batch-jobs-and-scheduling)
   - [5.2. Queue Long-Running Processes](#52-queue-long-running-processes)
6. [Application Profiling and Code Instrumentation](#6-application-profiling-and-code-instrumentation)
   - [6.1. Profiling Tools](#61-profiling-tools)
   - [6.2. Remove Dead Code](#62-remove-dead-code)
7. [Concurrency and Parallelism](#7-concurrency-and-parallelism)
   - [7.1. Leverage Multi-Threading](#71-leverage-multi-threading)
   - [7.2. Parallelize Independent Workloads](#72-parallelize-independent-workloads)
8. [Utilize Efficient Programming Techniques](#8-utilize-efficient-programming-techniques)
   - [8.1. Lazy Loading](#81-lazy-loading)
   - [8.2. Pooling](#82-pooling)
9. [Optimize Database Interactions](#9-optimize-database-interactions)
   - [9.1. Efficient Query Design](#91-efficient-query-design)
   - [9.2. Use of Indexes](#92-use-of-indexes)
   - [9.3. Avoiding the N+1 Query Problem](#93-avoiding-the-n1-query-problem)
10. [Efficient Memory Management](#10-efficient-memory-management)
    - [10.1. Avoiding Memory Leaks](#101-avoiding-memory-leaks)
    - [10.2. Use of Primitive Types](#102-use-of-primitive-types)
11. [Efficient Exception Handling](#11-efficient-exception-handling)
    - [11.1. Avoid Overusing Exceptions](#111-avoid-overusing-exceptions)
    - [11.2. Minimize Exception Costs](#112-minimize-exception-costs)
12. [Optimize Network Usage](#12-optimize-network-usage)
    - [12.1. Batching Network Requests](#121-batching-network-requests)
    - [12.2. Use of Compression](#122-use-of-compression)
13. [Efficient Logging Practices](#13-efficient-logging-practices)
    - [13.1. Avoid Excessive Logging](#131-avoid-excessive-logging)
    - [13.2. Use Appropriate Log Levels](#132-use-appropriate-log-levels)
14. [Resource Management](#14-resource-management)
    - [14.1. Proper Resource De-allocation](#141-proper-resource-de-allocation)
    - [14.2. Use of Auto-Closable Resources](#142-use-of-auto-closable-resources)

---

## 1. Introduction: What is Lean Coding?

**Lean Coding** is a development philosophy focused on creating efficient, high-performance code that minimizes resource consumption while maximizing productivity and maintainability. Inspired by lean manufacturing, lean coding emphasizes eliminating waste, including unnecessary computations, redundant code, and inefficient algorithms.

By adopting lean coding practices, developers can write code that performs better, scales efficiently, and reduces operational costs associated with compute resources. This approach not only improves application performance but also contributes to sustainability by lowering energy consumption.

This guide explores various strategies and techniques for implementing lean coding principles in your applications, with examples in TypeScript (TS) and Spring Java.

> **Learn More**
>
> - [Lean Software Development](https://en.wikipedia.org/wiki/Lean_software_development)

---

## 2. Optimize Algorithmic Complexity

Reducing algorithmic complexity is crucial for improving code efficiency and lowering compute costs.

### 2.1. Optimizing Loops

Minimize the number of nested loops to reduce time complexity.

#### Java Example

    public class AlgorithmOptimization {
        public Map<String, Integer> inefficientLoop(List<String> data) {
            Map<String, Integer> wordCount = new HashMap<>();
            for (String word : data) {
                int count = 0;
                for (String innerWord : data) { // Inefficient nested loop
                    if (innerWord.equals(word)) {
                        count++;
                    }
                }
                wordCount.put(word, count);
            }
            return wordCount;
        }
    }

**Why It’s Bad**: This approach re-counts each word multiple times, leading to excessive computations and slow performance.

**Strategies to Fix**

1. **Identify Redundancy**: Notice that the inner loop redundantly counts occurrences for each word.
2. **Use a Single Pass**: Iterate through the list once to count word frequencies.
3. **Utilize Efficient Data Structures**: Leverage a `HashMap` to store counts efficiently.

**Good Example** – Optimized loop with single pass:

    public class AlgorithmOptimization {
        public Map<String, Integer> optimizeLoop(List<String> data) {
            Map<String, Integer> wordCount = new HashMap<>();
            for (String word : data) {
                wordCount.put(word, wordCount.getOrDefault(word, 0) + 1);
            }
            return wordCount;
        }
    }

**How It Fixes**: This version iterates over the list only once and uses a `HashMap` to track frequencies, eliminating the nested loop and significantly reducing computational overhead.

> **Learn More**
>
> - [Big O Notation](https://en.wikipedia.org/wiki/Big_O_notation)
> - [Loop Optimization Techniques](https://www.geeksforgeeks.org/loop-optimization-in-compiler-design/)

---

### 2.2. Choosing the Right Data Structures

Selecting appropriate data structures can significantly improve performance.

#### Java Example

**Bad Example** – Using a `List` for frequent lookups (O(n) time):

    public class DataStructureExample {
        public boolean containsItem(List<String> items, String itemToFind) {
            return items.contains(itemToFind); // O(n) time complexity
        }
    }

**Why It’s Bad**: Here, we repeatedly scan the entire list for each lookup, which is O(n) time and inefficient for larger datasets.

**Strategies to Fix**

1. **Use a `HashSet`**: Provides O(1) time complexity for lookups.
2. **Convert List to Set**: If starting with a list, convert it to a set.
3. **Update Code Logic**: Adjust methods to work with sets.

**Good Example** – Using a `HashSet` for efficient lookups:

    public class DataStructureExample {
        public boolean containsItem(Set<String> items, String itemToFind) {
            return items.contains(itemToFind); // O(1) time complexity
        }
    }

**How It Fixes**: The `HashSet` approach allows near-constant-time lookups, removing the need to scan through every element in the list.

> **Learn More**
>
> - [Java Collections Framework](https://docs.oracle.com/javase/8/docs/technotes/guides/collections/overview.html)
> - [HashSet Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)

---

### 2.3. Avoiding Excessive Iterations

Employ algorithms with better time complexity to reduce iterations.

#### TypeScript Example

**Bad Example** – Linear search in an unsorted array (O(n) time):

    function findElement(arr: number[], target: number): boolean {
      return arr.includes(target); // O(n) time complexity
    }

**Why It’s Bad**: Performing a linear search for each query can become a bottleneck when the array is large.

**Strategies to Fix**

1. **Sort the Array**: Allows use of binary search.
2. **Implement Binary Search**: Reduces time complexity to O(log n).
3. **Keep the Array Sorted**: For future operations.

**Good Example** – Using binary search after sorting:

    function findElement(arr: number[], target: number): boolean {
      arr.sort((a, b) => a - b);
      let left = 0,
          right = arr.length - 1;
      while (left <= right) {
        const mid = Math.floor((left + right) / 2);
        if (arr[mid] === target) return true;
        if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
      }
      return false;
    }

**How It Fixes**: By first sorting the array and then applying binary search, this approach drastically reduces the number of comparisons needed, improving performance from O(n) to O(log n).

> **Learn More**
>
> - [Binary Search Algorithm](https://en.wikipedia.org/wiki/Binary_search_algorithm)
> - [Array.prototype.sort() MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

---

### 2.4. Divide and Conquer Strategies

Break down complex problems into simpler sub-problems.

**Bad Example** – Processing a large dataset in one go:

    public class LargeDatasetProcessor {
        public void processDataset(int[] dataset) {
            for (int num : dataset) {
                // Simulate heavy computation
                System.out.println("Processing element: " + num);
            }
        }
    }

**Why It’s Bad**: Processing a large dataset all at once can be slow, consume excessive memory, and offer no clear path for parallelism.

**Strategies to Fix**

1. **Split the Work**: Divide the dataset.
2. **Use Recursive or Parallel Algorithms**: E.g., merge sort.
3. **Combine Results**: Integrate sub-results for the final output.

**Good Example** – Using a divide-and-conquer approach (Merge Sort):

    public class MergeSort {
        public static void mergeSort(int[] array, int left, int right) {
            if (left < right) {
                int middle = (left + right) / 2;
                mergeSort(array, left, middle);
                mergeSort(array, middle + 1, right);
                merge(array, left, middle, right);
            }
        }
        // ...
    }

**How It Fixes**: Divide and conquer splits the dataset into smaller parts, processing them independently and then merging results, leading to more efficient use of time and space.

> **Learn More**
>
> - [Divide and Conquer](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm)
> - [Merge Sort](https://en.wikipedia.org/wiki/Merge_sort)

---

### 2.5. Memoization

Memoization stores the results of expensive function calls and reuses them when the same inputs occur again.

**Bad Example** – Recursive calculation without memoization:

    public class FibonacciCalculator {
        public int fibonacci(int n) {
            if (n <= 1) return n;
            return fibonacci(n - 1) + fibonacci(n - 2); // Redundant calculations
        }
    }

**Why It’s Bad**: Each call re-computes values for overlapping sub-problems, causing repeated work and high CPU usage.

**Strategies to Fix**

1. **Implement Memoization**: Use a cache.
2. **Check Cache Before Recursion**.
3. **Use the Cache**: Incorporate logic into the function.

**Good Example** – Recursive calculation with memoization:

    public class FibonacciCalculator {
        private Map<Integer, Integer> memo = new HashMap<>();

        public int fibonacci(int n) {
            if (n <= 1) return n;
            if (memo.containsKey(n)) return memo.get(n);
            int result = fibonacci(n - 1) + fibonacci(n - 2);
            memo.put(n, result);
            return result;
        }
    }

**How It Fixes**: By storing already calculated values in a map, the function avoids redundant re-computation and significantly improves performance.

> **Learn More**
>
> - [Memoization](https://en.wikipedia.org/wiki/Memoization)
> - [Dynamic Programming](https://en.wikipedia.org/wiki/Dynamic_programming)

---

## 3. Reduce Redundant Computations

### 3.1. Caching Intermediate Results

Store results of expensive computations for reuse.

**Bad Example** – Performing the same heavy computation multiple times:

    function expensiveComputation(x: number): number {
      let result = 0;
      for (let i = 0; i < 1e7; i++) {
        result += Math.sqrt(x + i);
      }
      return result;
    }

    function compute() {
      const a = expensiveComputation(5);
      const b = expensiveComputation(5); // Redundant
      console.log(a + b);
    }

**Why It’s Bad**: The function is called twice with the same input, needlessly using CPU resources to repeat the same calculations.

**Strategies to Fix**

1. **Implement a Cache**.
2. **Check Cache Before Computing**.
3. **Update Function Logic**: Incorporate caching inside the function.

**Good Example** – Using caching:

    const computationCache: { [key: number]: number } = {};

    function expensiveComputation(x: number): number {
      if (computationCache.hasOwnProperty(x)) return computationCache[x];
      let result = 0;
      for (let i = 0; i < 1e7; i++) {
        result += Math.sqrt(x + i);
      }
      computationCache[x] = result;
      return result;
    }

    function compute() {
      const a = expensiveComputation(5);
      const b = expensiveComputation(5);
      console.log(a + b);
    }

**How It Fixes**: The function now stores computed results in a cache and quickly retrieves them for identical inputs, saving significant computation time on repeat calls.

> **Learn More**
>
> - [Caching Strategies](https://en.wikipedia.org/wiki/Cache_replacement_policies)
> - [Memoization in JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/Memoization)

---

### 3.2. Database Caching

Cache frequent database query results to reduce load.

**Bad Example** – Always querying the DB:

    public class UserService {
        public User getUserById(String userId) {
            // Hits DB every time
            return database.findUserById(userId);
        }
    }

**Why It’s Bad**: Continuously querying the database for the same data creates unnecessary load, slows down the system, and can increase costs.

**Strategies to Fix**

1. **Implement a Caching Layer** (e.g., Redis, Memcached).
2. **Cache Query Results** with an expiration.
3. **Check the Cache First**.

**Good Example** – Using a local map as a simple cache:

    public class UserService {
        private Map<String, User> cache = new HashMap<>();

        public User getUserById(String userId) {
            if (cache.containsKey(userId)) {
                return cache.get(userId);
            }
            User user = database.findUserById(userId);
            cache.put(userId, user);
            return user;
        }
    }

**How It Fixes**: By storing frequently accessed data in memory, subsequent lookups avoid hitting the database and substantially improve response times.

> **Learn More**
>
> - [Database Caching Strategies](https://en.wikipedia.org/wiki/Cache_replacement_policies)
> - [Redis Overview](https://redis.io/)

---

### 3.3. Pre-Calculate Frequently Used Data

Compute and store data that is frequently needed and doesn't change often.

**Bad Example** – Calculating the same rate on every request:

    public class TaxService {
        public double getTaxRate() {
            // Heavy calculation each time
            return fetchTaxRateFromDatabase();
        }
    }

**Why It’s Bad**: Re-computing a constant or rarely changing value results in wasted CPU cycles on each request.

**Strategies to Fix**

1. **Precompute** during startup.
2. **Store in Memory**.
3. **Use Precomputed Values** directly.

**Good Example** – Pre-calculating and storing tax rate:

    public class TaxService {
        private static final double TAX_RATE = loadTaxRate();

        public double getTaxRate() {
            return TAX_RATE;
        }

        private static double loadTaxRate() {
            // Calculate once
            return 0.07;
        }
    }

**How It Fixes**: The tax rate is calculated only once at startup; subsequent requests simply retrieve the precomputed constant, eliminating repeated heavy calculations.

> **Learn More**
>
> - [Precomputation Techniques](https://en.wikipedia.org/wiki/Precomputation)
> - [Caching vs. Precomputation](https://www.geeksforgeeks.org/difference-between-caching-and-precomputation/)

---

## 4. Use Asynchronous Processing

### 4.1. Async/Await in I/O Operations

Prevent CPU idling during I/O by using async.

#### TypeScript Example

**Bad Example** – Synchronous file reading:

    const fs = require('fs');

    function readFileSync(path: string) {
      const data = fs.readFileSync(path, 'utf-8'); // Blocking
      console.log(data);
    }

**Why It’s Bad**: Synchronous operations block the event loop, preventing other tasks from running while file reading completes.

**Strategies to Fix**

1. **Use Asynchronous Methods**.
2. **Implement Async/Await**.
3. **Handle Errors** with try/catch.

**Good Example** – Non-blocking I/O:

    const fs = require('fs').promises;

    async function readFileAsync(path: string) {
      try {
        const data = await fs.readFile(path, 'utf-8');
        console.log(data);
      } catch (error) {
        console.error('Error reading file:', error);
      }
    }

**How It Fixes**: Using async/await frees up the main thread to handle other operations, avoiding bottlenecks from synchronous I/O.

> **Learn More**
>
> - [Async/Await MDN](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Async_await)
> - [Non-blocking I/O](https://en.wikipedia.org/wiki/Non-blocking_I/O)

---

### 4.2. Non-blocking Code

Ensure CPU resources are not blocked by heavy operations.

#### JavaScript Example

**Bad Example** – Main thread blocked by computation:

    function heavyComputation() {
      let sum = 0;
      for (let i = 0; i < 1e9; i++) {
        sum += i;
      }
      return sum;
    }

    app.get('/compute', (req, res) => {
      const result = heavyComputation(); // Blocks event loop
      res.send(\`Result: \${result}\`);
    });

**Why It’s Bad**: This computation monopolizes the event loop, causing the application to be unresponsive to other requests.

**Strategies to Fix**

1. **Offload Computation** (worker threads).
2. **Keep Event Loop Free**.
3. **Use Async Patterns**.

**Good Example** – Worker threads:

    const { Worker } = require('worker_threads');

    app.get('/compute', (req, res) => {
      const worker = new Worker('./heavyComputation.js');
      worker.on('message', (result) => res.send(\`Result: \${result}\`));
      worker.on('error', () => res.status(500).send('Error in computation'));
    });

**How It Fixes**: By moving heavy calculations to a separate thread, the main thread remains responsive, allowing the server to handle additional incoming requests.

> **Learn More**
>
> - [Node.js Worker Threads](https://nodejs.org/api/worker_threads.html)
> - [Event Loop Explained](https://nodejs.dev/learn/the-nodejs-event-loop)

---

### 4.3. Queue Background Tasks

Offload non-critical tasks to background.

#### JavaScript Example

**Bad Example** – Synchronous email sending:

    app.post('/signup', (req, res) => {
      sendWelcomeEmail(req.body.email); // Blocks request
      res.send('User created');
    });

**Why It’s Bad**: Processing emails directly in the request path delays responses and impacts user experience.

**Strategies to Fix**

1. **Use a Message Queue** (RabbitMQ, AWS SQS).
2. **Process in Background**.
3. **Immediate Response** to users.

**Good Example** – Enqueuing tasks:

    const queue = require('some-queue-lib');

    app.post('/signup', (req, res) => {
      queue.sendMessage({ type: 'WELCOME_EMAIL', to: req.body.email });
      res.send('User created');
    });

    queue.on('message', (msg) => {
      if (msg.type === 'WELCOME_EMAIL') {
        sendWelcomeEmail(msg.to);
      }
    });

**How It Fixes**: Queuing the email task returns control to the user quickly while the actual email send occurs asynchronously in the background.

> **Learn More**
>
> - [Message Queues Explained](https://en.wikipedia.org/wiki/Message_queue)
> - [RabbitMQ Overview](https://www.rabbitmq.com/)

---

## 5. Optimize Resource-Intensive Tasks

### 5.1. Batch Jobs and Scheduling

Run heavy tasks during off-peak hours.

#### Java Example

**Bad Example** – Manual runs during peak usage:

    public class ManualBatchService {
        public void runBatchJobManually() {
            // Might run at busy times
            System.out.println("Running batch job manually.");
        }
    }

**Why It’s Bad**: Triggering resource-intensive jobs whenever convenient can coincide with peak loads, straining the system.

**Strategies to Fix**

1. **Automate Scheduling** (Quartz, Spring @Scheduled).
2. **Off-Peak Execution**.
3. **Eliminate Manual Steps**.

**Good Example** – Scheduled batch job in Spring:

    @Service
    public class ScheduledBatchService {
        @Scheduled(cron = "0 0 2 * * ?")
        public void runBatchJob() {
            System.out.println("Running batch job at 2 AM.");
        }
    }

**How It Fixes**: Automation ensures heavy tasks run during low-traffic periods, minimizing their impact on overall performance.

> **Learn More**
>
> - [Batch Processing](https://en.wikipedia.org/wiki/Batch_processing)
> - [Spring Scheduling](https://docs.spring.io/spring-framework/docs/current/reference/html/integration.html#scheduling)

---

### 5.2. Queue Long-Running Processes

Break down and queue large tasks.

#### Java Example

**Bad Example** – Processing large file in one shot:

    public class FileProcessingService {
        public void processLargeFile(String filePath) {
            // Entire file at once
            System.out.println("Processing file: " + filePath);
        }
    }

**Why It’s Bad**: Handling massive files in a single pass can cause significant blocking and degrade system responsiveness.

**Strategies to Fix**

1. **Split Task into Chunks**.
2. **Use Background Threads**.
3. **Monitor Progress**.

**Good Example** – Chunk-based asynchronous processing:

    public class FileProcessingService {
        private ExecutorService executor = Executors.newFixedThreadPool(5);

        public void processLargeFile(String filePath) {
            List<String> chunks = splitFileIntoChunks(filePath);
            for (String chunk : chunks) {
                executor.submit(() -> processChunk(chunk));
            }
        }
        // ...
    }

**How It Fixes**: By processing file chunks in parallel, the load is spread out over multiple threads, improving throughput and avoiding a single massive operation.

> **Learn More**
>
> - [Task Queuing](https://en.wikipedia.org/wiki/Job_scheduler)
> - [ExecutorService Tutorial](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html)

---

## 6. Application Profiling and Code Instrumentation

### 6.1. Profiling Tools

Use profiling tools to monitor resource consumption.

#### Java Example

**Bad Example** – Guess-based optimization:

    public class GuessOptimizer {
        public void optimize() {
            // No actual profiling, just guesses
            System.out.println("Optimizing...");
        }
    }

**Why It’s Bad**: Without real measurements, you risk spending time optimizing areas that may not be bottlenecks.

**Strategies to Fix**

1. **Use Profilers** (Java Flight Recorder, VisualVM).
2. **Identify Bottlenecks** with real data.
3. **Optimize Based on Profiling**.

**Good Example** – Profiling approach:

    public class ProfilingExample {
        public void runWithProfiling() {
            // Start profiler
            // Perform heavy operations
            // Analyze results
        }
    }

**How It Fixes**: Profiling pinpoints actual performance hotspots, so developers focus on real issues rather than guesswork.

> **Learn More**
>
> - [Java VisualVM](https://visualvm.github.io/)
> - [Profiling Techniques](https://en.wikipedia.org/wiki/Program_instrumentation)

---

### 6.2. Remove Dead Code

Eliminate unused code to streamline the codebase.

#### Java Example

**Bad Example** – Keeping unused methods:

    public class DeadCodeExample {
        public void unusedMethod() {
            System.out.println("I am never called!");
        }
    }

**Why It’s Bad**: Unused methods add clutter, complicate maintenance, and can confuse future developers.

**Strategies to Fix**

1. **Identify Unused Methods** (IDE, static analysis).
2. **Remove or Archive** them.
3. **Refactor** if partial usage is needed.

**Good Example** – Cleaned codebase:

    public class CleanerCodeExample {
        // All methods in use, no dead code
    }

**How It Fixes**: Removing dead code leads to a cleaner, more efficient codebase and reduces the potential for bugs.

> **Learn More**
>
> - [Static Code Analysis](https://en.wikipedia.org/wiki/Static_program_analysis)
> - [Code Refactoring](https://en.wikipedia.org/wiki/Code_refactoring)

---

## 7. Concurrency and Parallelism

### 7.1. Leverage Multi-Threading

Run tasks concurrently to improve efficiency.

#### Java Example

**Bad Example** – Single-threaded processing:

    public class SingleThreadProcessor {
        public void processTasks(List<Runnable> tasks) {
            for (Runnable task : tasks) {
                task.run(); // Same thread
            }
        }
    }

**Why It’s Bad**: All tasks compete for the same thread, causing long wait times and underutilized CPU cores.

**Strategies to Fix**

1. **Create a Thread Pool**.
2. **Submit Tasks Concurrently**.
3. **Manage Thread Safety** carefully.

**Good Example** – ExecutorService:

    public class MultiThreadProcessor {
        private ExecutorService executor = Executors.newFixedThreadPool(4);

        public void processTasks(List<Runnable> tasks) {
            for (Runnable task : tasks) {
                executor.submit(task);
            }
        }
    }

**How It Fixes**: Multiple threads can handle different tasks simultaneously, improving throughput and leveraging multicore architectures.

> **Learn More**
>
> - [Java Concurrency](https://docs.oracle.com/javase/tutorial/essential/concurrency/)
> - [ExecutorService Tutorial](https://www.baeldung.com/java-executor-service)

---

### 7.2. Parallelize Independent Workloads

Process independent tasks in parallel to save time.

#### Java Example

**Bad Example** – Sequential processing:

    public class ImageProcessor {
        public void processAll(List<String> images) {
            for (String img : images) {
                process(img); // Sequential
            }
        }
    }

**Why It’s Bad**: Independent workloads are forced to run one after another, wasting potential parallel CPU resources.

**Strategies to Fix**

1. **Identify Independent Tasks**.
2. **Use Parallel Streams** or concurrency APIs.
3. **Avoid Over-parallelizing**.

**Good Example** – Parallel streams:

    public class ImageProcessor {
        public void processAll(List<String> images) {
            images.parallelStream().forEach(this::process);
        }
        // ...
    }

**How It Fixes**: Parallel streams split the workload across threads automatically, speeding up the overall processing for large datasets.

> **Learn More**
>
> - [Parallel Streams in Java](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html#parallel--)
> - [Java Concurrency Best Practices](https://www.baeldung.com/java-concurrency)

---

## 8. Utilize Efficient Programming Techniques

### 8.1. Lazy Loading

Load resources only when needed.

#### Java Example

**Bad Example** – Eagerly loading data:

    public class EagerLoader {
        private List<String> data = loadBigData();
        // ...
    }

**Why It’s Bad**: This approach loads potentially large data upfront, consuming memory and time even if the data is never used immediately.

**Strategies to Fix**

1. **Defer Loading** until necessary.
2. **Cache After First Load**.
3. **Improve Startup Performance** by delaying heavy ops.

**Good Example** – Lazy initialization:

    public class LazyLoader {
        private List<String> data = null;

        public List<String> getData() {
            if (data == null) {
                data = loadBigData();
            }
            return data;
        }
    }

**How It Fixes**: The data is only loaded when first requested, reducing unnecessary overhead at application startup.

> **Learn More**
>
> - [Lazy Loading](https://en.wikipedia.org/wiki/Lazy_loading)
> - [Lazy Initialization Pattern](https://refactoring.guru/design-patterns/lazy-initialization)

---

### 8.2. Pooling

Reuse resources to minimize overhead.

#### Java Example

**Bad Example** – Creating new DB connection per request:

    public class ConnectionManager {
        public Connection getConnection() throws SQLException {
            return DriverManager.getConnection("jdbc:mysql://localhost:3306/db", "user", "pass");
        }
    }

**Why It’s Bad**: Constantly opening new connections is expensive and can overload the database with connection overhead.

**Strategies to Fix**

1. **Use a Connection Pool** (HikariCP, etc.).
2. **Reuse Connections**.
3. **Close or Return** connections properly.

**Good Example** – Connection pooling with HikariCP:

    public class PooledConnectionManager {
        private HikariDataSource dataSource;

        public PooledConnectionManager() {
            HikariConfig config = new HikariConfig();
            config.setJdbcUrl("jdbc:mysql://localhost:3306/db");
            config.setUsername("user");
            config.setPassword("pass");
            dataSource = new HikariDataSource(config);
        }

        public Connection getConnection() throws SQLException {
            return dataSource.getConnection();
        }
    }

**How It Fixes**: The pool maintains a ready set of connections, so requests can borrow and return them efficiently without repeatedly establishing new connections.

> **Learn More**
>
> - [Object Pool Pattern](https://refactoring.guru/design-patterns/object-pool)
> - [HikariCP Documentation](https://github.com/brettwooldridge/HikariCP)

---

## 9. Optimize Database Interactions

### 9.1. Efficient Query Design

Design queries to retrieve only necessary data.

#### SQL Example

**Bad Example** – Using `SELECT *`:

    SELECT * FROM users WHERE id = 123;

**Why It’s Bad**: Pulling every column (many of which may be unneeded) increases data transfer and slows down queries.

**Strategies to Fix**

1. **Select Specific Columns**.
2. **Use WHERE to Filter**.
3. **Minimize Data Transfer**.

**Good Example** – Selecting needed columns:

    SELECT first_name, last_name FROM users WHERE id = 123;

**How It Fixes**: By fetching only the required columns, this query reduces the amount of data transferred and speeds up the operation.

> **Learn More**
>
> - [SQL Query Optimization](https://en.wikipedia.org/wiki/Query_optimization)
> - [Selective Column Retrieval](https://www.w3schools.com/sql/sql_select.asp)

---

### 9.2. Use of Indexes

Indexes can significantly improve query performance.

#### SQL Example

**Bad Example** – No index on frequently queried columns:

    CREATE TABLE users (
      id INT PRIMARY KEY,
      email VARCHAR(255),
      name VARCHAR(100)
    );

    SELECT * FROM users WHERE email = 'test@example.com'; -- Full table scan

**Why It’s Bad**: Without an index, the database must scan the entire table for each query, leading to poor performance.

**Strategies to Fix**

1. **Create Indexes** on frequently used columns.
2. **Monitor Performance**.
3. **Use Composite Indexes** if needed.

**Good Example** – Creating an index:

    CREATE INDEX idx_users_email ON users(email);

**How It Fixes**: The index allows the database to quickly locate rows matching \`email\`, avoiding a full table scan.

> **Learn More**
>
> - [Database Indexing](https://en.wikipedia.org/wiki/Database_index)
> - [Indexing Best Practices](https://www.geeksforgeeks.org/indexing-in-databases/)

---

### 9.3. Avoiding the N+1 Query Problem

Fetch related data efficiently.

#### Java Example

**Bad Example** – One query per record:

    public class OrderService {
        public List<Order> getAllOrders() {
            List<Order> orders = em.createQuery("SELECT o FROM Order o").getResultList();
            for (Order order : orders) {
                List<Item> items = em.createQuery("SELECT i FROM Item i WHERE i.orderId = :id")
                                     .setParameter("id", order.getId())
                                     .getResultList();
                order.setItems(items);
            }
            return orders;
        }
    }

**Why It’s Bad**: Executing separate queries for each record causes a surge in round trips, harming performance.

**Strategies to Fix**

1. **Fetch Joins**.
2. **Batch Fetching**.
3. **Single Query** for parent + children.

**Good Example** – Fetch join:

    public class OrderService {
        public List<Order> getAllOrders() {
            return em.createQuery("SELECT o FROM Order o JOIN FETCH o.items", Order.class)
                     .getResultList();
        }
    }

**How It Fixes**: A single query pulls both \`Order\` and associated \`Item\` data together, minimizing the number of round trips to the database.

> **Learn More**
>
> - [N+1 Query Problem](https://en.wikipedia.org/wiki/N%2B1_query_problem)
> - [Fetch Joins in JPA](https://www.baeldung.com/hibernate-fix-n-plus-1-selects)

---

## 10. Efficient Memory Management

### 10.1. Avoiding Memory Leaks

Manage memory carefully.

#### Java Example

**Bad Example** – Not closing resources:

    public class MemoryLeakExample {
        public void readLargeFile() throws IOException {
            FileInputStream fis = new FileInputStream("largefile.txt");
            // No close => possible leak
        }
    }

**Why It’s Bad**: Failing to close file streams or other resources can lead to memory leaks and degrade performance over time.

**Strategies to Fix**

1. **Close in Finally** or \`try-with-resources\`.
2. **Remove Unused References**.
3. **Use Tools** (profilers) to detect leaks.

**Good Example** – \`try-with-resources\`:

    public class ProperResourceHandling {
        public void readLargeFile() throws IOException {
            try (FileInputStream fis = new FileInputStream("largefile.txt")) {
                // Automatically closed
            }
        }
    }

**How It Fixes**: The \`try-with-resources\` block ensures that the file stream is closed automatically, preventing resource leakage.

> **Learn More**
>
> - [Memory Leaks in Java](https://en.wikipedia.org/wiki/Memory_leak)
> - [Try-with-Resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

---

### 10.2. Use of Primitive Types

Use primitives instead of wrappers to reduce memory usage.

#### Java Example

**Bad Example** – Overuse of wrapper classes:

    public class WrapperUsage {
        public Integer calculateSum(Integer a, Integer b) {
            return a + b;
        }
    }

**Why It’s Bad**: Wrapper classes add object overhead and can lead to autoboxing, which is less efficient than using primitives.

**Strategies to Fix**

1. **Favor Primitives** (\`int\`, \`double\`) over wrappers.
2. **Use Wrappers Only If Needed**.
3. **Beware of Autoboxing** overhead.

**Good Example** – Using primitives:

    public class PrimitiveUsage {
        public int calculateSum(int a, int b) {
            return a + b;
        }
    }

**How It Fixes**: By substituting \`Integer\` with \`int\`, this code avoids autoboxing overhead, improving both memory and execution efficiency.

> **Learn More**
>
> - [Primitives vs. Wrappers in Java](https://www.baeldung.com/java-primitives-why)
> - [Java Autoboxing](https://en.wikipedia.org/wiki/Autoboxing)

---

## 11. Efficient Exception Handling

### 11.1. Avoid Overusing Exceptions

Exceptions shouldn’t be used for normal control flow.

#### Java Example

**Bad Example** – Using exceptions for validation:

    public class OverusedExceptions {
        public int parseNumber(String input) {
            try {
                return Integer.parseInt(input);
            } catch (NumberFormatException e) {
                return -1;
            }
        }
    }

**Why It’s Bad**: Throwing exceptions for routine checks can be expensive and makes code harder to follow.

**Strategies to Fix**

1. **Validate Inputs** first.
2. **Use Exceptions for Exceptional Cases**.
3. **Return Sentinel Values** if invalid input is common.

**Good Example** – Conditional check:

    public class ProperExceptionUse {
        public int parseNumber(String input) {
            if (input.matches("\\d+")) {
                return Integer.parseInt(input);
            }
            return -1;
        }
    }

**How It Fixes**: Checking for valid input before parsing avoids unnecessary exception throwing, reducing overhead and clarifying logic.

> **Learn More**
>
> - [Exception Handling in Java](https://docs.oracle.com/javase/tutorial/essential/exceptions/)
> - [Best Practices for Exception Handling](https://www.baeldung.com/java-exceptions)

---

### 11.2. Minimize Exception Costs

Reduce performance impacts of exceptions.

#### Java Example

**Bad Example** – Throwing exceptions in a loop:

    public class LoopException {
        public void processList(List<String> list) {
            for (String item : list) {
                try {
                    if (item.isEmpty()) {
                        throw new IllegalArgumentException("Empty item");
                    }
                    // Process item
                } catch (IllegalArgumentException e) {
                    // High overhead in loop
                }
            }
        }
    }

**Why It’s Bad**: Repeatedly throwing exceptions in a tight loop can slow down your application and produce noisy logs.

**Strategies to Fix**

1. **Check Conditions** before throwing exceptions.
2. **Use Exceptions Sparingly**.
3. **Fail Fast** with upfront validation.

**Good Example** – Conditional checks:

    public class LoopCheck {
        public void processList(List<String> list) {
            for (String item : list) {
                if (!item.isEmpty()) {
                    // Process item
                } else {
                    // Handle empty case
                }
            }
        }
    }

**How It Fixes**: By validating items before operating on them, we remove the need to throw exceptions in normal circumstances, reducing overhead.

> **Learn More**
>
> - [Java Exception Handling Best Practices](https://www.baeldung.com/java-exceptions)
> - [Minimizing Exception Overhead](https://en.wikipedia.org/wiki/Exception_handling)

---

## 12. Optimize Network Usage

### 12.1. Batching Network Requests

Combine requests to save bandwidth.

#### JavaScript Example

**Bad Example** – Multiple calls in a loop:

    async function fetchData(ids) {
      for (const id of ids) {
        const response = await axios.get(`/data/${id}`);
        console.log(response.data);
      }
    }

**Why It’s Bad**: Making individual network calls for each item multiplies overhead, leading to slower responses.

**Strategies to Fix**

1. **Batch Calls** if the API supports it.
2. **Reduce Round Trips**.
3. **Use HTTP/2** if possible.

**Good Example** – Single request for multiple IDs:

    async function fetchData(ids) {
      const response = await axios.post('/data/batch', { ids });
      console.log(response.data);
    }

**How It Fixes**: Combining multiple items into one request decreases round-trip overhead and often improves overall performance.

> **Learn More**
>
> - [Batching Network Requests](https://en.wikipedia.org/wiki/Batch_processing)
> - [HTTP/2 Overview](https://http2.github.io/)

---

### 12.2. Use of Compression

Compress data to reduce payload size.

#### JavaScript Example

**Bad Example** – Sending large uncompressed responses:

    app.use(express.json()); // No compression

**Why It’s Bad**: Uncompressed responses can be large, slowing down data transfer and increasing bandwidth costs.

**Strategies to Fix**

1. **Enable Gzip or Brotli** on server.
2. **Compress Static Assets**.
3. **Use HTTP/2** for additional benefits.

**Good Example** – Enabling compression in Express:

    const compression = require('compression');
    app.use(compression());
    app.use(express.json());

**How It Fixes**: Compression reduces response sizes, speeding up data transfer and enhancing user experience.

> **Learn More**
>
> - [HTTP Compression](https://en.wikipedia.org/wiki/HTTP_compression)
> - [Express Compression Middleware](https://www.npmjs.com/package/compression)

---

## 13. Efficient Logging Practices

### 13.1. Avoid Excessive Logging

Logging too much can degrade performance.

#### Java Example

**Bad Example** – Debug logging in every iteration:

    for (int i = 0; i < 1000000; i++) {
        logger.debug("Processing index " + i);
    }

**Why It’s Bad**: Writing logs in tight loops can flood log files and slow down the application.

**Strategies to Fix**

1. **Reduce Log Volume**.
2. **Use Log Levels Appropriately**.
3. **Aggregate or Sample Logs** if needed.

**Good Example** – Controlled logging:

    for (int i = 0; i < 1000000; i++) {
        if (i % 100000 == 0) {
            logger.info("Processed " + i + " items");
        }
    }

**How It Fixes**: Logging at specific intervals rather than every iteration curbs I/O overhead and still provides sufficient visibility.

> **Learn More**
>
> - [Logging Best Practices](https://en.wikipedia.org/wiki/Logging)
> - [Centralized Log Management](https://www.loggly.com/ultimate-guide/centralized-log-management-logging-levels/)

---

### 13.2. Use Appropriate Log Levels

Ensure logs are meaningful and not overwhelming.

#### Java Example

**Bad Example** – Logging everything at `ERROR`:

    logger.error("User logged in successfully");
    logger.error("Fetched data from DB");

**Why It’s Bad**: This misuse of log levels generates noise and obscures real error conditions.

**Strategies to Fix**

1. **Map Levels Correctly** (`ERROR`, `WARN`, `INFO`, `DEBUG`).
2. **Reserve ERROR for Critical Failures**.
3. **Use INFO for Normal Ops**.

**Good Example** – Proper log levels:

    logger.info("User logged in successfully");
    logger.warn("Cache miss, falling back to DB");
    logger.error("Database connection failed", e);

**How It Fixes**: Each log event is assigned an appropriate severity, making it easier to filter and identify critical issues.

> **Learn More**
>
> - [Logging Levels](https://en.wikipedia.org/wiki/Logging)
> - [Log Level Best Practices](https://www.loggly.com/ultimate-guide/centralized-log-management-logging-levels/)

---

## 14. Resource Management

### 14.1. Proper Resource De-allocation

Clean up resources to prevent leaks.

#### Java Example

**Bad Example** – Holding onto resources indefinitely:

    public class ResourceHolder {
        private Connection conn;

        public void openConnection() throws SQLException {
            conn = DriverManager.getConnection("jdbc:mysql://localhost/db", "user", "pass");
            // No close => leak
        }
    }

**Why It’s Bad**: Failing to release resources such as DB connections can degrade performance and eventually cause the system to run out of connections.

**Strategies to Fix**

1. **Close in Finally** or \`try-with-resources\`.
2. **Ensure All Paths Close** resources.
3. **Null References** if needed.

**Good Example** – \`try-with-resources\`:

    public class ResourceHolder {
        public void openAndQuery() {
            try (Connection conn = DriverManager.getConnection("jdbc:mysql://localhost/db", "user", "pass")) {
                // DB operations
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

**How It Fixes**: Using \`try-with-resources\` ensures that the connection is automatically closed, freeing it for other operations.

> **Learn More**
>
> - [Resource Management in Java](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)
> - [Database Connection Pooling](https://en.wikipedia.org/wiki/Connection_pool)

---

### 14.2. Use of Auto-Closable Resources

Use \`AutoCloseable\` for efficient resource handling.

#### Java Example

**Bad Example** – Manual closing:

    public class ManualClosable implements Closeable {
        private FileOutputStream fos;
        public ManualClosable() throws FileNotFoundException {
            fos = new FileOutputStream("test.txt");
        }
        @Override
        public void close() throws IOException {
            fos.close();
        }
    }

**Why It’s Bad**: Manual resource management is error-prone; developers might forget to close resources or handle exceptions properly.

**Strategies to Fix**

1. **Implement \`AutoCloseable\`**.
2. **Use \`try-with-resources\`** to avoid manual closure.
3. **Handle Exceptions** gracefully.

**Good Example** – \`AutoCloseable\`:

    public class AutoClosableResource implements AutoCloseable {
        private FileOutputStream fos;
        public AutoCloseableResource() throws FileNotFoundException {
            fos = new FileOutputStream("test.txt");
        }
        @Override
        public void close() throws IOException {
            fos.close();
            System.out.println("Resource closed automatically!");
        }
    }

    // Usage:
    try (AutoClosableResource resource = new AutoClosableResource()) {
        // Perform operations
    } catch (IOException e) {
        e.printStackTrace();
    }

**How It Fixes**: By making the class implement \`AutoCloseable\`, we can leverage \`try-with-resources\` to guarantee closure, even if exceptions occur.

> **Learn More**
>
> - [AutoCloseable Interface](https://docs.oracle.com/javase/7/docs/api/java/lang/AutoCloseable.html)
> - [Effective Resource Management in Java](https://www.baeldung.com/java-try-with-resources)

