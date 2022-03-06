# `HTTP`

## `HTTP` vs. `TCP/IP`

`HTTP` is a protocol used mostly for browsing the internet (Edge, Chrome, IE, Firefox, etc). It rides on top of `TCP` which provides a reliable link between two computers (**if packet get lost - it is re-transmitted**). `TCP` itself rides on top of `IP`, which provides **unified addressing** to communicate between computers. `TCP/IP` is a basis for internet and 99% of other networks.


![Internet protocol stack](tcp.gif "Internet protocol stack")

> Basically it means if you are communicating `HTTP`, you are doing it with `TCP/IP` underneath (but I am sure this is not what your professor meant).