# Google Cloud Dataflow Examples

[Google Cloud Dataflow](https://cloud.google.com/dataflow/) provides a simple,
powerful programming model for building both batch and streaming parallel data
processing pipelines. This repository hosts example pipelines that use the [Cloud
Dataflow SDK](https://cloud.google.com/dataflow/model/programming-model) and demonstrate the basic functionality of Google Cloud Dataflow. 

## Word Count

A good starting point for new users is our set of
word count ([java](java/examples/src/main/java/com/google/cloud/dataflow/examples), [python](python/dataflow_examples)) examples, which compute word frequencies.  This series of four successively more detailed pipelines is described in detail in the accompanying [walkthrough](https://beam.apache.org/get-started/wordcount-example/).

1. `MinimalWordCount`([java](java/examples/src/main/java/com/google/cloud/dataflow/examples/MinimalWordCount.java), [python](python/dataflow_examples/wordcount_minimal.py)) is the simplest word count pipeline and introduces basic concepts like [Pipelines](https://beam.apache.org/documentation/programming-guide/#pipeline),
[PCollections](https://beam.apache.org/documentation/programming-guide/#pcollection),
[ParDo](https://beam.apache.org/documentation/programming-guide/#transforms-pardo),
and [reading and writing data](https://beam.apache.org/documentation/programming-guide/#io) from external storage.

1. `WordCount`([java](java/examples/src/main/java/com/google/cloud/dataflow/examples/WordCount.java), [python](python/dataflow_examples/wordcount.py)) introduces Dataflow best practices like [PipelineOptions](https://beam.apache.org/documentation/programming-guide/#options) and custom [PTransforms](https://beam.apache.org/documentation/programming-guide/#transforms-composite).

1. `DebuggingWordCount`([java](java/examples/src/main/java/com/google/cloud/dataflow/examples/DebuggingWordCount.java), [python](python/dataflow_examples/wordcount_debugging.py))
shows how to view live metrics in the [Dataflow Monitoring Interface](https://cloud.google.com/dataflow/pipelines/dataflow-monitoring-intf), get the most out of
[Cloud Logging](https://cloud.google.com/dataflow/pipelines/logging) integration, and start writing
[good tests](https://beam.apache.org/documentation/pipelines/test-your-pipeline/).

1. `WindowedWordCount`([java](java/examples/src/main/java/com/google/cloud/dataflow/examples/WindowedWordCount.java)) shows how to run the same pipeline over either unbounded PCollections in streaming mode or bounded PCollections in batch mode.

## Building and Running

### Java

The examples in the example module can be built and executed from the `java/examples` directory by running:

    mvn compile exec:java \
    -Dexec.mainClass=<MAIN CLASS> \
    -Dexec.args="<EXAMPLE-SPECIFIC ARGUMENTS>"

This will use the latest release of the Cloud Dataflow SDK for Java pulled from the
[Maven Central Repository](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22com.google.cloud.dataflow%22).

For example, you can execute the `WordCount` pipeline on your local machine as follows:

    mvn compile exec:java \
    -Dexec.mainClass=com.google.cloud.dataflow.examples.WordCount \
    -Dexec.args="--inputFile=<LOCAL INPUT FILE> --output=<LOCAL OUTPUT FILE>"

Once you have followed the general Cloud Dataflow
[Getting Started](https://cloud.google.com/dataflow/getting-started) instructions, you can execute
the same pipeline on fully managed resources in Google Cloud Platform:

    mvn compile exec:java \
    -Dexec.mainClass=com.google.cloud.dataflow.examples.WordCount \
    -Dexec.args="--project=<YOUR CLOUD PLATFORM PROJECT ID> \
    --stagingLocation=<YOUR CLOUD STORAGE LOCATION> \
    --runner=BlockingDataflowPipelineRunner"

Make sure to use your project id, not the project number or the descriptive name.
The Cloud Storage location should be entered in the form of
`gs://bucket/path/to/staging/directory`. 

Alternatively, you may choose to bundle all dependencies into a single JAR and
execute it outside of the Maven environment. For example, you can execute the
following commands to create the
bundled JAR of the examples and execute it both locally and in Cloud
Platform:

    mvn package

    java -cp target/google-cloud-dataflow-java-examples-all-bundled-manual_build.jar \
    com.google.cloud.dataflow.examples.WordCount \
    --inputFile=<INPUT FILE PATTERN> --output=<OUTPUT FILE>

    java -cp target/google-cloud-dataflow-java-examples-all-bundled-manual_build.jar \
    com.google.cloud.dataflow.examples.WordCount \
    --project=<YOUR CLOUD PLATFORM PROJECT ID> \
    --stagingLocation=<YOUR CLOUD STORAGE LOCATION> \
    --runner=BlockingDataflowPipelineRunner

Other examples can be run similarly by replacing the `WordCount` class path with the example classpath, e.g.
`com.google.cloud.dataflow.examples.cookbook.BigQueryTornadoes`,
and adjusting runtime options under the `Dexec.args` parameter, as specified in
the example itself. 

Note that when running Maven on Microsoft Windows platform, backslashes (`\`)
under the `Dexec.args` parameter should be escaped with another backslash. For
example, input file pattern of `c:\*.txt` should be entered as `c:\\*.txt`.

### Python

The examples in this repository can be built and executed from the `python`
directory by running the following in a virtual environment:

    python setup.py install

This will use the latest release of the Cloud Dataflow SDK for Python pulled from the
[PyPI](https://pypi.python.org/pypi/google-cloud-dataflow).

For example, you can execute the `WordCount` pipeline on your local machine as follows:

    python -m dataflow_examples.wordcount \
    --inputFile=<LOCAL INPUT FILE> --output=<LOCAL OUTPUT FILE>

## Beyond Word Count

After you've finished running your first few word count pipelines, take a look at the `cookbook`([java](java/examples/src/main/java/com/google/cloud/dataflow/examples/cookbook), [python](python/dataflow_examples/cookbook))
directory for some common and useful patterns like joining, filtering, and combining.

The `complete`([java](java/examples/src/main/java/com/google/cloud/dataflow/examples/complete), [python](python/dataflow_examples/complete))
directory contains a few realistic end-to-end pipelines.

## Additional Resources

For more information on Google Cloud Dataflow, see the following resources:
* [Apache Beam](https://beam.apache.org/)
* [Google Cloud Dataflow](https://cloud.google.com/dataflow/)
* [Apache Beam Programming Guide](https://beam.apache.org/documentation/programming-guide/)
* [SDK Javadoc](https://beam.apache.org/documentation/sdks/javadoc/)
* [SDK Pydocs](https://beam.apache.org/documentation/sdks/pydoc/)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/google-cloud-dataflow)
posts tagged with [google-cloud-dataflow](http://stackoverflow.com/questions/tagged/google-cloud-dataflow)
* [DataflowJavaSDK](https://github.com/GoogleCloudPlatform/DataflowJavaSDK) repository, containing the SDK source code
