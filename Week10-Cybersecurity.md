# Week 10 – Cybersecurity

## [Course link]( https://cs50.harvard.edu/x/2024/weeks/10/)</br>
[![week10 - Cybersecurity]( https://img.youtube.com/vi/EKof-cJiTG8/0.jpg)](https://youtu.be/EKof-cJiTG8)

## Week 10 - Cybersecurity
## Purposes
To protect information, property, and more.
For example:
Secure your account, Data, System, Software,…

## Some suggestions for creatiing your password
*	For user:
－>　Avoid easy password like:123456, abc, qweryui, etc,…
－>　Combine characters(including upper and lower case) and symbols
－>　Change your password periodically.

*	For Verifiers:
－>　Verifiers should allows all printed ASII characters and Unicode symbols up to 64 characters in length.(Inhancment of complexity)
－>　Verifiers should not allow unautheticated claimants to access password hints.(Only trust authenticated services)
－>　Verifiers should limit the number of failed authentication attemps and lock out potential adversaries.
…
## Password nowadays
*	Password Managers
Automatically generate complicate password and remebere them for you.
* Two-factor/Multi-factor Authentication
Verify your identity with two devices.
End-to-end Encryption
End-to-end encryption is a way by which encrypting and decrypting happen on the same system without a middleman.(A more practical example: Zoom and Apple messages.)
## Interesting fact
Deletion in computer didn't really remove "all" data. It still preserves remnants in your hard disk.
－> Secure deletion comes into play: It turns those remnants files into zeros and ones.
－> Some remnants are still remained, since your deleted files are inaccessible by operation system.

## Technique
* Hashing
Using difficult math to encrypt your information. One-way hashing allows only the hashed password to be stored, so your original password wouldn't be known.
![image](https://hackmd.io/_uploads/Hyynx7Uc0.png)

![image](https://hackmd.io/_uploads/BJYaemUq0.png)

To avoid the same result, add another input to interfere the result:
![image](https://hackmd.io/_uploads/r1wH4QL5A.png)
![image](https://hackmd.io/_uploads/HJHvVmUqC.png)

## Cryptography
Similar to hashing, a cipher algorithm use a public key and text to create ciphertext.
In turn, a private key and the ciphertext can be fed to the algorithm to decrypt the text.

## Passkeys
Unlike traditional username and password to represent a single account, It is a password less verification.
When you first visit a website, your device will generate both public key and private key and send the public key to the website.
Next time when you log in, the website will send a challenge message, your device uses a math algorithm to encrypt with your private key, turn out generating a signature.
On the web side, the web decrypt the signature with the public key you provide last time to prove you are the same user.

![image](https://hackmd.io/_uploads/rybDpfIqC.png)
Examples: Microsoft Authenticator App or Windows Hello, faceID, fingerprint (biology identification), FIDO and more.

## Encryption
Encryption is a way by which data is obscured such that only the sender and intended receiver can be read. Use someone's public key to encrypted message, and use their private key to decrypt it.
Example: whatsapp, i-message, Zoom(video) …

Learn more: [OWASP top 100](https://owasp.org/www-project-proactive-controls/v4/en/introduction)

There are tradeoffs between security and convenience. Take the easiest example, if you choose a very difficult password, it’s probably very hard to remember it.

## Claim
All pictures and material credit to cs50 staffs
