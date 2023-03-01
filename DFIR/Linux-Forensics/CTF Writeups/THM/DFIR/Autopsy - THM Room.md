# Autopsy - THM Room

### Workflow Overflow

Before analysing data, a basic workflow is required. This looks like:
* Create a case
* Select the data you want to analyse
* Configure the ingest modules to extract specific artifacts from the data source
* Review the extracted artifacts
* Create your report

Autopsy files have a ```.aut``` file extension. 

### Data Sources

Autopsy can ingest data from a number of different sources. Typically, you would provide a disk image as a data source. Autopsy can also take in files originating on the Local Disk, Logical Files, Unallocated Space Image File, Autopsy Logical Imager Results, XRY Text Export, and VM files.

Support disk formats include:
* Raw Single (For example: *.img, *.dd, *.raw, *.bin)
* Raw Split (For example: *.001, *.002, *.aa, *.ab, etc)
* EnCase (For example: *.e01, *.e02, etc)
* Virtual Machines (For example: *.vmdk, *.vhd)

### Ingest Modules

Ingest modules are designed to analyse and retrieve specific information. They are essentially plugins, which we can choose to use based on what we need from our data.

You can deselect all the ingest modules and Autopsy will still process the data source and update the local database. Once the data has loaded you will need to right-click on the data source and click **Run Ingest Modules**.

### The User Interface

The UI is made up of 5 primary areas:
* Tree Viewer (Left pane)
* Result Viewer (Top right pane)
* Keyword Search (Upper Top Right)
* Contents Viewer (Bottom right pane)
* Status Area (Lower Bottom right)

##### Tree Viewer
Where you can browse all the files in the case and saved results from automated analysis.
##### Result Viewer
When an item is selected in the Tree Viewer, additional information about the item is displayed in the result viewer.
##### Contents Viewer
Provides information on items selected from the Table tab in the Result Viewer.
##### Keyword Search
An analyst can perform AD-HOC keyword searches.
##### Status Area
Provides the progress status of ingest modules.

### Data Analysis

Case Scenario: An employee was suspected of leaking company data. A disk image was retrieved from the machine. You were assigned to perform the initial analysis. Further action will be determined based on the initial findings. 

1. Windows 7 Ultimate Service Pack 1
2. 40.8% (Click on the Disk -> Listing -> Summary)
3. March 25 2015
4. Eraser (Keyword Search for 6.2.0.2962)
5. 10.11.11.128 (Found by searching for keyword SECRET, right-click one of the files and click View Source File In Timeline, click File Modified. Note the IP address present.)
6. Tomorrow...Everything will be OK...(Found at C:\Users\informant\AppData\Roaming\Microsoft\StickyNotes)