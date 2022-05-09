{
    "title": "CFTUTIL  utility output messages: CFTunnx",
    "linkTitle": "CFTUTIL utility messages",
    "weight": "300"
}<span id="Messages_from_the_CFTUTIL_utility"></span>You can find the utility output in either the standard output or the redirection file.


| Error | <span id="CFTU00I"></span>CFTU00I: &amp;Cmd _ Correct (&amp;str) |
| --- | --- |
| Information | The &amp;Cmd command is executed correctly. The &amp;str string represents the parameters passed with this command (up to 50 characters). |


 


| Error | <span id="CFTU01E"></span>CFTU01E: storage allocation error |
| --- | --- |
| Explanation | The communication utility could not acquire the memory required to run. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Inform Product Support. |


 


| Error | <span id="CFTU02E"></span>CFTU02E: unable to allocate file &amp;Fname |
| --- | --- |
| Explanation | Problem allocating the file containing the parameter setting commands. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the existence and state of the file, correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU03E"></span>CFTU03E: unable to open file &amp;Fname |
| --- | --- |
| Explanation | Problem opening the file containing the parameter setting commands. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the characteristics of the file to be opened and inform Product Support if necessary. |


 


| Error | <span id="CFTU04E"></span>CFTU04E: error reading input file &amp;Fname |
| --- | --- |
| Explanation | Problem reading the file containing the parameter setting commands. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the characteristics of the file to be opened and inform Product Support if necessary. |


 


| Error | <span id="CFTU05E"></span>CFTU05E: &amp;Cmd Failed _ Unexpected end of file (command) |
| --- | --- |
| Explanation | The end of the file was reached before the end of the command (the last character of the command line may be a comma). |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Review the command syntax, correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU06E"></span>CFTU06E: unexpected end of file before new command |
| --- | --- |
| Explanation | The end of the file was reached before the start of a new command (a comment in the file read may not be closed). |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Add an end of comment marker (*/) to the file and then restart the communication utility. |


 


| Error | <span id="CFTU07E"></span>CFTU07E: &amp;Cmd Failed _ unexpected end of file (comments) |
| --- | --- |
| Explanation | The end of the file was reached before the end of the command (a comment inside the command may not be closed). |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Add an end of comment marker (*/) in the command and then restart the communication utility. |


 


| Error | <span id="CFTU08E"></span>CFTU08E: &amp;Cmd Failed _ missing parenthesis |
| --- | --- |
| Explanation | An opening or closing parenthesis is missing in the command syntax. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Review the command syntax, correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU09E"></span>CFTU09E: &amp;Cmd Failed _ command size too large |
| --- | --- |
| Explanation | The length of the command name is greater than 8. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the command syntax, correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU10E"></span>CFTU10E: &amp;Cmd Failed _ unknown command |
| --- | --- |
| Explanation | The command is unknown. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the [command syntax](../../../c_intro_userinterfaces/command_summary) in the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation. Correct the error and restart the communication utility.<br/> Check the command syntax in the {{< TransferCFT/axwayvariablesComponentShortName  >}} Online documentation. Correct the error and restart the communication utility. |


 


