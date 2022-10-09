Transport catalogue
-----------------

A system for storing transport routes and processing requests to it:

- Input data and response in JSON format.
- The output JSON file contains a visualization of the route map(s) in SVG file format.
- Search for the shortest route.
- Serialization of the database and directory settings using Google Protocol Protobuf.
- JSON objects support method chaining during construction, turning errors in the use of JSON format data into compilation errors.

Requirements
-----------

* C++17 and above
* Protobuf 3

Used ideoms, technologies and language elements
--------------------------------------------

- OOP: inheritance, abstract interfaces, final classes
- Unordered map/set
- STL smart pointers
- std::variant and std:optional
- JSON load / output
- SVG image format embedded inside XML output
- Curiously Recurring Template Pattern (CRTP)
- Method chaining
- Facade design pattern
- C++17 with C++20 Ranges emulation
- Directed Weighted Graph data structure for Router module
- Google Protocol Buffers for data serialization
- Static libraries .LIB/.A
- CMake generated project and dependency files

Building a project (for Visual Studio)
-------------------------------------

1. Download the Google Protobuf archive from the [official repository](https://github.com/protocolbuffers/protobuf/releases ) and unzip it on your computer to a folder that does not contain Russian characters and spaces in the name and/or path to it. Protobuf 21.1 2 was used in this project
. Create the `my_build` folder inside the unpacked archive and open it on the command line.
3. Generate a Protobuf project for VS using cmake:
`cmake ../cmake -Dprotobuf_BUILD_TESTS=OFF -DCMAKE_INSTALL_PREFIX=path\proto-21-1-src\my_build -Dprotobuf_MSVC_STATIC_RUNTIME=OFF`
4. To build projects from the command line, create the `Debug` and `Release` folders inside the `my_build` folder and execute commands:
```
cmake --build . --config Debug
cmake --install . --config Debug
cmake --build . --config Release
cmake --install . --config Release
```
5. It is not necessary to execute the `cmake --install` command if you plan to link library files manually.
6. =or= To build projects using the VS environment, open the solution file `my_build\protobuf.sln` in the VS environment, select the required configurations (Debug, Release) and/or target platforms and build the project as usual.
7. Add folders with generated .lib protobuf libraries to additional dependencies of the transport directory project (Additional Include Directories, Additional Dependencies, etc.).

Running the program
------------------

The Transport Directory application is designed to work in 2 modes: database creation mode and database query mode.

To create a transport directory database with its subsequent serialization into a file, you need to run the program with the make_base parameter. The input data comes from stdin, so you can redefine the data source, for example, by specifying the input JSON file from which the information for filling the database will be taken instead of stdin.
Example:
`transport_catalogue.exe make_base <input_data.json`

To process requests to the created database (the database itself is deserialized from a previously created file), you need to run the program with the process_requests parameter, specifying the input JSON file containing the request(s) to the database and the output file that will contain responses to requests, also in JSON format.
Example:
`transport_catalogue.exe process_requests <requests.json >output.txt`


Input data format 
----------------

Input data is received from stdin in JSON format. The top-level structure looks like this:
``
{
"base_requests": [ ... ],
"render_settings": { ... },
"routing_settings": { ... },
"serialization_settings": { ... },
"stat_requests": [ ... ]
}
```
Each element is a dictionary containing the following data:
`base_requests' — description of bus routes and stops.
`stat_requests' — requests to the transport directory.
`render_settings' — map rendering settings in the format .SVG.
`routing_settings` — router settings for finding shortest routes.
`serialization_settings' — data serialization/deserialization settings.
