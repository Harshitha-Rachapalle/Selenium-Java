<?xml version="1.0" encoding="UTF-8"?> <project xmlns="http://maven.apache.org/POM/4.0.0"          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"          xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">     <modelVersion>4.0.0</modelVersion>     <groupId>com.penske</groupId>     <artifactId>Penske-automation</artifactId>     <version>1.0</version>     <name>Penske-automation</name>     <properties>         <com.penske.core.version>3.1.2</com.penske.core.version>         <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>         <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>         <selenium.version>4.34.0</selenium.version>         <maven.surefire.version>3.2.5</maven.surefire.version>         <exec.maven.plugin.version>1.6.0</exec.maven.plugin.version>         <maven.failsafe.version>3.4.0</maven.failsafe.version>         <org.projectlombok.version>1.18.22</org.projectlombok.version>         <maven.failsafe.ignore.fails>true</maven.failsafe.ignore.fails>         <junit.jupiter.version>5.11.0</junit.jupiter.version>         <junit.platform.version>1.11.0</junit.platform.version>         <input.log4j.log.dir>${project.build.directory}/logs/</input.log4j.log.dir>         <output.log4j.log.dir>${project.build.directory}/final-log-file/</output.log4j.log.dir>         <aggregateReport>true</aggregateReport>         <cucumber.version>7.14.0</cucumber.version>         <maven.install.plugin.version>2.5.2</maven.install.plugin.version>         <maven.jar.plugin.version>3.0.2</maven.jar.plugin.version>         <maven.compiler.plugin.version>3.7.0</maven.compiler.plugin.version>     </properties>     <profiles>         <profile>             <id>rsa_run</id>             <properties>                 <maven.failsafe.includes>                     RsaJunitRunner.class                 </maven.failsafe.includes>             </properties>         </profile>     </profiles>     <build>         <plugins>             <!--  define Java version for environment  -->             <plugin>                 <groupId>org.apache.maven.plugins</groupId>                 <artifactId>maven-compiler-plugin</artifactId>                 <version>3.7.0</version>                 <configuration>                     <source>17</source>                     <target>17</target>                     <showDeprecation>true</showDeprecation>                 </configuration>             </plugin>             <!-- maven-surefire-plugin -->             <plugin>                 <groupId>org.apache.maven.plugins</groupId>                 <artifactId>maven-surefire-plugin</artifactId>                 <version>${maven.surefire.version}</version>                 <configuration>                     <skip>true</skip>                 </configuration>             </plugin>             <plugin>                 <groupId>org.apache.maven.plugins</groupId>                 <artifactId>maven-failsafe-plugin</artifactId>                 <version>${maven.failsafe.version}</version>                 <executions>                     <execution>                         <id>RUN-PARALLEL-TESTS</id>                         <phase>test</phase>                         <goals>                             <goal>integration-test</goal>                             <goal>verify</goal>                         </goals>                         <configuration>                             <includes>                                 <!--suppress UnresolvedMavenProperty -->                                 <include>${maven.failsafe.includes}</include>                             </includes>                             <properties>                                 <configurationParameters>                                     cucumber.junit-platform.naming-strategy=long                                     rerunFailingTestsCount=1                                 </configurationParameters>                             </properties>                         </configuration>                     </execution>                 </executions>                 <configuration>                     <printSummary>true</printSummary>                     <testFailureIgnore>true</testFailureIgnore>                     <shutdown>kill</shutdown>                     <systemPropertyVariables>                         <java.awt.headless>true</java.awt.headless>                     </systemPropertyVariables>                     <useSystemClassLoader>true</useSystemClassLoader>                     <argLine>-Xms1024m</argLine>                     <argLine>-Xmx4096m</argLine>                 </configuration>             </plugin>             <!-- execute custom class method -->             <plugin>                 <groupId>org.codehaus.mojo</groupId>                 <artifactId>exec-maven-plugin</artifactId>                 <version>${exec.maven.plugin.version}</version>                 <executions>                     <execution>                         <id>KILL-ALL-EXISTED-WEBDRIVERS-BEFORE-TESTS</id>                         <phase>process-test-classes</phase>                         <goals>                             <goal>java</goal>                         </goals>                         <configuration>                             <classpathScope>test</classpathScope>                             <mainClass>com.penske.core.framework.precondition.EnvironmentCleaner</mainClass>                         </configuration>                     </execution>                     <execution>                         <id>KILL-ALL-EXISTED-WEBDRIVERS-AFTER-TESTS</id>                         <phase>post-integration-test</phase>                         <goals>                             <goal>java</goal>                         </goals>                         <configuration>                             <classpathScope>test</classpathScope>                             <mainClass>com.penske.core.framework.precondition.EnvironmentCleaner</mainClass>                         </configuration>                     </execution>                     <execution>                         <id>MERGE-LOG4J-LOG-FILES-TO-ONE</id>                         <phase>post-integration-test</phase>                         <goals>                             <goal>java</goal>                         </goals>                         <configuration>                             <classpathScope>test</classpathScope>                             <mainClass>com.penske.core.framework.collectors.log.LogCollector</mainClass>                             <systemProperties>                                 <systemProperty>                                     <key>input.log4j.log.dir</key>                                     <value>${input.log4j.log.dir}</value>                                 </systemProperty>                                 <systemProperty>                                     <key>output.log4j.log.dir</key>                                     <value>${output.log4j.log.dir}</value>                                 </systemProperty>                             </systemProperties>                         </configuration>                     </execution>                 </executions>                 <configuration>                     <commandlineArgs>-Xms512m</commandlineArgs>                     <commandlineArgs>-Xmx2048m</commandlineArgs>                 </configuration>             </plugin>             <!--  to exclude empty jar file generation  -->             <plugin>                 <groupId>org.apache.maven.plugins</groupId>                 <artifactId>maven-jar-plugin</artifactId>                 <version>3.0.2</version>                 <configuration>                     <skipIfEmpty>true</skipIfEmpty>                 </configuration>             </plugin>             <!--  to exclude jar file install  -->             <plugin>                 <groupId>org.apache.maven.plugins</groupId>                 <artifactId>maven-install-plugin</artifactId>                 <version>2.5.2</version>                 <configuration>                     <skip>true</skip>                 </configuration>             </plugin>         </plugins>     </build>     <dependencies>         <dependency>             <groupId>io.opentelemetry</groupId>             <artifactId>opentelemetry-sdk</artifactId>             <version>1.53.0</version>         </dependency>         <dependency>             <groupId>com.penske</groupId>             <artifactId>automation-browser-core</artifactId>             <version>${com.penske.core.version}</version>         </dependency>         <dependency>             <groupId>com.penske</groupId>             <artifactId>ZephyrScale-core</artifactId>             <version>2.0.4</version>         </dependency>         <dependency>             <groupId>commons-lang</groupId>             <artifactId>commons-lang</artifactId>             <version>2.6</version>         </dependency>         <dependency>             <groupId>org.projectlombok</groupId>             <artifactId>lombok</artifactId>             <version>${org.projectlombok.version}</version>             <scope>provided</scope>         </dependency>         <dependency>             <groupId>joda-time</groupId>             <artifactId>joda-time</artifactId>             <version>2.10.14</version>         </dependency>         <dependency>             <groupId>io.cucumber</groupId>             <artifactId>cucumber-java</artifactId>             <version>${cucumber.version}</version>             <scope>test</scope>         </dependency>         <dependency>             <groupId>io.cucumber</groupId>             <artifactId>cucumber-junit-platform-engine</artifactId>             <version>${cucumber.version}</version>             <scope>test</scope>         </dependency>         <dependency>             <groupId>org.junit.platform</groupId>             <artifactId>junit-platform-engine</artifactId>             <version>${junit.platform.version}</version>         </dependency>         <dependency>             <groupId>org.junit.platform</groupId>             <artifactId>junit-platform-suite</artifactId>             <version>${junit.platform.version}</version>             <scope>test</scope>         </dependency>         <dependency>             <groupId>org.junit.jupiter</groupId>             <artifactId>junit-jupiter</artifactId>             <version>${junit.jupiter.version}</version>             <scope>test</scope>         </dependency>         <dependency>             <groupId>org.junit.platform</groupId>             <artifactId>junit-platform-launcher</artifactId>             <version>${junit.platform.version}</version>             <scope>test</scope>         </dependency>         <dependency>             <groupId>org.junit.platform</groupId>             <artifactId>junit-platform-commons</artifactId>             <version>${junit.platform.version}</version>         </dependency>         <dependency>             <groupId>org.junit.platform</groupId>             <artifactId>junit-platform-suite-commons</artifactId>             <version>${junit.platform.version}</version>             <scope>runtime</scope>         </dependency>         <dependency>             <groupId>org.junit.platform</groupId>             <artifactId>junit-platform-reporting</artifactId>             <version>${junit.platform.version}</version>         </dependency>         <dependency>             <groupId>net.masterthought</groupId>             <artifactId>cucumber-reporting</artifactId>             <version>5.7.5</version>         </dependency>         <dependency>             <groupId>org.seleniumhq.selenium</groupId>             <artifactId>selenium-java</artifactId>             <version>${selenium.version}</version>         </dependency>         <dependency>             <groupId>com.penske</groupId>             <artifactId>roadside24x7-tools</artifactId>             <version>2.2.6</version>         </dependency>     </dependencies>     <dependencyManagement>         <!--  SELENIUM  -->         <dependencies>             <dependency>                 <groupId>org.seleniumhq.selenium</groupId>                 <artifactId>selenium-java</artifactId>                 <version>${selenium.version}</version>             </dependency>             <dependency>                 <groupId>com.penske</groupId>                 <artifactId>ZephyrScale-core</artifactId>                 <exclusions>                     <exclusion>                         <groupId>com.intuit.karate</groupId>                         <artifactId>*</artifactId>                     </exclusion>                     <exclusion>                         <groupId>io.cucumber</groupId>                         <artifactId>*</artifactId>                     </exclusion>                 </exclusions>             </dependency>             <dependency>                 <groupId>com.penske</groupId>                 <artifactId>automation-browser-core</artifactId>                 <exclusions>                     <exclusion>                         <groupId>io.cucumber</groupId>                         <artifactId>cucumber-testng</artifactId>                     </exclusion>                 </exclusions>             </dependency>         </dependencies>     </dependencyManagement> </project> explain in depth for a beginner 

