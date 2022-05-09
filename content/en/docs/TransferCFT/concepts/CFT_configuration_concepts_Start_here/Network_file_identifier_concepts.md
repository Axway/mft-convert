{
    "title": "Virtual file association template",
    "linkTitle": "Template to virtual file association CFTIDF",
    "weight": "150"
}****Related
topics****

- Command syntax
    [CFTIDF](../../../c_intro_userinterfaces/command_summary#CFTIDF)
- Object concepts
    [Template to virtual file association](#)

<span id="About_the_CFTIDF_object"></span>

About the CFTIDF object
-----------------------

A CFTIDF
object defines a network identifier for a given partner, and a transfer
direction, when partners cannot agree on common file identifiers.

The CFTIDF object is used to locally set up this correspondence between
the local ****idf**** and the sent or
received ****nidf****.

**For all protocols EXCEPT PeSIT SIT profile**

If partners cannot agree on common file identifiers because of operating
constraints, the **network identifier,**
NIDF, can be used to reconcile, for a given partner and transfer
direction, the local identifier (IDF) with the file identifier supplied
by (or sent to) the partner.

Other ways of establishing this correspondence are indicated in the
section *NIDF/IDF correspondence*. The default value taken by the
NIDF (sent in requester mode) or by the IDF (found using the NIDF received
in server mode) and assigned in the absence of this command, is indicated.

In DELETE mode, only the ID, TYPE, and PART parameters are mandatory.
