==============================================================================
OSLAND - BANKER'S ALGORITHM (ACTIVITY 1)
==============================================================================

The C program for Activity 1 is bankersalgo.c. Complete TODO 1 to
TODO 6. Full build, certificate and run instructions are in the
comment block at the top of that file. Quick start:

    gcc -std=c11 -Wall -Wextra bankersalgo.c -o bankersalgo -lssl -lcrypto

    openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem \
        -days 365 -nodes -subj "/CN=localhost" \
        -addext "subjectAltName=DNS:localhost"

    ./bankersalgo                                          # local menu
    ./bankersalgo server 8443 cert.pem key.pem             # TLS server
    ./bankersalgo client localhost 8443 cert.pem student01 1 1 0 2

Set OSLAND_TOKEN (at least 16 characters) and OSLAND_CLIENT_ID on
the server, and the same OSLAND_TOKEN on the client (see the file
header for examples).

Everything runs inside the supplied devcontainer; no extra packages
are needed for Activity 1. Start in local mode, then try the TLS
server and client.

==============================================================================
SUMMER BALL E-TICKET SYSTEM
==============================================================================

OVERVIEW
------------------------------------------------------------------------------
This exercise implements a secure e-ticket system with four components:

    Box office   issues signed tickets and delivers them over TLS
    Wallet       stores a ticket and presents it as a QR image
    Scanner      verifies a QR image at a gate
    Hub          synchronises entry lists between gates

The system addresses three threats:

    Gate 1   forged or modified tickets   Ed25519 digital signatures
    Gate 2   copied QR codes              time-based one-time codes (RFC 6238)
    Gate 3   repeated entry               entry lists synchronised with HMAC

summer_ball_gate_TODO.py contains the full program with the tasks to complete
(TODO 1.1 to TODO 3.4).


REQUIREMENTS
------------------------------------------------------------------------------
Python 3.8 or later, and the following packages:

    pip install cryptography "qrcode[pil]" opencv-python

On Windows, use "py" in place of "python" if required.


CONFIGURATION
------------------------------------------------------------------------------
The student ID is set at the top of the file before the first run:

    STUDENT_ID = "00000000"

The student ID determines the event name, the code interval (10-30 seconds),
the number of code digits (6-8) and the network ports. These values and a
fingerprint of the ID are displayed at the start of every run.


WORKFLOW
------------------------------------------------------------------------------
1. Each task is labelled in the file, for example "TODO 1.1".
2. Each task is divided into numbered steps. Every  ...  marked # TODO is
   replaced with code; the comment beside it describes the expected code.
   All other code is provided and remains unchanged.
3. Each part is tested with its own command:

     Part   Tasks        Command                                Tests
     1      1.1 - 1.3    python summer_ball_gate_TODO.py 1         4
     2      2.1 - 2.6    python summer_ball_gate_TODO.py 2        10
     3      3.1 - 3.4    python summer_ball_gate_TODO.py          16

4. The work is complete when the summary reports 16 of 16 tests passed.


TASKS
------------------------------------------------------------------------------
Part 1 - Gate 1: forged tickets
  1.1  make_ticket        Encode the ticket details and sign them with the
                          private key.
  1.2  check_ticket       Verify the signature with the public key, and reject
                          tickets issued for a different event.
  1.3  edit_name          Simulate tampering: change the holder name while
                          keeping the original signature.

Part 2 - Gate 2: copied QR codes
  2.1  totp               Implement RFC 6238; verified against the RFC test
                          vector.
  2.2  save_qr, read_qr   Write and read QR images.
  2.3  TLS contexts       Configure server and client TLS (version 1.2 or
                          later); the client trusts only box.crt.
  2.4  box_office         Accept each order code once; reject used and unknown
                          codes.
  2.5  Wallet             Download the ticket over TLS and generate the QR
                          image.
  2.6  Scanner            Read the QR image, verify the ticket (Gate 1) and
                          check that the code is current (Gate 2).

Part 3 - Gate 3: repeated entry
  3.1  Scanner.scan       Reject a ticket that has already been admitted.
  3.2  make_mac           Compute an HMAC-SHA256 over a message.
  3.3  hub                Verify the HMAC, record entries and detect
                          duplicate entries.
  3.4  Scanner.sync       Send this gate's entries with an HMAC and merge the
                          entries of other gates.


TEST OUTPUT
------------------------------------------------------------------------------
Each test reports PASS or FAIL and names the functions it checks. For a
failing test, the program reports the first incomplete task it depends on,
with the line numbers still to be completed:

    TEST 1: A valid ticket is accepted   [make_ticket, check_ticket]
       ->  TODO 1.1 is incomplete:
          line 82:  data = ...        # TODO 1.1: use the helper to_bytes()

    TEST 5: totp() matches the RFC 6238 test vector   [totp]
       ->  TODO 2.1: implemented, but the result is incorrect.
           The [BOX OFFICE] [WALLET] [SCANNER] [HUB] output of this test
           shows each step.

A failing test does not stop the program. The summary lists the incomplete
tasks and the next task in order:

Incomplete tasks: eg: TODO 2.6, TODO 3.1, TODO 3.2, TODO 3.3, TODO 3.4 
Next task: TODO 2.6

==============================================================================
