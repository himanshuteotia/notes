

Node.js: Buffers, Unicode, UTF-8, and Base64 – Explained Simply

1. What is a Buffer?
	•	A Buffer is a temporary memory space used to store binary data in Node.js.
	•	It’s used when handling:
	•	Files (e.g., PDFs, images)
	•	Network streams (HTTP, TCP)
	•	Low-level binary data

Why Buffer is Needed?
	•	JavaScript was built for browsers and deals mostly with text.
	•	Node.js needs to handle binary data, and strings aren’t suitable for that.
	•	So, Buffer was introduced in Node.js to deal with binary data safely and efficiently.

⸻

2. What is Unicode?
	•	Unicode is a universal system that gives a unique number (code point) to every character in every language.
	•	Example:
	•	'A' → U+0041
	•	'न' → U+0928
	•	'🙂' → U+1F642

Why Unicode?
	•	Solves compatibility issues between languages and systems.
	•	Supports all characters across different languages.

⸻

3. What is UTF-8?
	•	UTF-8 is an encoding system that converts Unicode characters into bytes (1 to 4 bytes per character).
	•	It’s the most popular and web-safe encoding format.

How UTF-8 Works:
	•	'A' → U+0041 → 0x41 (1 byte)
	•	'न' → U+0928 → 0xE0 A4 A8 (3 bytes)
	•	'🙂' → U+1F642 → 0xF0 9F 99 82 (4 bytes)

UTF-8 Encoding & Decoding:
	•	Encode: Buffer.from('नमस्ते', 'utf-8') → converts to bytes
	•	Decode: buffer.toString('utf-8') → converts bytes back to readable string

⸻

4. What is Base64?
	•	Base64 encodes binary data into plain text format using letters, numbers, +, /, and =.
	•	It’s useful for storing or sending files as strings in HTML, JSON, or APIs.

Example:

const base64 = Buffer.from('Hello').toString('base64'); // "SGVsbG8="
const text = Buffer.from(base64, 'base64').toString('utf-8'); // "Hello"


⸻

5. Saving Files in Database
	•	For small files, use Buffer or Base64.
	•	For large files, save the file on disk/cloud (like AWS S3) and store the file path or URL in DB.

⸻

6. Summary Table

Concept	Role
Buffer	Stores raw binary data in Node.js
Unicode	Gives unique number to every character
UTF-8	Converts Unicode to bytes and back
Base64	Converts binary → text for storage/sharing