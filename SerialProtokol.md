# SerialProtokol



All Strings need to End with an '\0'

PirAtE_NO_DATA 0x29

//Value Offset 0x30
PirAtE_MSG_DATAID_OFFSET ((byte)'0')




## Node to Arduino

The Arduino Side is kept small, means the Node side hast to Convert the Data in bytes before sending

|  Symbol  |    Msgtype    |       Style        |                    Content                    |
| :------: | :-----------: | :----------------: | :-------------------------------------------: |
|   0x29   |    No Data    |        0x29        | Informs the Arduino that no Data is available |
| 0x30-... | Data Transfer | [ID][ValueInBytes] |              Transfer of a Value              |




## Arduino to Node

byte PirAtE_SERIAL_START[PirAtE_SERIAL_START_LENGTH] = {0xee, 'P', 'i', 'r', 'A', 't', 'E', '\n'};

PirAtE_DATATYPE_INFO 'P'

PirAtE_TRANSMIT_SEPERATOR '$'

byte PirAtE_MSG_DELIMITER[PirAtE_MSG_DELIMITER_LENGTH] = {0xff, 'P', 'i', 'r', 'A', 't', 'E', '\n'};

### Msg Types 
| Symbol |               Msgtype                |                       Style                       |                   Content                   |
| :----: | :----------------------------------: | :-----------------------------------------------: | :-----------------------------------------: |
|   P    |            Datatype Sizes            |                P[Type][Bytes]\$..                 | All datatypes with Size in Bytes get Listed |
|   T    |     Configuration of one SendMsg     | T[ID]\$[Name]\$[Type]\$[DefaultV]\$[MaxV]\$[MinV] |  Every Definition of a Send Msg gets Send   |
|   t    |   Configuration of one ReceiveMsg    |         t[ID]\$[Name]\$[Type]\$[DefaultV]         | Every Definition of a Receive Msg gets Send |
|   R    |         Request for new Data         |                         R                         |        Request to Node for more Data        |
|   I    |      Data Msg containing an int      |                I[ID][ValueInBytes]                |                 sizeof(int)                 |
|   U    | Data Msg containing an unsigned int  |                U[ID][ValueInBytes]                |            sizeof(unsigned int)             |
|   L    |     Data Msg containing an long      |                L[ID][ValueInBytes]                |                sizeof(long)                 |
|   u    | Data Msg containing an unsigned long |                u[ID][ValueInBytes]                |            sizeof(unsigned long)            |
|   F    |     Data Msg containing an float     |                F[ID][ValueInBytes]                |                sizeof(float)                |
|   D    |    Data Msg containing an double     |                D[ID][ValueInBytes]                |               sizeof(double)                |
|   B    |     Data Msg containing an byte      |                   B[ID][1Byte]                    |                sizeof(byte)                 |
|   W    |     Data Msg containing an word      |                   W[ID][2Bytes]                   |                sizeof(word)                 |
|   b    |     Data Msg containing an bool      |                   b[ID][1Byte]                    |                sizeof(bool)                 |
|   C    |     Data Msg containing an char      |                    C[ID][Char]                    |                sizeof(char)                 |
|   S    |    Data Msg containing an char[]     |                   S[ID][String]                   |                      *                      |

[*1]
```
hallo ich bin code
```

### Datatypes
|   Datatype    | Symbol |    Size Definition    |
| :-----------: | :----: | :-------------------: |
|      int      |   I    |      sizeof(int)      |
| unsigned int  |   U    | sizeof(unsigned int)  |
|     long      |   L    |     sizeof(long)      |
| unsigned long |   u    | sizeof(unsigned long) |
|     float     |   F    |     sizeof(float)     |
|    double     |   D    |    sizeof(double)     |
|     byte      |   B    |     sizeof(byte)      |
|     word      |   W    |     sizeof(word)      |
|     bool      |   b    |     sizeof(bool)      |
|     char      |   C    |     sizeof(char)      |
|    char[]     |   S    |           *           |
*MaxLength depends on buffer size and Overhead
PirAtE_RECEIVE_DATATYPE_STRING_MAXLENGTH = PirAtE_Serial_Buffer_Size - PirAtE_CHARARRAY_END_LENGTH - PirAtE_MSG_DATAID_LENGTH
PirAtE_MSG_DATATYPE_STRING_MAXLENGTH = PirAtE_MSG_DATA_MAXLENGTH - PirAtE_CHARARRAY_END_LENGTH


//Offset of actual Data to DataMsg Start
PirAtE_MSG_DATA_OVERHEAD (PirAtE_MSG_DATAID_LENGTH + PirAtE_MSG_DATATYPE_LENGTH)
//MaxData Length
PirAtE_MSG_DATA_MAXLENGTH (PirAtE_Serial_Buffer_Size - PirAtE_MSG_DATA_OVERHEAD - PirAtE_MSG_DELIMITER_LENGTH)
