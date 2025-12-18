Objective

Identify and bypass multiple authentication mechanisms by analyzing client-side logic, HTTP headers, and encoded communications, using CyberChef to reverse custom encoding chains.

This project demonstrates practical skills in:

Web application inspection

Encoding and decoding techniques

Client-side authentication weaknesses

Structured investigation and tooling usage

Tools Used

CyberChef

Browser DevTools (Inspector, Network, Debugger)

Base64 / XOR / ROT / MD5 decoding

CrackStation (hash identification)

Environment Setup

Target web application accessed via:
http://MACHINE_IP:8080

AttackBox browser with Developer Tools enabled

📸 Screenshot: Target application landing page + DevTools open

Investigation Methodology (Used for All Locks)

For every lock, I followed the same workflow:

Identify guard name from page source or login form

Inspect HTTP headers for hidden hints or parameters

Analyze login logic in the Debugger tab (JavaScript)

Extract encoded values from in-app chat

Reverse encoding logic using CyberChef

Authenticate using derived credentials

📸 Screenshot: DevTools tabs (Inspector, Network, Debugger) showing workflow

Lock 1: Outer Gate
Step 1: Guard Identification

Guard name extracted from login form hint

Encoded to Base64 for username

📸 Screenshot: Login form showing guard name
📸 Screenshot: CyberChef Base64 encoding output

Step 2: Magic Question via Headers

Inspected Network tab

Found custom header containing the magic question:

"What is the password for this level?"

Encoded the question using Base64

📸 Screenshot: Network tab showing header
📸 Screenshot: CyberChef encoding the question

Step 3: Chat Extraction

Sent encoded question in chat

Received encoded password from guard

📸 Screenshot: Encoded chat message + response

Step 4: Login Logic Analysis

Debugger tab showed password comparison using Base64 encoding

📸 Screenshot: JavaScript login logic

Step 5: Password Recovery

Decoded guard response from Base64

Resulted in plaintext password

📸 Screenshot: CyberChef decoding result

Step 6: Authentication

Username: Base64-encoded guard name

Password: Recovered plaintext

📸 Screenshot: Successful login confirmation

Lock 2: Outer Wall
Key Difference

Password encoding applied twice

Steps

Reused encoded guard name

Extracted new magic question from headers:
"Did you change the password?"

Retrieved encoded password via chat

Debugger revealed double Base64 encoding

Decoded password twice in CyberChef

📸 Screenshot: Double Base64 logic
📸 Screenshot: CyberChef chained decoding

Lock 3: Guard House
New Elements

XOR encryption introduced

XOR key identified as: cyberchef

📸 Screenshot: Login logic showing XOR + Base64
📸 Screenshot: XOR key discovery

Decoding Process

Login logic flow:

XOR password with key

Encode result to Base64

Reverse process:

From Base64

XOR with same key (cyberchef)

📸 Screenshot: CyberChef recipe (From Base64 → XOR)

Result

Plaintext password recovered

Authentication successful

📸 Screenshot: Successful unlock

Lock 4: Inner Castle
Observation

Login logic applies MD5 hashing

Guard response decoded into a hash value

📸 Screenshot: MD5 logic in Debugger
📸 Screenshot: Hash extracted from chat response

Password Recovery

Submitted hash to CrackStation

Retrieved original plaintext password

📸 Screenshot: CrackStation hash lookup result


