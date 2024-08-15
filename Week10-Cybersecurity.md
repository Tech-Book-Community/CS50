# Week 10 – Cybersecurity

## [Course link]( https://cs50.harvard.edu/x/2024/weeks/10/)</br>
[![week10 - Cybersecurity]( https://img.youtube.com/vi/EKof-cJiTG8/0.jpg)](https://youtu.be/EKof-cJiTG8)

## Week 10 - Cybersecurity
## Purposes
To protect information, property, and more.
For example:
Secure your account, Data, System, Software,…

## Suggestions for creating your password
*	For user:

－>　Avoid easy password like:123456, abc, qweryui, etc,…

－>　Combine characters (including upper and lower case) and symbols

－>　Change your password periodically.

*	For Verifiers:

－>　Verifiers should allow all printed ASCII characters and Unicode symbols up to 64 characters in length. (Enhancement of complexity)

－>　Verifiers should not allow unauthenticated claimants to access password hints. (Only trust authenticated services)

－>　Verifiers should limit the number of failed authentication attempts and lock out potential adversaries.

…

## Password nowadays
*	Password Managers
Automatically generate complicated passwords and remember them for you.
* Two-factor/Multi-factor Authentication
Verify your identity with two devices.
End-to-end Encryption
End-to-end encryption is a way of encrypting and decrypting on the same system without a middleman. (A more practical example is Zoom and Apple messages.)

## Interesting fact
Deletion in the computer didn't remove "all" data. It still preserves remnants in your hard disk.

－> Secure deletion comes into play: It turns those remnant files into zeros and ones.

－> Some remnants remain since your deleted files are inaccessible by the operation system.

## Technique
* Hashing
It uses difficult math to encrypt your information. One-way hashing allows only the hashed password to be stored, so your original password wouldn't be known.

<img src="https://github.com/user-attachments/assets/3ee2899c-2d4b-4a5d-99eb-18958ac77136" width = 50% style ="display: block; margin: auto;"></br>

<img src = "https://github.com/user-attachments/assets/b96e5cce-b49c-4bdd-bb22-a5141a5c5a96" width = 50% style ="display: block; margin: auto;"></br> 

To avoid the same result, add another input to interfere with the result:

<img src = "https://github.com/user-attachments/assets/3da8c629-f808-4692-8bb0-0b64fa6c25b5" width = 50% style ="display: block; margin: auto;"></br>

<img src = "https://github.com/user-attachments/assets/3a419e3a-81b9-4c07-a7af-7b11806e8f70" width = 50% style ="display: block; margin: auto;"></br> 

## Passkeys
Unlike using traditional usernames and passwords to represent a single account, It is a password-less verification.
When you first visit a website, your device will generate both a public key and a private key and send the public key to the website.
Next time when you log in, the website will send a challenge message, your device uses a math algorithm to encrypt your private key, which turns out to generate a signature.
On the website, the web decrypts the signature with the public key you provided last time to prove you are the same user.

<img src = "https://github.com/user-attachments/assets/03f9426a-4ab7-48b2-b27e-127631ea2260" width = 50% style ="display: block; margin: auto;"></br> 

Examples: Microsoft Authenticator App or Windows Hello, faceID, fingerprint (biology identification), FIDO, and more.

## Encryption
Encryption is a way by which data is obscured so that only the sender and intended receiver can be read. Use someone's public key to encrypt a message, and use their private key to decrypt it.
Examples: WhatsApp, i-message, Zoom(video) …

Learn more: [OWASP top 100](https://owasp.org/www-project-proactive-controls/v4/en/introduction)

There are tradeoffs between security and convenience. Take one of the distinct examples, if you choose a complicated password, it is probably hard to remember.

## Claim
All pictures and material credit to CS50 staff