| Error | <span id="CFTU11E"></span>CFTU11E: &amp;Cmd Failed _ keyword &amp;Keyw too large |
| --- | --- |
| Explanation | The length of the &amp;Keyw keyword is greater than 8. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the description of this parameter in the Transfer CFT [parameter index](../../../c_intro_userinterfaces/command_summary/parameter_intro), correct the error and then restart the communication utility.<br/> Check the description of this parameter in the Transfer CFT parameter index, correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU12E"></span>CFTU12E: &amp;Cmd Failed _ illegal separator for keyword &amp;Keyw |
| --- | --- |
| Explanation | A parameter separator in the &amp;Cmd command is invalid. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the [command syntax](../../../c_intro_userinterfaces/command_summary) being sure that the separator is a comma. Correct the error and then restart the communication utility.<br/> Check the command syntax being sure that the separator is a comma. Correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU13E"></span>CFTU13E: &amp;Cmd Failed_missing quote |
| --- | --- |
| Explanation | A closing quote (') is missing in the value assigned to a command parameter. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the invalid parameter; correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU14E"></span>CFTU14E: &amp;Cmd Failed _ too many keywords |
| --- | --- |
| Explanation | There are too many keywords for this command. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the [command syntax](../../../c_intro_userinterfaces/command_summary). Correct the error and then restart the communication utility.<br/> Check the command syntax. Correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU15E"></span>CFTU15E: &amp;Cmd Failed _ keyword &amp;Keyw unknown or duplicate |
| --- | --- |
| Explanation | The &amp;Keyw keyword is unknown or appears twice in the command. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the [command syntax](../../../c_intro_userinterfaces/command_summary). Correct the error and then restart the communication utility.<br/> Check the command syntax. Correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU16E"></span>CFTU16E: &amp;Cmd Failed _ keyword &amp;Keyw missing |
| --- | --- |
| Explanation | The &amp;Keyw keyword, which is mandatory for the command, is missing. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the command syntax, correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU17E"></span>CFTU17E: &amp;Cmd Failed _ keyword &amp;Keyw value out of bounds |
| --- | --- |
| Explanation | The &amp;Keyw keyword of the &amp;Cmd command is numeric and its value has exceeded the authorized limits. |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the possible values for this parameter, correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU18E"></span>CFTU18E: &amp;Cmd Failed _ invalid value for keyword &amp;Keyw |
| --- | --- |
| Explanation | The value of the &amp;Keyw keyword of the &amp;Cmd command is not authorized (numeric value for an alphabetic parameter for example). |
| Consequence | Immediate shutdown of the communication utility. |
| Action | Check the possible values for this [parameter](../../../c_intro_userinterfaces/command_summary/parameter_intro). Correct the error and then restart the communication utility.<br/> Check the possible values for this parameter. Correct the error and then restart the communication utility. |


 


| Error | <span id="CFTU19E"></span>CFTU19E: CFTDEST Failed _ keywords FNAME and &amp;str are mutually exclusive |
| --- | --- |
| Explanation | If &amp;str=PART<br/> The components of a broadcast list can either be described within the command (PART = parameter) or in an external file (FNAME = parameter). These two parameters are mutually exclusive.<br/> If &amp;str=IDF<br/> The IDENTIFIERS authorized in the CFTAUTH command can either be described within the command (IDF = parameter) or in an external file (FNAME = parameter). These two parameters are mutually exclusive. |


 


| Error | <span id="CFTU20I"></span>CFTU20I: &amp;str |
| --- | --- |
| Explanation | CFTUTIL command interpreter information messages.<br/> The &amp;str is self-explanatory and can be of several types:<br/> • Execution header: Information messages indicating the product, release, copyright and execution start date and time<br/> • Execution report: Information messages indicating the number of commands interpreted, the number of errors detected and the execution end date and time<br/> • Dynamic modification of a {{< TransferCFT/axwayvariablesComponentShortName  >}} partner state: Part = &amp;part : &amp;str1<br/> The ACT or INACT command has been executed correctly (&amp;str1 indicates the change of state for the &amp;part partner: initial state -&gt; final state). There are four possible states for a partner:<br/> • ACTIVEBOTH: Partner active in both modes (requester and server)<br/> • NOACTIVE: Partner inactive in both modes<br/> • ACTIVEREQ: Partner active in the requester mode<br/> • ACTIVESERV: Partner active in the server mode |


 


| Error | <span id="CFTU24W"></span>CFTU24W : &amp;Cmd _ Warning (&amp;str) |
| --- | --- |
| Explanation | A CFTUTIL command was correctly interpreted but no information was given; the &amp;str field specifies the reason.<br/> Example: a CFTUTIL LISTCAT command on an empty Transfer CFT catalog causes the following message to be displayed:<br/> <span id="CFTU24W1"></span>CFTU24W : LISTCAT Warning (no record found).  |


 


| Error | <span id="CFTU26E"></span>CFTU26E : &amp;Cmd _ Error (&amp;str) |
| --- | --- |
| Explanation | When executing the command, an error was detected. The &amp;str field can be set to one of the following values. Note that the following list is not exhaustive, as the &amp;str field is relatively self-explanatory:<br/> • Parameter file opening error: Execution of the &amp;Cmd command (LISTPARM for example) resulted in an error when opening the parameter file.<br/> • Partners file opening error: Execution of the &amp;Cmd command (LISTPART for example) resulted in an error when opening the partner file.<br/> • Catalog file opening error: Execution of the &amp;Cmd command (LISTCAT for example) resulted in an error when opening the catalog file.<br/> • Media communication file opening error: Execution of the &amp;Cmd command (LISTCOM for example) resulted in an error when opening the communication file.<br/> • File creation error: An error was detected when creating and formatting the {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafile (CFTUTIL CFTFILE TYPE=CAT/LOG/PARM/PART/ command).<br/> • File delete error: An error was detected when executing a request to delete a {{< TransferCFT/axwayvariablesComponentShortName  >}} internal datafile (CFTUTIL CFTFILE TYPE= ,MODE=DELETE command).<br/> • Output file creating error cs=&amp;scs: Execution of the &amp;Cmd command (COPYFILE for example) resulted in an error when creating the output file.<br/> • Input file opening error cs=&amp;scs: Execution of the &amp;Cmd command (COPYFILE for example) resulted in an error when opening the input file.<br/> • &amp;id: Partner record already exists: Command execution, e.g. writing information to the {{< TransferCFT/axwayvariablesComponentShortName  >}} partner file, resulted in an attempt to add a record that already existed in the file (the &amp;Cmd command requested is designated by &amp;Id).<br/> • &amp;id: Partner record &amp;str error (&amp;str=writing/reading/selecting): Command execution resulted in a write/read/article selection error in the file (the &amp;Cmd command requested is designated by &amp;Id).<br/> • &amp;id: Parameter record already exists: Command execution, writing information to the {{< TransferCFT/axwayvariablesComponentShortName  >}} parameter file, resulted in an attempt to add a record that already existed in the file (&amp;Id being the command identifier).<br/> • &amp;id: Invalid value for NRPART paramete:r Executing the CFTPART command, writing information to the {{< TransferCFT/axwayvariablesComponentShortName  >}} partner file, resulted in an attempt to add a record that already existed in the file (&amp;Id being the CFTPART command identifier). In this case, the NRPART parameter is already assigned to an existing CFTPART command.<br/> • &amp;id: Non bijective table: The conversion table specified in the file referenced by the CFTXLATE DIRECT=BOTH command is not bijective (&amp;Id being the command identifier). Check that the conversion table specified is bijective or create one command for each transfer direction (DIRECT=SEND or DIRECT=RECV).<br/> • No partner found: A partner activation or deactivation command (ACT or INACT) resulted in an error. The identifier indicated (ACT ID=&amp;Part) does not correspond to an existing partner identifier (CFTPART ID=&amp;Part).<br/> • Media communication is full: A transfer command (SEND or RECV) could not be written to the {{< TransferCFT/axwayvariablesComponentShortName  >}} communication file. The maximum number of requests in the communication file that have not yet been processed by the {{< TransferCFT/axwayvariablesComponentShortName  >}} has been reached ([COPYFILE - Copy files off-line](../../../c_intro_userinterfaces/command_summary/parameter_intro/recnb)<br/><br/> • &amp;id: command not authorized: The command specified by the user is not authorized by the {{< TransferCFT/axwayvariablesComponentShortName  >}} security system (&amp;id being the command identifier)<br/> • Habilitation opening error: An error was detected when initializing the {{< TransferCFT/axwayvariablesComponentShortName  >}} security system; a file required by the system could not be opened correctly. Check that the initialization file exists and is valid (contains the operating rules and indirections pointing to the object and action dictionaries) and ensure that the security system dictionary files exist.<br/> • Create channel failed: An error was detected when creating the {{< TransferCFT/axwayvariablesComponentShortName  >}} synchronous communication media. Check that you have enough memory.<br/> • Open channel failed: An error was detected when opening the {{< TransferCFT/axwayvariablesComponentShortName  >}} synchronous communication media. Check that the synchronous communication process is launched.<br/> • Channel read error: An error was detected when reading the {{< TransferCFT/axwayvariablesComponentShortName  >}} synchronous communication media. Check that the synchronous communication process is launched.<br/> • Channel write error: An error was detected when writing in the {{< TransferCFT/axwayvariablesComponentShortName  >}} synchronous communication media. Check that the synchronous communication process is launched.<br/> • Close channel failed: An error was detected when closing the {{< TransferCFT/axwayvariablesComponentShortName  >}} synchronous communication media. Check that the communication media is not already closed. |
| Consequence | The command is ignored. |
| Action | Check the parameter settings, analyze the &amp;scs code if it is set and, if necessary, inform Product Support. |


 


| Error | <span id="CFTU30E"></span>CFTU30E : &amp;Cmd Failed _ Unable to create file &amp;Fname |
| --- | --- |
| Explanation | Execution of the &amp;Cmd command resulted in an error during the file create process.<br/> Example: redirection of CFTUTIL information messages to an invalid report file (CONFIG TYPE=OUTPUT,FNAME=&amp;Fname command). |
| Consequence | The command is ignored. |
| Action | Check the validity of the file name and, if necessary, inform Product Support. |