No worries — this is a big, real-world Maven pom.xml, so feeling overwhelmed as a beginner is totally normal 🙂
I’ll break it down piece by piece, explain what each section does, and why it exists, using simple language and real examples.

1️⃣ What is this file overall?

This is a Maven POM file (pom.xml).

Think of it as:

📦 The instruction manual for your Java automation project

It tells Maven:

What your project is

Which Java version to use

Which libraries (dependencies) you need

How tests should run

What should happen before/after tests

2️⃣ Top-level project identity
<modelVersion>4.0.0</modelVersion>
<groupId>com.penske</groupId>
<artifactId>Penske-automation</artifactId>
<version>1.0</version>
<name>Penske-automation</name>

What this means:
Tag	Meaning
modelVersion	Maven version (almost always 4.0.0)
groupId	Company or organization name
artifactId	Project name
version	Project version
name	Human-readable name

📌 Together they uniquely identify your project:

com.penske:Penske-automation:1.0

3️⃣ <properties> – reusable configuration values
<properties>
    <selenium.version>4.34.0</selenium.version>
    <junit.jupiter.version>5.11.0</junit.jupiter.version>
    ...
</properties>

Why this exists:

Instead of repeating versions everywhere, you define them once and reuse them.

Example:

<version>${selenium.version}</version>

