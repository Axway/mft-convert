{
    "title": "Client/server  variable additions",
    "linkTitle": "Client and server variable additions",
    "weight": "280"
}To allow recent clients to dialog with older servers, several additions
have been made to the environment for Transfer CFT releases on UNIX systems.
****This topic**** describes the additional
environmental variables for the Client/server model.

These additions are mainly used when creating a:

- *cftinq.cfg*
    configuration file
- CFTDIRINQ environment
    variable

Client/server environmental variables
-------------------------------------

### cftinq.cfg configuration file

The *cftinq.cfg* file is found in the *&lt;installdir&gt;/runtime/data/* subdirectory.
When Transfer CFT is running normally, not in client/server mode, the
appropriate values for your local Transfer CFT are automatically set in
this file.

### CFTDIRINQ environment variable

When Transfer CFT is installed, an environment variable called CFTDIRINQ
is automatically set. By default, not in client/server mode, its content
points to the Transfer CFT *&lt;installdir&gt;/runtime/data/* subdirectory containing the *cftinq.cfg*
file.

### Client/server mode

To enable a client to dialog with a server, the CFTDIRINQ variable must
contain the path pointing to the *cftinq.cfg* file of the remote
server, as provided by the remote site administrator.
