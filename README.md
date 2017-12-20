# GBA-Memory-Analysis-Manager
This module will be responsible for communicating with emulators to running memory access scans, as well as managing the output in its own project hierarchy. It will be able to run TAS test cases on multiple emulators running at the same time to analyze memory access

# Input
- A configuration file specifying essentials, such as the project location, the ROM to analyze, etc
- Should be able to take in a list of structures to analyze and forward those requests to one or more VBA-rr emulators running in parallel. 
- The list should be specified in a manner such that its operation may vary. For each structure: 
	- The manager may perform different actions, such as reporting the structure to a specified filename, binding it to that filename.
	- The size of the structure must be specified.
	- Operation mode settings of the structure. It may be seen as a single structure, it may be a memory block with multiple structures to analyze. Different scheduling algorithms can be performed to analyze very large memory blocks.
	-  A list of test cases to be TAS ran to detect game usage of the memory blocks. Those test cases represent actual game control lua scripts. These are executed to actually automate the playing of the game to detect game code accesses to the memory blocks.
- Location to the TAS test cases must be specified. These can be run in parallel across multiple emulators to test multiple structs at once, or to run several test cases on one structure at once.


# Requirements
- Must be operatable interactively, through an API, and via the console.
- Analysis requests can be passed in lists, or an analysis can be requested on-the-fly interactively or through command line.
- Should be able to deduct whether the structure bases passed, and whether the sizes of those structuers passed are accurate. If not, they must inform the user.
- This manager manages its own project structure, where it outputs all of its memory scans in the form of header files. It can merge different structs together, or it can always generate new files, these modes will be configurable and overridable.
- Once a memory block is scanned, All detected structures will be generated and then hashed. If A hash is found to 95% match the current detected structure in memory, the game code that accessed both structures are compared. If that matches 95%, then both outputs are merged into one, and it is noted of in the file. If the 95% tests fail, a new file is created for the newly analyzed struct.
- When a struct is fully analyzed, the following outputs appear in the header file: 
	- The structure typedef
	- A synronizable structure documentation section
	- The memory access output provided by the memory access scanner
	- A list of all function accesses to the structure
- In the case a structure contains another detected structure inside it, it is put on a separate file, and must be included into the correct file. This is managed by keeping a record of all child structures.
- For easy, safe renaming of structures, all automated project management must use an identifier for the structure that is managed automatically by the manager.
- A memory block should be scannable in both series, and parallel. The manager must be able to communicate with the emulators and coordinate how the memory access scanning is performed. The results are then returned to the manager, which merges parallel results into one complete scan.
- The manager may parallelize the running of each test case into an emulator, in which the scanning of one structure occurs at multiple emulators, or it may deligate one structure to each emulator, scanning several structures at once.
