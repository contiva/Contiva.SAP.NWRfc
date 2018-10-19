# Contiva.SAP.NWRfc - .NET SAP RFC API based on SAP Netweaver RFC SDK
## Description

This library provides a alternative SAP .NET Connector based on the _SAP NetWeaver RFC Library_,


## Platforms & Prerequisites

The library requires .NET Framework 4.7.1. 
Even if the library itself is compatible with .NET Standard 2.0, only Windows is supported as runtime environment.
The background is that the native library is implemented with Managed C++/CLI. 

On Windows platforms the Microsoft Visual C++ 2005 Service Pack 1 Redistributable Package (KB973544), or [newer](https://www.microsoft.com/en-us/download/details.aspx?id=48145), must be installed, per [SAP Note 1375494 - SAP system does not start after applying SAP kernel patch](https://launchpad.support.sap.com/#/notes/1375494).

To start using _pyrfc_ you need to obtain _SAP NW RFC Library_ from _SAP Service Marketplace_,
following [these instructions](http://sap.github.io/PyRFC/install.html#install-c-connector). The /dist/ folder of this repository contains egg files (.egg) and wheel files (.whl). Either use _easy_install_ to install the appropriate egg file for your system or use _pip_ to install _pyrfc_ from an appropriate wheel file.

A prerequisite to download is having a **customer or partner account** on _SAP Service Marketplace_ and if you
are SAP employee please check [SAP Note 1037575 - Software download authorizations for SAP employees](https://launchpad.support.sap.com/#/notes/1037575).

_SAP NW RFC Library_ is fully backwards compatible, supporting all NetWeaver systems, from today, down to release R/3 4.0.
You can therefore always use the newest version released on Service Marketplace and connect to older systems as well.

## Usage examples

In order to call remote enabled ABAP function module (ABAP RFM), first a connection must be opened.

```python
>>> from pyrfc import Connection
>>> conn = Connection(ashost='10.0.0.1', sysnr='00', client='100', user='me', passwd='secret')
```

Using an open connection, we can invoke remote function calls (RFC).

```python
>>> result = conn.call('STFC_CONNECTION', REQUTEXT=u'Hello SAP!')
>>> print result
{u'ECHOTEXT': u'Hello SAP!',
 u'RESPTEXT': u'SAP R/3 Rel. 702   Sysid: ABC   Date: 20121001   Time: 134524   Logon_Data: 100/ME/E'}
```

Finally, the connection is closed automatically when the instance is deleted by the garbage collector. As this may take some time, we may either call the close() method explicitly or use the connection as a context manager:

```python
>>> with Connection(user='me', ...) as conn:
        conn.call(...)
    # connection automatically closed here
```

Alternatively, connection parameters can be provided as a dictionary,
like in a next example, showing the connection via saprouter.

```python
>>> def get_connection(connmeta):
...     """Get connection"""
...     print 'Connecting ...', connmeta['ashost']
...     return Connection(**connmeta)

>>> from pyrfc import Connection

>>> TEST = {
...    'user'      : 'me',
...    'passwd'    : 'secret',
...    'ashost'    : '10.0.0.1',
...    'saprouter' : '/H/111.22.33.44/S/3299/W/e5ngxs/H/555.66.777.888/H/',
...    'sysnr'     : '00',
...    'client'    : '100',
...    'trace' : '3', #optional, in case you want to trace
...    'lang'      : 'EN'
... }

>>> conn = get_connection(TEST)
Connecting ... 10.0.0.1

>>>conn.alive
True
```

## Installation & Documentation

For further details on connection parameters, _pyrfc_ installation and usage,
please refer to [_pyrfc_ documentation](http://sap.github.io/PyRFC),
complementing _SAP NW RFC Library_ [programming guide and documentation](http://service.sap.com/rfc-library)
provided on SAP Service Marketplace.