Benefits:

✅ Easy upgrades
✅ Less mistakes
✅ Cleaner POM

4️⃣ Maven profiles – conditional behavior
<profiles>
    <profile>
        <id>rsa_run</id>
        <properties>
            <maven.failsafe.includes>
                RsaJunitRunner.class
            </maven.failsafe.includes>
        </properties>
    </profile>
</profiles>

What is a profile?

A profile lets you change behavior based on how Maven is run.

Example:

mvn clean test -P rsa_run


This profile:

Runs only RsaJunitRunner.class

Useful for:

Smoke tests

Environment-specific runs

Partial test execution

5️⃣ <build> section – how the project is built and tested

This is the engine room of the POM.

5.1 Maven Compiler Plugin (Java version)
<maven-compiler-plugin>
    <source>17</source>
    <target>17</target>
</maven-compiler-plugin>


📌 Tells Maven:

Use Java 17

Compile source code to Java 17 bytecode

Without this, Maven might default to an older Java version.

5.2 Surefire Plugin – unit tests (disabled)
<maven-surefire-plugin>
    <configuration>
        <skip>true</skip>
    </configuration>
</maven-surefire-plugin>

What is Surefire?

Runs unit tests

Usually runs:

*Test.java

*Tests.java

🚫 Here it is disabled, meaning:

