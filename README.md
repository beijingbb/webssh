WebSSH2
GitHub version

Buy Me A Coffee

Web SSH Client using ssh2, socket.io, xterm.js, and express

A bare bones example of an HTML5 web-based terminal emulator and SSH client. We use SSH2 as a client on a host to proxy a Websocket / Socket.io connection to a SSH2 server.

WebSSH2 v0.2.0 demo

Requirements
Node v6.x or above. If using <v6.x you should be able to run by replacing the "read-config" package to @1 like this (after a clone):

npm install --save read-config@1

Just keep in mind that there is no intention to ensure compatability with Node < v6.x

Instructions
To install:

Clone to a location somewhere and npm install --production. If you want to develop and rebuild javascript and other files utilize npm install instead.

If desired, edit config.json to change the listener to your liking. There are also some default options which may be definied for a few of the variables.

Run npm start

Fire up a browser, navigate to IP/port of your choice and specify a host (https isn't used here because it's assumed it will be off-loaded to some sort of proxy):

http://localhost:2222/ssh/host/127.0.0.1

You will be prompted for credentials to use on the SSH server via HTTP Basic authentcaiton. This is to permit usage with some SSO systems that can replay credentials over HTTP basic.

Docker Instructions
Modify config.json

{
  "listen": {
    "ip": "0.0.0.0",
    "port": 2222
  }
}
Build and run

docker build -t webssh2 .
docker run --name webssh2 -d -p 2222:2222 webssh2
Options
GET request vars
port= - integer - port of SSH server (defaults to 22)

header= - string - optional header to display on page

headerBackground= - string - optional background color of header to display on page

readyTimeout= - integer - How long (in milliseconds) to wait for the SSH handshake to complete. Default: 20000. Enforced Values: Min: 1, Max: 300000

cursorBlink - boolean - Cursor blinks (true), does not (false) Default: true.

scrollback - integer - Lines in the scrollback buffer. Default: 10000. Enforced Values: Min: 1, Max: 200000

tabStopWidth - integer - Tab stops at n characters Default: 8. Enforced Values: Min: 1, Max: 100

bellStyle - string - Style of terminal bell: ("sound"|"none"). Default: "sound". Enforced Values: "sound", "none"

Headers
allowreplay - boolean - Allow use of password replay feature, example allowreplay: true

mrhsession - string - Can be used to pass APM session for event correlation mrhsession: abc123

Config File Options
config.json contains several options which may be specified to customize to your needs, vs editing the javascript directly. This is JSON format so mind your spacing, brackets, etc...

listen.ip - string - IP address node should listen on for client connections, defaults to 127.0.0.1

listen.port - integer - Port node should listen on for client connections, defaults to 2222

user.name - string - Specify user name to authenticate with. In normal cases this should be left to the default null setting.

user.password - string - Specify password to authenticate with. In normal cases this should be left to the default null setting.

ssh.host - string - Specify host to connect to. May be either hostname or IP address. Defaults to null.

ssh.port - integer - Specify SSH port to connect to, defaults to 22

ssh.term - string - Specify terminal emulation to use, defaults to xterm-color

ssh.readyTimeout - integer - How long (in milliseconds) to wait for the SSH handshake to complete. Default: 20000.

ssh.keepaliveInterval - integer - How often (in milliseconds) to send SSH-level keepalive packets to the server (in a similar way as OpenSSH's ServerAliveInterval config option). Set to 0 to disable. Default: 120000.

ssh.keepaliveCountMax - integer - How many consecutive, unanswered SSH-level keepalive packets that can be sent to the server before disconnection (similar to OpenSSH's ServerAliveCountMax config option). Default: 10.

terminal.cursorBlink - boolean - Cursor blinks (true), does not (false) Default: true.

terminal.scrollback - integer - Lines in the scrollback buffer. Default: 10000.

terminal.tabStopWidth - integer - Tab stops at n characters Default: 8.

terminal.bellStyle - string - Style of terminal bell: (sound|none). Default: "sound".

header.text - string - Specify header text, defaults to My Header but may also be set to null. When set to null no header bar will be displayed on the client.

header.background - string - Header background, defaults to green.

session.name - string - Name of session ID cookie. it's not a horrible idea to make this something unique.

session.secret - string - Secret key for cookie encryption. You should change this in production.

options.challengeButton - boolean - Challenge button. This option, which is still under development, allows the user to resend the password to the server (in cases of step-up authentication for things like sudo or a router enable command.

options.allowreauth - boolean - Reauth button. This option creates an option to provide a button to create a new session with new credentials. See issue 51 and pull 85 for more detail.

algorithms - object - This option allows you to explicitly override the default transport layer algorithms used for the connection. Each value must be an array of valid algorithms for that category. The order of the algorithms in the arrays are important, with the most favorable being first. Valid keys:

kex - array - Key exchange algorithms.

Default values:

ecdh-sha2-nistp256
ecdh-sha2-nistp384
ecdh-sha2-nistp521
diffie-hellman-group-exchange-sha256
diffie-hellman-group14-sha1
Supported values:

ecdh-sha2-nistp256
ecdh-sha2-nistp384
ecdh-sha2-nistp521
diffie-hellman-group-exchange-sha256
diffie-hellman-group14-sha1
diffie-hellman-group-exchange-sha1
diffie-hellman-group1-sha1
cipher - array - Ciphers.

Default values:

aes128-ctr
aes192-ctr
aes256-ctr
aes128-gcm
aes128-gcm@openssh.com
aes256-gcm
aes256-gcm@openssh.com
aes256-cbc legacy cipher for backward compatibility, should removed 👍
Supported values:

aes128-ctr
aes192-ctr
aes256-ctr
aes128-gcm
aes128-gcm@openssh.com
aes256-gcm
aes256-gcm@openssh.com
aes256-cbc
aes192-cbc
aes128-cbc
blowfish-cbc
3des-cbc
arcfour256
arcfour128
cast128-cbc
arcfour
hmac - array - (H)MAC algorithms.

Default values:

hmac-sha2-256
hmac-sha2-512
hmac-sha1 legacy hmac for backward compatibility, should removed 👍
Supported values:

hmac-sha2-256
hmac-sha2-512
hmac-sha1
hmac-md5
hmac-sha2-256-96
hmac-sha2-512-96
hmac-ripemd160
hmac-sha1-96
hmac-md5-96
compress - array - Compression algorithms.

Default values:

none
zlib@openssh.com
zlib
Supported values:

none
zlib@openssh.com
zlib
serverlog.client - boolean - Enables client command logging on server log (console.log). Very simple at this point, buffers data from client until it receives a line-feed then dumps buffer to console.log with session information for tracking. Will capture anything send from client, including passwords, so use for testing only... Default: false. Example:

serverlog.client: GcZDThwA4UahDiKO2gkMYd7YPIfVAEFW/mnf0NUugLMFRHhsWAAAA host: 192.168.99.80 command: ls -lat
serverlog.server - boolean - not implemented, default: false.

accesslog - boolean - http style access logging to console.log, default: false

Experimental client-side logging
Clicking Start logging on the status bar will log all data to the client. A Download log option will appear after starting the logging. You may download at any time to the client. You may stop logging at any time my pressing the Logging - STOP LOG. Note that clicking the Start logging option again will cause the current log to be overwritten, so be sure to download first.

Example:
http://localhost:2222/ssh/host/192.168.1.1?port=2244&header=My%20Header&color=red

Tips
If you want to add custom JavaScript to the browser client you can either modify ./src/client.html and add a <script> element, modify ./src/index.js directly, or check out webpack.*.js and add your custom javascript file to a task there (best option).
