cucumber-jvm-parallel-plugin
============================

A common approach for running Cucumber features in parallel is to create a suite of Cucumber JUnit runners, one for each suite of tests you wish to run in parallel.  For maximum parallelism, there should be a runner per feature file.

This is a pain to maintain and not very DRY.

This is where the cucumber-jvm-parallel-plugin comes in.  This plugin automatically generates a Cucumber JUnit runner for each feature file found in your project.

Usage
-----

Add the following to your POM file:

```xml
<plugin>
  <groupId>com.github.temyers</groupId>
  <artifactId>cucumber-jvm-parallel-plugin</artifactId>
  <version>1.0-SNAPSHOT</version>
  <executions>
    <execution>
      <id>generateRunners</id>
      <phase>validate</phase>
      <goals>
        <goal>generateRunners</goal>
      </goals>
      <configuration>
          <!-- Mandatory -->
          <!-- comma separated list of package names to scan for glue code -->
         <glue>foo, bar</glue>
         <!-- These are the default values -->
          <!-- Where to output the generated Junit tests -->
           <outputDirectory>${project.build.directory}/generated-test-sources/cucumber</outputDirectory>
           <!-- The diectory containing your feature files.  -->
          <featuresDirectory>src/test/resources/features/</featuresDirectory>
           <!-- Directory where the cucumber report files shall be written  -->
          <cucumberOutputDir>target/cucumber-parallel</cucumberOutputDir>
          <!-- comma separated list of output formats -->
         <format>json</format>
         <!-- CucumberOptions.strict property -->
         <strict>true</strict>
         <!-- CucumberOptions.monochrome property -->
         <monochrome>true</monochrome>
         <!-- The tags to run, maps to CucumberOptions.tags property -->
         <tags>"@complete", "@accepted"</tags>
         <!-- If set to true, only feature files containing the required tags shall be generated. -->
         <!-- Excluded tags (~@notMe) are ignored. -->
         <filterFeaturesByTags>false</filterFeaturesByTags>
      </configuration>
    </execution>
  </executions>
</plugin>
```

Where glue is a comma separated list of package names to use for the Cucumber Glue.

The plugin will search `featuresDirectory` for `*.feature` files and generate a JUnit test for each one.

The Java source is generated in `outputDirectory`, and will have the pattern `ParallelXXIT.java`, where `XX` is a one up counter.

Each JUnit test is configured to output the results to a separate output file under `target/cucumber-parallel`

FAQ
===
Q. Why isn't there much activity on this project
A. The plugin is considered feature complete.  If you feel there is something missing, raise an issue.

Changelog
=========
1.0.0 Implemented issue#XX - Added support for filtering generated files by tag.

Contributing
============

To contribute:

* Create an integration test to demonstrate the behaviour under `src/it/`.  For example, to add support for multiple output formats:
    * Create src/it/multiple-format
    * copy the contents of the src/it/simple-it directory and update the pom/src as appropriate to demonstrate the configuration.  Update the verify.groovy to implement the test for your feature.
    * Run `mvn clean install -Prun-its` to run the integration tests.
* Implement the feature
* When all tests are passing, submit a pull request.

Once the pull request has been merged, a new release will be performed as soon as practicable.