Unit tests are not run

This project relies on Failsafe instead

5.3 Failsafe Plugin – integration & automation tests
<maven-failsafe-plugin>


This is very important for your automation framework.

Why Failsafe?

Designed for integration tests

Perfect for:

Selenium tests

Cucumber tests

End-to-end automation

Execution lifecycle
<phase>test</phase>
<goals>
    <goal>integration-test</goal>
    <goal>verify</goal>
</goals>


This means:

Run automation tests

Verify results after execution

Includes (which tests to run)
<include>${maven.failsafe.includes}</include>


Controlled by:

Profiles

Command line

Properties

Cucumber & retry config
<configurationParameters>
    cucumber.junit-platform.naming-strategy=long
    rerunFailingTestsCount=1
</configurationParameters>


📌 This means:

Better test naming in reports

Retry failed tests once

Other important settings
<testFailureIgnore>true</testFailureIgnore>
<shutdown>kill</shutdown>


Tests can fail without stopping the build

Browser processes are killed if stuck

JVM memory settings ⚠️

⚠️ Issue here (important for beginners)

<argLine>-Xms1024m</argLine>
<argLine>-Xmx4096m</argLine>


🚨 Only the last argLine is used

✅ Correct version:

<argLine>-Xms1024m -Xmx4096m</argLine>

6️⃣ Exec Maven Plugin – running Java code during build

This plugin runs custom Java classes at certain phases.

Before tests: clean environment
<phase>process-test-classes</phase>
<mainClass>
    com.penske.core.framework.precondition.EnvironmentCleaner
</mainClass>


📌 This:

Kills existing WebDriver instances

Prevents test flakiness

After tests: cleanup & log merge
<post-integration-test>


Runs:

Browser cleanup

Log file merge into one final log

This is enterprise-level test hygiene 👍

7️⃣ Jar & Install plugins – disabled
<maven-jar-plugin>
    <skipIfEmpty>true</skipIfEmpty>
</maven-jar-plugin>

<maven-install-plugin>
    <skip>true</skip>
</maven-install-plugin>


Why?

This is a test automation project

You don’t want:

JAR packaging

Installation to local Maven repo

8️⃣ <dependencies> – libraries your code uses

This is your toolbox.

Key dependencies explained
Selenium
<selenium-java>


Browser automation

WebDriver API

Cucumber
<cucumber-java>
<cucumber-junit-platform-engine>


BDD testing

Gherkin (Given / When / Then)

Integrates with JUnit 5

JUnit 5
junit-jupiter
junit-platform


Test framework

Test discovery

Reporting

Lombok
<scope>provided</scope>


Generates getters/setters

Reduces boilerplate

Not included in final build

Company internal libraries
automation-browser-core
ZephyrScale-core
roadside24x7-tools


Custom Penske frameworks

Reporting, browser handling, integrations

9️⃣ <dependencyManagement> – version control & exclusions

This section:

Controls dependency versions globally

Removes unwanted transitive dependencies

Example:

<exclusion>
    <groupId>io.cucumber</groupId>
    <artifactId>*</artifactId>
</exclusion>


📌 Prevents:

Version conflicts

Duplicate cucumber libraries

Classpath issues

🔟 Big picture summary
What this project is:

✅ Java 17
✅ Selenium automation
✅ Cucumber BDD
✅ JUnit 5
✅ Maven Failsafe (not Surefire)
✅ Enterprise logging & cleanup
✅ Profile-based execution

If you want next:

I can:

Draw a simple diagram of Maven lifecycle

Explain Surefire vs Failsafe visually

Show how a Cucumber test flows from Maven → browser

Review this POM for best practices & improvements

Just tell me 👍

yes please

Nice 😄 let’s build this up step-by-step and visually, like you’re learning the story of what happens when you run Maven.

I’ll cover three things, in the exact order beginners usually “get it”:

1️⃣ Maven lifecycle (simple diagram)
2️⃣ Surefire vs Failsafe (why your project chose Failsafe)
3️⃣ End-to-end flow: mvn test → Cucumber → Selenium → Browser

