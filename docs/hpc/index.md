# High Performance Computing


The [Research Computing Service (RCS)](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/) at Imperial provides access to [several High Performance Computing (HPC) systems](./cluster-specification.md) that support research across the university. These systems are designed to handle a wide variety of computational workloads, including:

* Highly parallel processing
* High-throughput computing
* Data-intensive tasks
* GPU-accelerated workloads (including AI applications)

This documentation offers guidance on how to effectively use these HPC resources. 

## What is High Performance Computing?

High performance computing (HPC) can refer to a variety of things, from processing large amounts of data very quickly (high-throughput processing, often single-core processing) to running complex code managing so much data that multiple computers coordinating simultaenously is required (multi-core processing).
To illustrate the difference between these two types of computing, concerete examples may be helpful.

An example of single-node process could be converting a coloured image to grey-scale as each pixel needs to have its RGB value modified one by one. Each individual operation is simple, but the total sum of all these small operations can be ginourmonus. While most modern laptops can easily compute the transformation of the pixels in an image within this example, there are many cases where the scale of the data is so large that a designated service is required. Within research environments, this can look like XXX.

In contrast, an example of multi-node processing is weather simulations. XXX

To learn more in-depth knowledge on the topic, check out the [official web Imperial page on the topic](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/hpc/) that can be accessed from the [main RCS website](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/).

## What HPC services are provided by the RCS?

Imperial offers a variety of HPC facilities, covering AI workloads, high throughput use-cases, and having a capability machine available for more advanced users. Of course, there is also a facility in place for new users to safely explore HPC for the first time.

To learn a bit more about the specific systems and what hardware they run on, check out the [HPC Clusters Specification](../cluster-specification.md) documentation.

## Registering for access

You must have a full Imperial account and be registered with the HPC service before you can use it. Please visit the [Get Access](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) page on the main RCS web pages for information on how to register to use the HPC service. You will also need some experience with using the Linux Operating system and shell scripting before you use the HPC service; please see the [Using Linux](../support/using-linux/index.md) page for advice on where you can gain experience, including details on an appropriate course provided by the [Graduate School](https://www.imperial.ac.uk/students/academic-support/graduate-school/professional-development/doctoral-students/research-computing-data-science/courses/) at Imperial.

We recommend all users (regardless of your prior experience with HPC) the review the [Getting Started](./getting-started/index.md) page before trying to use the facility.

## Citing RCS Facilities in Research Papers

Please acknowledge our contributions by citing us in your research. See [here](../../support/citing-rcs.md) for our suggested template.