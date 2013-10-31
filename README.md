told-perl-client
==========
[![Build Status](https://travis-ci.org/petershaw/told-perl-client.png)](https://travis-ci.org/petershaw/told-perl-client)
---

 A client to log messages into a told log recorder.
 See <https://github.com/petershaw/told-LogRecorder> to lern more about told.

Description
----------
Sends a message to the server. Noting more, nothing less.

Installation
---------
Get the source from CPAN and run

```bash
perl Makefile.PL
make
make test
make install
```

or use CPAN

```bash
perl -MCPAN -e 'install Told::Client
```

Usage
---------
###Configuration###
First of all you have to initiate the Client with a few minimal configurations. 
It is up to you how you set them. It is possible to pass an config array to the init 
method of the client, or to initiate the client blank and set the config-params later on. 

#### Example ####

```perl
my $told = Told::Client->new({
	"host"	=> 'http://told.my-domain.com'
});
$told->tell('This message should be logged.');
```

or

```perl
my $told = Told::Client->new();
$told->setHost('http://told.my-domain.com');
$told->tell('This message should be logged.');
```


The 'host' parameter is **mandatory**.!
Optional parameters are: type, tags, defaulttage and debug.

| tag | Description |
| ------	| ------	|  
|type | Describes the type of this log message. Choose the type wisely, because you will group by type at the administration frontend. |  
|tags |   These tags will be send ALWAYS right next to the tags that are send by the call it self. It is a good decision to add your application-id and maybe customer-id as tags. |  
|defaultags | To set some default tags that will be overwritten by the call . If no tags are given, than the defaulttags take place. |  

Again, it is possible to set the global type and tags in the constructor, or via setters 
later on. 

### Send ###

After initialisation the client is ready to use.
To send a message with the default tags and type, just call

```perl
$told->tell("This is my little test message");
```

Each call can have special types and tags. For example: to send a message with a type of 'Testing' and the Tag 'Honigkuchen' call

```perl
$told->tell("This is my little test message", "Testing", "Honigkuchen");
```

Or to send multiple tags, like Honigkuchen and Zuckerschlecken it is possible to pass a array:

```perl
$told->tell("This is my little test message", "Testing", ["Honigkuchen", "Zuckerschlecken"]);
```

It is also possible to send the information in one single hash:

```perl
$told->tell({
	'message' => "This is my little test message"
	, 'type' => "Testing"
	, 'tags' => ["Honigkuchen", "Zuckerschlecken"]
});
```

The above example is valid for all the three transportation types. 
Set the connection type with _setCOnnectionType()_ to GET, POST or UDP.

Complex structured messages can not be send with GET. If you try to choose GET and send a 
komplex data, than this module choose POST instead. Turn the debug mode on to get verbose 
output about the internal correction.

```perl
$told->setConnectionType("POST");
$told->setDebug(1);
```

A Complex example:

```perl
my $told = Told::Client->new({
	'host' => 'http://told.my-domain.com'
	, 'type' => 'Demo'
	, 'defaulttags' => ['perl', 'told-client', 'manual']
	, 'tags' => 'notag'
});
$told->setDebug(1);
$told->tell({
	'message' => {
		'said' => "This is my little test message"
		, 'thrown' => 'Exception'
	}
	, 'type' => "Testing"
	, 'tags' => ["Honigkuchen", "Zuckerschlecken"]
});
```

 Protokoll
----------

As default, this client use http POST over tcp ip. GET is also available and UDP is in planing.



