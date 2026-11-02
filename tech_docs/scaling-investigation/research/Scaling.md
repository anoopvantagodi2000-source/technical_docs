<h1>Introduction</h1>
<h3>What is Scalability.</h3>
<p>A system's ability to handle increasing workloads, users, and data while maintaining acceptable performance.</p>
<h3> Why Scalability?</h3>
<p>As the number of incoming requests to a system increases, the workload also increases. CPU utilization and memory consumption may rise, which can cause higher latency when system resources become insufficient. This can negatively affect customer experience and lead to financial losses for the organization.</p>
<h3>Scaling Approaches.</h3>
<p>There are two scaling approaches for systems.</p>
<ul>
  <li>Vertical Scaling (Scale up)</li>
  <li>Horizontal Scaling (Scale Down)</li>
</ul>
<div>
   <h2>Vertical Scaling (Scale Up)</h2>
    <section>
     <h4>What is Vertical Scaling?</h4>
      <p>The process of increasing capacity of  resources of system by adding more hardware resources such as CPU,memory (RAM) , Storage.</p>
      <h5>How does it improve System performance?</h5>
      <p> Vertical scaling improves System performance by providing addition hardware resources such as RAM and CPU</p>
      <p> Let's see how upgrading RAM and CPU in System improves performances</p>
     <ul>
      <h4>Upgrading Memory(RAM)</h4>
       <li> when system runs out of available primary memory then system moves memory pages into hard disk as we know reading from hard disk is comparatively 
           slower then reading from RAM and moving pages from primary memory to hard disk is known has memory swap, as memory swaping lead to decrease in system performance, 
           by increasing RAM capacity we are reducing the memory swaps</li>
      <li> Databases uses available memory to cache the most frequently accessed data and indexes as we increase the RAM we can store more frequently 
         accessed data and index which reduces IO operation Which inturn increases the performance.</li>
       <h4>Upgrading CPU cores</h4>
      <li>Increasing the number of CPU cores can improve an application's ability to process multiple computational tasks concurrently.

<li>Each CPU core can execute instructions independently. With additional cores, applications that support parallel execution can process more tasks simultaneously.</li>

<li>This can improve performance when handling computationally intensive workloads and concurrent user requests.</li>


</ul>
<h3> Limitations of Vertical Scaling.</h3>
<ul><li>
Hardware ceiling - once you buy larger available RAM AND CPU from the market we can't again scale system further</li>
<li>Single Point of failure - once server crashes the entire application goes down and there is not System has back.</li>
 <li> Required Downtime (upgrade) - when we want to upgrade hardware resources we need to take down server.</li>
<li>Cost of maintenance - cost of hardware resources increase exponentially. </li>
</ul>

   <h3> Use cases of Vertical Scaling</h3>
   <ul>
   <li>Database servers -	More RAM for caching and more CPU for query processing.</li>
   <li>Monolithic applications-	Increases capacity without introducing multiple instances.</li>
  <li>Memory-intensive applications -Provides additional memory for processing.</li>
   <li>CPU-intensive applications -Provides more computational capacity.</li>
   <li>Predictable workload -Offers a simpler scaling approach.</li>
</ul>

   </section>
</div>

<div>
   <section>
    <h2>Horizontal Scaling (Scale Down)</h2>
    <h4>What is Horizontal Scaling?</h4>
    <p>It's the process of increasing application performance by adding more server or application instance 
     without upgrading  hardware resources.</p>
    <h4>How does it work?</h4>
    <ul>
     <li>In Horizontal scaling multiple application instance are created.</li>
     <li>With the help of load balancing we can distribute incoming traffic to these instances, 
      each assigned request are processed by these instances , can share same resource such as database to retrieve the data and store the data.</li>
      <li>By distributing workloads across multiple instances, the system can handle increased traffic and improve its overall processing capacity.</li>
  
   </ul>
    <h3>Horizontal scaling also introduces some new problems.</h3>
    <h4> Session Management</h4>
     <p>
         In Horizontally scaled application each instances has some memory, session stored in one memory not available to another instance,
        to avoid this problem we need to use Redis which act has shared resources between session and also provides faster look ups.
     </p> 
    <h4>Database Bottleneck</h4>
      <p>If multiple instances access same databases then there will database performance bottleneck</p>
      <p>Concurrent queries from different instances to the shared database leads to performance issues such as: </p>
      <ul><li>High CPU and memory utilization.</li>

<li>Database connection exhaustion.</li>

<li>Increased query execution time.</li>

<li>Increased response latency.</li>
</ul>
 <p> Above issues can be resolved by.</p>
   <ul>
   <li>Connection Pooling: Reuses existing database connections instead of establishing new connections for every request.</li>

<li>Database Read Replicas: Distributes read queries across multiple database instances.</li>

<li>Database Sharding: Distributes data across multiple database instances to handle larger datasets and workloads.</li>

<li>Caching: Stores frequently accessed data in a faster storage layer to reduce repeated database queries.</li>

<li>Database Indexing: Improves query performance by helping the database locate records more efficiently.</li>
</ul>
 <h4>Data Consistency</h4>
  <p>When multiple application instances access and modify shared data, maintaining data consistency becomes important.</p>
  <p>The application and database must handle concurrent updates correctly to prevent inconsistent data.Database replication can also introduce replication lag, where changes made to the primary database are not immediately available on read replicas.</p>
 <h3>
   Advantages of Horizontal Scaling
 </h3>
  <li>Increased Processing Capacity</li>
  <li>Improved Fault Tolerance</li>
  <li>High Availability</li>
 <li>Better Workload Distribution.</li>

    
   </section>
   
   
</div>

## Load balancing

### What is Load balancing?
It helps us to distribute incoming requests evenly to our server instances such way that no single instance of server gets overwhelming request.

### Why do we need it?
1. __Distribute Traffic__: Shares the workload among a group of servers.
2. __Health Checks__: Continuously checks if servers are working. If Server 2 crashes, the load balancer instantly stops sending traffic there and routes it to Servers 1 and 3 instead.
3. __Keep Apps Online__: Allows you to take a server down for maintenance or updates without taking the whole website offline.

## Algorithms
1. Round Robin - Passes traffic down the line in order (Server 1, then 2, then 3, then repeat).
2. Least Connections: Sends the next user to whichever server is handling the fewest active tasks at that exact moment.
3. IP Hashing: Uses the user's IP address to ensure a specific user always connects to the exact same server (useful for keeping them logged in on older systems).


### How to Handle User Data (Sessions)

• The Stateful Way (Sticky Sessions): The load balancer forces a user to stay stuck to one specific server because that server holds their temporary data (like a shopping cart). If that server dies, the data is lost.

• The Stateless Way (Modern): Servers don't store user data locally. Instead, they all read from a shared, central database. The load balancer can freely send the user to any server on any click, making the system much more reliable.


### The Backup Cop (High Availability)

If you only have one load balancer, and it crashes, your entire app goes down. To prevent this, production systems use an Active-Passive Pair:

• Active Balancer: Handles 100% of the live traffic.

• Passive Balancer: Stands by silently, watching the active one.

• If the active balancer fails, the passive one instantly takes over in milliseconds. Your users never notice a thing.
