# Future vs. Queuable Apex

## Future Methods 
* Future method should return void and it should be declared as static.
* Can we pass objects as a parameter to the future method? No, we cannot pass the objects (custom, standard) as parameter. We can only pass the Id’s as a parameter, primitive data type or collection of primitive data types as a parameter.
* Why we cannot pass the object (custom, standard) as a parameter? Remember we are working with asynchronous apex they will execute only when the resources are available so if we use the object as a parameter then the system will not hold the current data.
* Did future method execute in the same order they called? The answer is no, as they are not guaranteed to execute in the same order. So, can we call it a limitation? Yes, we can overcome this by using the queueable apex.
* Will future methods run concurrently which means updating the same record by 2 future methods at a same time from different sources or the same source? Yes, and which could result in locking the record. so, need to incredibly careful while dealing with future methods.
* Do I have to take any special care while writing the test class for future methods? Yes, you need to enclose the code between start and stop test methods.
* Can I use future method for bulk transactions? The answer will not be likeable, but it is good to avoid future and good to use batch processing.
* What is the limit on future calls per apex invocation? 50, additionally there are limits base on the 24-hour time frame.
* Can we user future method in visual force controller constructors? The answer is no.
* Can I call a future method from another future method? No, you cannot call one future method from another future method.
* Can I monitory future jobs? No, it is a method that resides in an apex class and it will execute whenever the resources are available. So, how can we resolve this? Is there any way that I can implement asynchronous jobs and monitor them?
* Can we implement chain process? Forget with future because we cannot call future method from another future method. So, how can we resolve this?

## Considerations when using future methods
* Avoid adding large numbers of future methods to the asynchronous queue, if possible. If more than 2,000 unprocessed requests from a single organization are in the queue, any additional requests from the same organization will be delayed while the queue handles requests from other organizations.
* Ensure that future methods execute as fast as possible. To ensure fast execution of batch jobs, minimize Web service callout times and tune queries used in your future methods. The longer the future method executes, the more likely other queued requests are delayed when there are a large number of requests in the queue.
* Test your future methods at scale. Where possible, test using an environment that generates the maximum number of future methods you’d expect to handle. This will help determine if delays will occur.
* Consider using batch Apex instead of future methods to process large numbers of records.
* To check how many asynchronous Apex executions are available, make a request to the REST API limits resource. See List Organization Limits in the REST API Developer Guide
* *Future method jobs queued before a Salesforce service maintenance downtime remain in the queue. After service downtime ends and when system resources become available, the queued future method jobs are executed. If a future method was running when downtime occurred, the future method execution is rolled back and restarted after the service comes back up.*
* If more than 2,000 unprocessed requests from a single organization are in the queue, any additional requests from the same organization will be delayed while the queue handles requests from other organizations
*  We cannot call future methods directly from batch apex but we can call a web service from batch class and that web service can call the @future method. Also, we can call the future method from finish method in the batch class.

## Queueable Apex
Queueable apex is hybrid class. Why you are calling it as hybrid class? Yes, I mean to say it is designed in a way by combining the batch apex and future apex. So, that means do we have to again write the start, execute, and finish methods like the batch. As I said it is a combination not the exact replica. No, you don’t need to write start and finish methods. All you need is execute method

### Queueable Apex solves the following Future limitations
* Can we pass objects, apex types as a parameter to the future method? No, we cannot pass them as arguments.
* Can I monitory future jobs? No, it is a method that resides in an apex class and it will execute whenever the resources are available. Is there any way that I can implement asynchronous jobs and monitor them?
* Can we implement chain process? Forget with future because we cannot call future method from another future method. *(e.g. send an email to Admin for the failed object updates executed in the first queueable by chaining a susequent process to send the email to admins)*

### Limitations for queueable apex
* How many jobs can we schedule from queueable apex? Only one.
* Can I chain the jobs from test class? No, you cannot do that unless you want to end up with errors.
* How many jobs I can add by using system.enqueueJob for a single transaction? 50
* What is the stack depth for chaining the jobs? In developer edition it is limited to 5 including the parent job.
* Which one is easy to modify queueable or future? The answer is future as it resides in the apex class and easy to remove the annotation “@future”. Does it impact the test class no it won’t impact the test class? How cool it is right switching between synchronous to asynchronous and vice versa

## Differences between Future and queueable apex
FUTURE APEX	| QUEUEABLE APEX
----------- | --------------
It is annotation based so we can use the same apex class to write the future method. Syntax: @future |	It is a class which implements’ queueable interface. Syntax: public class CLASS_NAME implements Queueable{}
We cannot monitor the jobs	| We can monitor the jobs based on the job Id.
we cannot call a future from another future or batch apex. The limit on future method for single apex invocation is 50.	| We can chain the Queueable jobs and the stack depth in developer org is 5 and in enterprise edition you can chain 50 jobs.
Future method supports only primitive datatypes	| Queueable supports both primitive and non-primitive data types.

## Sources
https://www.forcetalks.com/salesforce-topic/can-we-call-a-future-method-from-a-salesforce-batch-class-using-apex/
https://www.forcetalks.com/salesforce-topic/can-we-call-a-future-method-from-a-salesforce-batch-class-using-apex/
https://www.forcetalks.com/salesforce-topic/what-are-the-different-things-which-we-need-to-consider-while-using-future-methods-in-salesforce/
