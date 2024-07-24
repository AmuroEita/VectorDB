# Vexless: A Serverless Vector Data Management System Using Cloud Functions

## 1. What is the problem?
With the further development of large language models (LLMs), the demand for database systems that can skillfully manage high-dimensional vector data is increasing. At the same time, the emergence of serverless computing introduces a novel approach to cloud computing that is both on-demand and flexible. 

This research addresses the technical hurdles associated with building vector databases by using serverless cloud functions, such as dealing with limited resources, minimizing communication costs, and mitigating the effects of cold starts.


## 2. Why is it hard? 
Cloud functions come with limitations that manifest as performance challenges.

Dynamic resource allocation, where the cloud provider assigns varying amounts of computational resources for each function call, can hinder consistent performance. Additionally, reliance on network connectivity introduces latency issues as functions interact with other services. Finally, cold start latency adds a performance hurdle, as functions experience a delay when invoked for the first time to spin up and load code. These limitations necessitate careful design considerations to ensure optimal performance in serverless environments. 


## 3. Why do existing solutions not work?

Existing vector database solutions do not align well with cloud functions due to the stateless and resource-constrained nature of serverless environments, leading to challenges such as cold start delays and scalability issues. Their designs often do not account for the unique cost structures of cloud functions, where the pay-as-you-go model can lead to higher expenses if not properly optimized. 

Traditional systems also lack the specialized optimizations needed for efficient vector data management, such as handling high-dimensional data and implementing fast ANN search algorithms. 


## 4. What is the core intuition for the solution?
The core intuition behind the Vexless solution is to overcome the specific challenges of using cloud functions for vector database management by:

### 1. Building index and search strategy: 

Using the constrained K-Means clustering algorithm to create shards of the dataset helps to maintain balance across instances and provides a more accurate nearest neighbor search during the search phase by adding a small number of boundary nodes to each cluster. And using the HNSW in the implementation, because of its great scalability with higher dimensional vectors and great balance between accuracy and query speed. In addition, the indexing phase introduces a trade-off redundancy.

### 2. Reducing communication overhead:

Vexless introduces a hybrid serverless architecture and uses stateful Functions (Azure Durable Functions) as coordinators, and uses message queue services such as Azure Queue Storage to achieve efficient communication among different functions. This method has lower overhead than two common server less function communication methods, the global storage based method and the HTTP-based method.

### 3. Minimizing cold-start delays:

On the pay-as-you-go serverless platform, cloud functions experience cold startup issues after a period of inactivity or when there are no hot instances available to handle external requests, which leads to initialization before actual execution, including allocating compute and memory resources, loading function code, etc., which increases latency. Vexless introduces a novel approach to reduce cold start problems, two strategies - the "Rewind Mechanism" and the "Diligent Search Before Laziness Mechanism" - are used to dynamically adjust the keep-alive time according to query workload. It can ensure quick  response times and reduce performance limitations caused by cold starts.


## 5. Does the paper prove its claims?

Yes, this research proves its claims through a series of experiments that evaluate Vexless against traditional VM-based and naive cloud function implementations. Using datasets like SIFT10M, DEEP10M and GIST1M, the experiments assess performance under various workloads, focusing on latency, recall rates, and cost. The results show Vexless's superior cost-efficiency and query performance, especially under bursty and sparse workloads. An ablation study further validates the individual design choices, such as sharding strategy and communication optimization. 


## 6. What is the setup of analysis/experiments? Is it sufficient?

The aspects of the experiment setup:

### 1. Datasets: 
Three datasets in Approximate Nearest Neighbor (ANN) research: SIFT10M, DEEP10M  and GIST1M. 

### 2. Query pattern and workloads:
Various query workloads, including sparse, bursty, continuous, and real-world workloads.

### 3. Evaluation metrics:
The primary metrics for evaluation are query latency, measured in milliseconds, and accuracy, measured by recall rates.

### 4. Experimental environments:
The baseline solution uses an Azure compute-optimized VM with a specific CPU configuration and memory size to support vector search workloads.

### 5. Experimental methodology:
Comparing the optimized strategies in Vexless with HNSW index running vector searches on a cloud VM as the baseline.

### 6. Ablation study:
Isolating and evaluating the impact of different components and design choices within the Vexless system. 

### 7. Additional experiments:
Further analyzing and validating the performance and cost-efficiency of the Vexless system under various real-world conditions.

The setup of experiments is sufficient, the use of standard datasets and a variety of workloads ensures the results can be compared against other research. And the comparison with baselines and the inclusion of an ablation study provide a deep understanding of the system's performance and the impact of its optimizations. It covers various aspects of system evaluation, including performance and cost-efficiency.


## 7. Are there any gaps in the logic/proof?
No, there are no explicit gaps in logic or proof. The paper follows a logical structure in research and the experiment presents a comprehensive among Vexless, VM-based and naive cloud function implementations, covering different aspects, especially focusing on the aspects of performance and cost-efficiency. 

## 8. Describe at least one possible next step.
Potential next step for further research could be:

### 1. Disaster recovery solution：
The paper does not mention how the system deals with server failures, This could be an area for further investigation in future experiments.

### 2. Support video and audio search:
The paper proposes a method for processing image datasets.  Could it be adapted to handle video and audio data as well? To support the retrieval of various data types, the system may need some adjustments.