1️⃣ Maven lifecycle (the big picture)

Think of Maven as a conveyor belt.
Each phase runs in order.

clean
  ↓
validate
  ↓
compile
  ↓
test
  ↓
package
  ↓
verify
  ↓
install
  ↓
deploy


👉 When you run:

mvn test


Maven automatically runs everything before test too.

Where your project plugs in
Phase	What your project does
compile	Compiles Java 17 code
process-test-classes	Kills old browsers (exec plugin)
test	Runs Failsafe integration tests
verify	Confirms test results
post-integration-test	Cleanup + log merge

📌 Your automation does NOT use package, install, or deploy
This is normal for test frameworks.

2️⃣ Surefire vs Failsafe (SUPER important)

This confuses almost everyone at first — you’re not alone.

Maven Surefire (UNIT tests)

What it’s for:

Small tests

No browser

Fast

Example:

@Test
void addTwoNumbers() {}


Default naming:

*Test.java
*Tests.java


🚫 In your project:

<skip>true</skip>


👉 Disabled on purpose

Maven Failsafe (INTEGRATION / AUTOMATION tests)

What it’s for:

Selenium

Cucumber

End-to-end flows

Slower, real browsers

Typical tests:

Login flows

API + UI combined

Full business scenarios

✅ That’s why your project uses:

maven-failsafe-plugin

Why not Surefire for Selenium?

Imagine this situation:

Browser opens → test fails → build stops → browser never closes 😬

Failsafe solves this by:

Running cleanup after tests

Separating test execution from verification

Allowing retries & recovery

💡 Rule of thumb

Surefire  → unit tests
Failsafe → automation / selenium / cucumber

3️⃣ Full execution flow (your project, end to end)

Let’s walk through what happens when you run:

mvn clean test -P rsa_run

STEP 1: Maven reads the POM

Maven learns:

Java version = 17

Selenium = 4.34.0

Use profile = rsa_run

Test runner = RsaJunitRunner.class

STEP 2: Environment cleanup (before tests)

Phase: process-test-classes

EnvironmentCleaner


What it does:

Kills ChromeDriver / GeckoDriver

Cleans stuck browser processes

📌 This prevents flaky test failures.

STEP 3: Failsafe starts tests

Phase: test

Failsafe:

Finds RsaJunitRunner.class

Loads JUnit Platform

Boots Cucumber engine

STEP 4: Cucumber kicks in 🥒

Cucumber does this:

.feature file
   ↓
Step Definitions
   ↓
Java code


Example:

Given user is on login page
When user enters valid credentials
Then dashboard is displayed

STEP 5: Selenium takes control 🌐

Inside step definitions:

WebDriver driver = new ChromeDriver();
driver.get("https://...");


Selenium:

Launches browser

Clicks

Types

Validates UI

STEP 6: Failure handling & retries

If a test fails:

rerunFailingTestsCount=1

Test runs one more time

This reduces:

Network flakiness

Timing issues

STEP 7: Test completion

Phase: verify

Failsafe:

Collects results

Prints summary

Does NOT crash build (by design)

STEP 8: Post-test cleanup

Phase: post-integration-test

Exec plugin runs:

1️⃣ EnvironmentCleaner
→ closes all browsers

2️⃣ LogCollector
→ merges Log4j logs into one file

📌 This is very enterprise-grade 👍

4️⃣ Mental model (remember this)

If you remember only this, you’re solid:

Maven
  ↓
Failsafe
  ↓
JUnit Platform
  ↓
Cucumber
  ↓
Step Definitions
  ↓
Selenium
  ↓
Browser

5️⃣ What most beginners struggle with (you’re ahead already)

Common confusion:

❌ “Why my test doesn’t run?” → profile not activated

❌ “Why Surefire doesn’t work?” → intentionally skipped

❌ “Why browser stays open?” → cleanup phase not reached

❌ “Why duplicate dependencies?” → missing exclusions

You already touched advanced territory just by reading this POM.

Want to go even deeper next?

I can:

Explain RsaJunitRunner.class line by line

Show a minimal sample project version of this POM

Explain Cucumber + JUnit 5 integration visually

Help you debug failing tests using Maven logs

Just tell me what you want to tackle next 🚀

yes 

