# Vault — File Encryption and Decryption System

A browser-based file encryption and decryption application built with HTML, CSS, JavaScript, and the Web Crypto API. It uses **AES-256-GCM** for authenticated encryption and **PBKDF2 with SHA-256** to derive an encryption key from a password.

## Features

- Encrypt files of any type in the browser
- Decrypt `.enc` files using the original password
- AES-256-GCM authenticated encryption
- PBKDF2-SHA-256 key derivation with 200,000 iterations
- Fresh random 16-byte salt and 12-byte IV for every encryption
- Detects invalid, incomplete, modified, or incorrectly-passworded encrypted files
- Local processing: file contents are not sent to a server
- Responsive interface with drag-and-drop file selection

## Run locally

The Web Crypto API is available in secure contexts. The easiest way to run this project is with a local development server.

### Option 1: Python

```bash
python -m http.server 8000
```

Open <http://localhost:8000> in your browser.

### Option 2: VS Code

Install the **Live Server** extension, right-click `index.html`, and select **Open with Live Server**.

You can also deploy the static files to GitHub Pages, which serves the site over HTTPS.

## How it works

1. The app generates a random salt and initialization vector (IV).
2. PBKDF2 derives a 256-bit AES key from the password and salt.
3. AES-GCM encrypts the selected file and creates an authentication tag.
4. The output file is written in this binary format:

```text
MAGIC (8 bytes) | SALT (16 bytes) | IV (12 bytes) | CIPHERTEXT + AUTH TAG
```

5. During decryption, the app reads the header, derives the same key from the password and stored salt, then asks AES-GCM to authenticate and decrypt the ciphertext.

Encrypted output is named `original-filename.ext.enc`. Decrypting a file ending in `.enc` removes that suffix from the downloaded filename.

## Important notes

- **Remember your password.** There is no password reset or recovery mechanism. If the password is lost, the encrypted file cannot be decrypted by this app.
- Use a unique, strong password. A long passphrase is preferable to a short password.
- This is an educational project, not a professionally audited security product. Do not rely on it as the only protection for irreplaceable or highly sensitive data.
- The app currently reads a complete file into browser memory. Very large files may use substantial memory or fail depending on the device and browser.
- Keep an unencrypted backup until you have successfully tested decryption of your encrypted copy.

## Project structure

```text
file-encryption-decryption-system/
├── index.html
├── README.md
├── .gitignore
└── LICENSE
```

## Future improvements

- Chunked processing for large files
- Batch encryption and decryption
- Optional desktop packaging
- Automated tests for encryption/decryption and malformed inputs
- Accessibility and additional interface refinements

## License

MIT — see [LICENSE](LICENSE).


## Screenshots

### 1. Home Screen
![Home Screen](Screenshots/home%20screen.png)

### 2. File Encryption
![File Encryption](Screenshots/encrypt%20file.png)

### 3. Encryption Successful
![Encryption Successful](Screenshots/encryption%20sucessful.png)

### 4. Decryption Successful
![Decryption Successful](Screenshots/decrypted%20successfully%20.png)
