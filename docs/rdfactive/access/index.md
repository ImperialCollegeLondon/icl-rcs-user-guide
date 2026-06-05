# Accessing your Storage Allocation on the RDF-Active
TODO rename file to `rdfactive-access-index.md`

!!! warning

    You must be connected to the campus network or otherwise using the [University's remote access service](../../remoteaccess.md) in order to connect to the RDF-Active.

The RDF-Active is accessible at the following file path:

*  `\\rdf-active.ic.ac.uk\research\` ([Windows](./windows.md)) or
* `smb://rdf-active.ic.ac.uk/research/` ([macOS](./macos.md) and [Linux](./linux.md))

Your username will be in the form `username@ic.ac.uk` and you will need your standard Imperial password; please use the above links for more details for each operating system.

We recommend you read through the *"Where are my files?"* section below for advice on how to browse the file system or otherwise know how to connect to the specific directory (XXX) of the file system that you need to.

## Where are my files?

The RDF-Active uses a file system layout that corresponds to the organisational structure of Imperial’s faculties and departments:

Faculty/Department/Research Group/Storage Allocation/

* **Faculty** - this is represented as a shortname, you will find a list of these [listed below](#faculty-shortnames).
* **Department** - this will be the full department name in lower case, with spaces replaced with underscores.
* **Research Group** - this will be a shortname for the research group your storage allocation is contained in. It will typically be the username of the research group lead.
* **Storage Allocation** - this will be the shortname created for the storage allocation.

You should be able to tell what your path is by opening your storage allocation when

!!! tip
    The path to your storage allocation is visible on the [ReCAP](../../recap/index.md) portal under the *Allocation Attributes*. In that section, you will find an *Attribute* called *Filesystem location* which will have a path of the form:

    `/rdfactive/icl/faculty/department/research_group/storage_allocation/`

    Removing `/rdfactive/icl/` from the filesystem location will give you the path to your storage allocation. 

### Faculty Shortnames

Faculty Shortname | Faculty
------------------|--------
buss | Business School
facility | Imperial-wide facilities
foe | Faculty of Engineering
fom | Faculty of Medicine
fons | Faculuty of Natural Sciences


