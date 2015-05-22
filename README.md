lirc_web
========

``lirc_web`` is a [nodejs](http://nodejs.org) app that creates a web interface & JSON API for the [LIRC](http://lirc.org) project. It uses [lirc_node](https://github.com/alexbain/lirc_node) to handle communication between LIRC and nodejs.

This project allows you to control LIRC from any web browser - phone, tablet, or desktop. The mobile web interface is responsive and optimized for all sized displays. In addition, with the JSON API, you can control LIRC from any web connected device - pebble watch, myo armband, emotiv EEG headset, or beyond.

This is part of the [Open Source Universal Remote](http://opensourceuniversalremote.com) project.


## Configuration

As of v0.0.8, ``lirc_web`` supports customization through a configuration file (``config.json``) in the root of the project. There are currently four configuration options:

1. ``repeaters`` - buttons that repeatedly send their commands while pressed. A common example are the volume buttons on most remote controls. While you hold the volume buttons down, the remote will repeatedly send the volume command to your device.
2. ``macros`` - a collection of commands that should be executed one after another. This allows you to automate actions like "Play Xbox 360" or "Listen to music via AirPlay". Each step in a macro is described in the format ``[ "REMOTE", "COMMAND" ]``, where ``REMOTE`` and ``COMMAND`` are defined by what you have programmed into LIRC. You can add delays between steps of macros in the format of ``[ "delay", 500 ]``. Note that the delay is measured in milliseconds so 1000 milliseconds = 1 second.
3. ``commandLabels`` - a way to rename commands that LIRC understands (``KEY_POWER``, ``KEY_VOLUMEUP``) with labels that humans prefer (``Power``, ``Volume Up``).
4. ``remoteLabels`` - a way to rename the remotes that LIRC understands (``XBOX360``) with labels that humans prefer (``Xbox 360``).


#### Example config.json:


    {
      "repeaters": {
        "SonyTV": {
          "VolumeUp": true,
          "VolumeDown": true
        }
      },
      "macros": {
        "Play Xbox 360": [
          [ "SonyTV", "Power" ],
          [ "delay", 500 ],
          [ "SonyTV", "Xbox360" ],
          [ "Yamaha", "Power" ],
          [ "delay", 250 ],
          [ "Yamaha", "Xbox360" ],
          [ "Xbox360", "Power" ]
        ],
        "Listen to Music": [
          [ "Yamaha", "Power" ],
          [ "delay", 500 ],
          [ "Yamaha", "AirPlay" ]
        ]
      },
      "commandLabels": {
        "Yamaha": {
          "Power": "Power",
          "Xbox360": "Xbox 360",
          "VolumeUp": "Volume Up",
          "VolumeDown": "Volume Down"
        }
      },
      "remoteLabels": {
         "Xbox360": "Xbox 360"
      }
    }


## Using the JSON API

Building an app on top of lirc_web is straight forward with the included JSON based RESTful API.

API endpoints:

* ``GET`` ``/remotes.json`` - Returns all known remotes and commands
* ``GET`` ``/remotes/:remote.json`` - Returns all known commands for remote ``:remote``
* ``GET`` ``/macros.json`` - Returns all known macros
* ``POST`` ``/remotes/:remote/:command`` - Send ``:command`` to ``:remote`` one time
* ``POST`` ``/remotes/:remote/:command/send_start`` - Begin sending ``:command``
* ``POST`` ``/remotes/:remote/:command/send_stop`` - Stop sending ``:command``
* ``POST`` ``/macros/:macro`` - Send all commands for ``:macro`` one time


## Development

Would you like to contribute to and improve ``lirc_web``? Fantastic. To contribute
patches, run tests or benchmarks, install ``lirc_web`` using the instructions above. Once that is complete, you'll need to setup the development environment.

Now, you'll need to setup the development environment. ``lirc_web`` uses the [GruntJS](http://gruntjs.com/) built system to make development easier.

Install GruntJS (build environment):

    npm install -g grunt-cli
    npm install -g grunt-init
    grunt server


**You may need to reload your shell before continuing so the Grunt binares are detected.**

* ``grunt`` will create all of the static assets.
* ``grunt server`` will start a development server (using sample data) and watch all static assets for change

You can run the test suite by running:

```
make test
```


## License

(The MIT License)