Perfect 👍 let’s do this the smartest beginner-friendly next step.

We’ll go in this order (this makes everything click fast):

1️⃣ What RsaJunitRunner.class actually is and why it exists
2️⃣ A tiny, minimal example (so you can mentally compare it to your big project)
3️⃣ How Cucumber + JUnit 5 connect (the magic glue)

No Maven jargon overload — promise 🙂

1️⃣ What is RsaJunitRunner.class?

In plain English:

RsaJunitRunner.class is the entry point that tells Maven + JUnit + Cucumber
👉 “Start the automation from here.”

It is NOT a test itself.
It is a test launcher.

Why you need a runner at all

Maven doesn’t understand:

.feature files

Gherkin (Given / When / Then)

Cucumber scenarios

So you need one Java class that says:

“Hey JUnit, please run Cucumber features using these rules.”

That class is your Runner.

2️⃣ What RsaJunitRunner typically looks like

A simplified version (very close to real life):

package com.penske.runners;

import org.junit.platform.suite.api.ConfigurationParameter;
import org.junit.platform.suite.api.IncludeEngines;
import org.junit.platform.suite.api.SelectClasspathResource;
import org.junit.platform.suite.api.Suite;

import static io.cucumber.junit.platform.engine.Constants.GLUE_PROPERTY_NAME;
import static io.cucumber.junit.platform.engine.Constants.PLUGIN_PROPERTY_NAME;

@Suite
@IncludeEngines("cucumber")
@SelectClasspathResource("features")
@ConfigurationParameter(
    key = GLUE_PROPERTY_NAME,
    value = "com.penske.stepdefinitions"
)
@ConfigurationParameter(
    key = PLUGIN_PROPERTY_NAME,
    value = "pretty, html:target/cucumber-report.html"
)
public class RsaJunitRunner {
}


Now let’s decode this slowly 👇

3️⃣ Line-by-line explanation (beginner mode)
@Suite

📌 Tells JUnit:

“This class is a test suite, not a normal test.”

@IncludeEngines("cucumber")

📌 Tells JUnit:

“Use the Cucumber engine, not regular JUnit tests.”

This is why:

.feature files work

Given / When / Then works

@SelectClasspathResource("features")

📌 Means:

src/test/resources/features


JUnit will:

Look inside that folder

Find all .feature files

Execute them

GLUE_PROPERTY_NAME
com.penske.stepdefinitions


📌 This is VERY IMPORTANT.

It tells Cucumber:

“My step definitions live in this package.”

If this is wrong:
❌ Steps show as undefined
❌ Tests don’t run

PLUGIN_PROPERTY_NAME
pretty
html:target/cucumber-report.html


📌 Controls:

Console output

HTML report generation

4️⃣ How Maven finds this runner

Back to your POM 👇

<include>${maven.failsafe.includes}</include>


And your profile 👇

<maven.failsafe.includes>
    RsaJunitRunner.class
</maven.failsafe.includes>


So when you run:

mvn test -P rsa_run


Maven says:

Use Failsafe

Run only RsaJunitRunner.class

JUnit starts

JUnit loads Cucumber

Cucumber runs features

🔥 That’s the full chain.

5️⃣ Ultra-minimal mental example (compare with yours)
Folder structure
src
 └── test
     ├── java
     │   ├── runners
     │   │   └── TestRunner.java
     │   └── steps
     │       └── LoginSteps.java
     └── resources
         └── features
             └── login.feature

login.feature
Feature: Login

  Scenario: Valid login
    Given user is on login page
    When user enters valid credentials
    Then home page is displayed

LoginSteps.java
@Given("user is on login page")
public void userIsOnLoginPage() {
    driver.get("https://example.com/login");
}

TestRunner.java
@Suite
@IncludeEngines("cucumber")
@SelectClasspathResource("features")
@ConfigurationParameter(
  key = GLUE_PROPERTY_NAME,
  value = "steps"
)
public class TestRunner {
}


This tiny setup is exactly the same concept as your big enterprise project — just without the extras.

6️⃣ Why enterprises prefer THIS setup

Your project design gives you:

✅ Clean separation
✅ Reliable cleanup
✅ Retry handling
✅ Profile-based execution
✅ CI/CD friendly automation

This is senior-level Maven usage, not beginner stuff.
