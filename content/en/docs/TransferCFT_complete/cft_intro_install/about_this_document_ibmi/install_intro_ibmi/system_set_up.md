{
    "title": "About the installed environment",
    "linkTitle": "About the installed environment",
    "weight": "170"
}Transfer CFT libraries
----------------------

After complete installation of the Transfer CFT product, the procedure will create the following libraries:

- CFTPGM: This library is generally reserved for Transfer CFT and which contains:
    -   Programs
    -   Examples of source files (CFTSRC file)
    -   Savf File for IFS environment
    -   CFTPROD: This library is generally reserved for customer and which contains:
    -   Source files customized by the client
    -   SAVF files when you apply Service Pack
    -   CFT specific objects and subsystems

Transfer CFT specific objects and subsystems
--------------------------------------------

Transfer CFT IBM i runs on a specific subsystem and use specific objects. These objects are automatically created during the installation, in the production library. These objects are mandatory for Transfer CFT operations, but their name can be customized when executing the INSTALL command (see Install Transfer CFT on the IBM i system). These objects are:

- Job description (\*JOBD)
- Job queue (\*JOBQ)
- Subsystem with a private memory pool of 245 MB
- Standard batch class

For practical reasons, you may consider other configurations:

- If you foresee activity peaks for Transfer CFT and other applications at the same time, consider working in a shared pool.
- If Transfer CFT is being used in time slots corresponding to normal activity levels and when no other applications are running, the basic memory pool is sufficient.
