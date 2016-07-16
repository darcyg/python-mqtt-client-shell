# Python-based MQTT Client Shell

Written in Python *(works with Python 2 and 3)*, and using the [Eclipse Paho](http://eclipse.org/paho/) MQTT Python client library, this is an interactive MQTT client command shell.

## Purpose

The purpose of this utility is to provide a text console-based, interactive shell for exercising various tasks associated with MQTT client communications, including connecting to an MQTT server, subscribing to topics, and publishing messages.

Other MQTT client tools exist, such as [mqtt-spy](https://github.com/kamilfb/mqtt-spy), but they tend to be GUI-based. This project arose from the need for a quick and easy way to test MQTT communications from the command line, e.g. terminal/SSH session.

## Getting Started

### Dependencies

Other than Python itself, the only other dependency is the [Paho Python](http://www.eclipse.org/paho/clients/python/) client library.

### Installation

* Copy the `mqtt_client_shell.py` file to your desired folder location.

* Install the Paho Python client, via PyPI.
  * Alternatively, if security restrictions prohibit installing Python packages in your environment, then the source (src/paho) can simply be copied into a `paho` subfolder in the same folder where the `mqtt_client_shell.py` file resides.

### Running

```
python mqtt_client_shell.py [optional playback file]
```

## Usage

### Basics

The program implements a set of nested *sub* shells:

1. Upon startup the initial shell is the **Main** (i.e. Client) shell, where changes can be made to MQTT client parameters, e.g. client_id, protocol version, etc.

2. From the Main shell, the *connection* command takes you to the **Connection** shell, which handles the setting of MQTT connection parameters, e.g. host, port, username, etc.

3. From the Connection shell, the *connect* command initiates a connection to the MQTT server, and then takes you to the **Messaging** shell, for performing pub/sub messaging via the active MQTT connection.

#### Help

Enter ```help``` or ```?``` to show a list of commands that are valid at that point.

To get help for a particular command, enter ```help``` (or ```?```) followed by the command, e.g. ```help publish```.

#### Command Completion and History

If your system supports it (Windows doesn't support TAB completion), then, when entering commands, the TAB key will let the shell try to complete the command or propose alternatives.

Additionally, the up/down arrow keys can be used to cycle through the command history.

#### Exit

Typing ```exit``` or ```quit``` (or pressing Ctrl-D, for those systems supporting it) will exit the current shell. If at the Main shell, then this will quit the program.

### Sample Session

Below is a sample session showing the basics: connecting to an MQTT server, subscribing to all topics (#), and publishing a message. To clarify, once the program was started, the following commands, entered in succession, produced the sample session:

logging off  
connection  
connect  
subscribe #  
publish test/a "hi, world"  
exit  
exit  
exit

```
$ python mqtt_client_shell.py

Welcome to the MQTT client shell.
Type help or ? to list commands.
Pressing <Enter> on an empty line will repeat the last command.

Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: on (indent=30), Recording: off, Pacing: 0
> logging off

Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: off, Recording: off, Pacing: 0
> connection

Connection args: host=localhost, port=1883, keepalive=60, bind_address=, username=, password=, will=None
Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: off, Recording: off, Pacing: 0
> connect

***CONNECTED***
Subscriptions:
Connection args: host=localhost, port=1883, keepalive=60, bind_address=, username=, password=, will=None
Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: off, Recording: off, Pacing: 0
> subscribe #
...msg_id=1, result=0 (No error.)

***CONNECTED***
Subscriptions: (topic=#,qos=0)
Connection args: host=localhost, port=1883, keepalive=60, bind_address=, username=, password=, will=None
Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: off, Recording: off, Pacing: 0
> publish test/a "hi, world"
...msg_id=2, result=0 (No error.)
                              on_message(): message received: Topic: test/a, QoS: 0, Payload Length: 9
                                                              Payload (str): b'hi, world'
                                                              Payload (hex): b'68692c20776f726c64'

***CONNECTED***
Subscriptions: (topic=#,qos=0)
Connection args: host=localhost, port=1883, keepalive=60, bind_address=, username=, password=, will=None
Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: off, Recording: off, Pacing: 0
> exit

Connection args: host=localhost, port=1883, keepalive=60, bind_address=, username=, password=, will=None
Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: off, Recording: off, Pacing: 0
> exit

Client args: client_id=paho-9241-mypc, clean_session=True, protocol=4 (MQTTv3.1.1)
Logging: off, Recording: off, Pacing: 0
> exit
$
```

## Commands

### Global Commands

These commands work in any of the (sub) shells.

#### logging *[on | off]*
* Turn on/off the display of MQTT client callback messages.
  * If on/off argument is not specified, then setting is toggled.
  * Note: This doesn't affect the on_message callback, which always logs when a message is received.
* Additionally, the ```logging_indent``` command controls the indentation for the callback messages.
  * This is helpful to distinguish callback messages from the interactive shell input/output.

#### prompt_verbosity *[N | L | M | H]*
* Set the amount of detail displayed in the shell prompt: 'N'one, 'L'ow, 'M'edium, 'H'igh.
  * If argument is not specified, then setting is reset to default ('H'igh).
* A high verbosity will show essentially all current settings.

#### record *file*
* Start recording commands to the given file.
  * If the file exists, then commands will be appended to it.

#### stop_recording
* Stop recording commands, if currently recording commands to a file.

#### playback *file*
* Play back commands from the given file.

#### pacing *delay*
* Set a delay between commands, when playing back commands from a file.
  * A fractional delay value is supported, e.g. 1.5 (seconds).


## Built With

* Python [Cmd](https://docs.python.org/3/library/cmd.html) module
* [Eclipse Paho Python Client for MQTT](http://www.eclipse.org/paho/clients/python/)

## Tested Environments

* Ubuntu 14.04 LTS - Python 2.7 and 3.4
* Ubuntu 16.04 LTS - Python 2.7 and 3.5
* Red Hat Enterprise Linux 7 - Python 2.7
* Raspberry Pi - Raspbian - Python 2.7 and 3.4
* Windows - Python 2.7 and 3.4

## Requests/Issues/Bugs

This project uses [GitHub Issues](https://github.com/bapowell/python-mqtt-client-shell/issues)
to track feature requests, to-dos, bugs, etc.

## Authors

* Brad Powell - [gh](https://github.com/bapowell), [in]( https://www.linkedin.com/in/bradpowell14)

## License

This project is licensed under the Eclipse Public License - see the [LICENSE](LICENSE) file for details.
