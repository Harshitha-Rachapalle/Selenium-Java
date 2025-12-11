
### 1.How can we run TestNG test cases without Java compiler?

TestNG never depends on the Java compiler directly. It **only runs the compiled .class files**.

When running TestNG through tools like:

testng.xml

IntelliJ / Eclipse

Maven Surefire plugin

Jenkins pipeline

These tools automatically compile the code in the background. TestNG simply picks up the compiled files and executes them.
So we’re running TestNG tests without manually invoking the Java compiler.

### 2. What is the purpose of testng.xml?

The testng.xml file is used to **configure and control the execution** of TestNG test cases.

It defines which classes, methods, or packages to run, allows grouping, parallel execution, parameterization, and test suite management.

It also helps integrate tests into CI/CD tools like Jenkins.

### Why do we need testng.xml when TestNG can use annotations?

Annotations work only inside the code.

But XML provides **external control**, where we manage:

which tests to run

grouping

parallel execution

parameters

multiple classes

ordering

Without modifying Java code.


This makes XML essential in real projects and CI pipelines.

### 3.Can we run TestNG without testng.xml?

Yes, but only individually or class-wise.

We cannot run:

multiple test classes

groups

parallel execution

suite-level configurations

So in real-time project execution, XML is mandatory.

### 4.How does testng.xml help in Data-Driven Testing?

XML provides parameters externally.

We can pass URLs, usernames, passwords, environments etc.

Example:
```
<parameter name="browser" value="chrome"/>
```
Thus the same test runs on different configurations.

### 5.Explain parallel testing using testng.xml

XML allows setting:

**parallel="methods" thread-count="5"**

This runs test methods concurrently, improving execution speed.

**Used for:**

cross-browser testing

speeding up large test suites

Selenium Grid executions

### 6.What are the advantages of including/excluding tests in XML?

We can skip certain tests without touching Java code.

Example:

<exclude name="paymentTest"/>

Useful for:

failing modules

partial deployments

smoke/regression cycles

### 7. How do you run test suites in Jenkins?

We configure Jenkins to run:
```
mvn test -DsuiteXmlFile=testng.xml
```

XML becomes the entry point for automation pipeline.

### 8. What is priority in TestNG?

Priority in TestNG is used **to control the order of execution of test methods**.

**Lower** number means **higher priority**. This helps us execute tests in a logical business flow or ensure critical test cases run first.

### 9. What happens if two test methods have the same priority?

If two tests have the same priority, TestNG resolves the tie by **executing them in alphabetical order of the method name**.

### 10. What is the default priority if not specified?

The **default priority is 0**, and such tests run before tests with positive priorities—but still in alphabetical order.

### 11.Can we use negative priorities?

Yes. TestNG accepts negative values. A test with **priority -1 runs before priority 0**, which is useful when you want to enforce some test as the absolute first execution step.

### 12. Real-time scenario: How do you prioritize test cases in a workflow?

In real-time projects, we follow the business flow.

For example, in an e-commerce application:

login → search → add to cart → checkout

I assign priority in increasing order to ensure the flow is followed.

This prevents failures caused by out-of-sequence execution and makes CI runs predictable.

### 13. How is priority different from dependsOnMethods?

priority controls sequence, but does not enforce dependency.

dependsOnMethods ensures a **test will run only if its dependent test passes**.

In workflow-dependent scenarios, we often combine both.

### 14. In your project, where did you use TestNG priority?

In my previous project, we used priority for:

Running critical functional tests first

Maintaining workflow sequence for our end-to-end scenarios

Ensuring smoke tests run before full regression

Making CI execution faster by failing early if critical flows broke

This made our pipeline more stable and reduced debugging time.
