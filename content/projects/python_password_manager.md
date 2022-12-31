---
date    : 2022-11-28
title   : Security Focused Password Manager in Python
toc     : true
tech    : ["Python",
            "SQL",
            "Git",
            "GitHub"]
---

## Introduction

This project is about a local password manager for generating passwords and managing login credentials. It is developed in Python and offers various security mechanisms to safely store, retrieve and modify any created login records as well as a graphical user interface for a user-friendly experience.

The source code for this project can be found in the repository on [GitHub](https://github.com/zPseudonym/Password-Manager).


## Security and Privacy Features

### Encryption, Hashing and Salting

With [bcrypt](https://pypi.org/project/bcrypt/) — an algorithm designed for securely storing passwords —, the vault's master password is encrypted with an algorithm based on the [blowfish cipher](https://en.wikipedia.org/wiki/Blowfish_(cipher)), but also hashed with a digest size of 184 bit and additional salt to protect against rainbow table attacks. [bcrypt](https://pypi.org/project/bcrypt/) is an adaptive function: over time, the iteration count can be increased to make it slower, so that it remains resistant to brute-force search attacks even as computing power increases.  
The module [cryptography](https://pypi.org/project/cryptography/) on the other hand — specifically its implementation of a symmetric encryption algorithm with [Fernet](https://cryptography.io/en/latest/fernet/) — guarantees that the login credentials encrypted using it cannot be manipulated or read without the key after they have been stored in the database. [Fernet](https://cryptography.io/en/latest/fernet/) is based on the [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) encryption algorithm in [CBC mode](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) and uses the additional padding scheme [PKCS 7](https://en.wikipedia.org/wiki/PKCS_7). Fernet also assures authentication through an [HMAC](https://en.wikipedia.org/wiki/HMAC) that uses the [SHA-256](https://en.wikipedia.org/wiki/SHA-2) hash function, as [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard)-[CBC](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#CBC) does not offer in-built authentication.


### Cryptographic Randomization

The password manager offers a built-in functionality of generating new passwords, where the password's individual characters are randomly shuffled before the password is displayed. For generating random numbers that can be used for cryptography purposes — in this case shuffling the password's characters in a random order —, the module [secrets](https://docs.python.org/3/library/secrets.html) is the only proper implementation of a cryptographically secure pseudorandom number generator ([CSPRNG](https://en.wikipedia.org/wiki/Cryptographically_secure_pseudorandom_number_generator)) in Python as it is non-deterministic; especially in opposition to the built-in module [random](https://docs.python.org/3/library/random.html), which also implements pseudo-random number generators, but is deterministic and therefore not suitable for cryptographic purposes.

### Privacy
This password manager runs completely offline on a user's computer. At no point data is sent to or received from the internet or any other hosts in a network. All generated data and files (e.g. the password database or the encryption key) are stored locally only on the running device. Therefore, only the original author of the password manager's content is able to retrieve or alter any data within the password manager.


## Requirements

### Installing Python
Python is usually already installed on UNIX-based machines, but can also be manually installed from the [Python Website](https://www.python.org/downloads/).


### Installing Modules
Most of the Python modules required for the password manager to run come pre-installed with Python itself. However, there are a few modules that need to be installed manually (e.g. via `pip`):
- [tkinter](https://docs.python.org/3/library/tkinter.html): Included in Python by default
- [sqlite3](https://docs.python.org/3/library/sqlite3.html): Included in Python by default
- [secrets](https://docs.python.org/3/library/secrets.html): Included in Python by default
- [string](https://docs.python.org/3/library/string.html): Included by default in Python
- [pyperclip](https://pypi.org/project/pyperclip/): `python3 -m pip install pyperclip`
- [cryptography](https://pypi.org/project/cryptography/): `python3 -m pip install cryptography`
- [os](https://docs.python.org/3/library/os.html): Included in Python by default
- [bcrypt](https://pypi.org/project/bcrypt/): `python3 -m pip install bcrypt`


## Demo

### Logging in
When using the password manager for the first time, a new master password must be set and confirmed. This master password is the key to the password manager's database as it unlocks the vault and passwords can only be accessed with the master password.  
Afterwards, the password manager can be accessed via the set master password.

Setting a new master password | Logging in with the master password
:-------------------------:|:-------------------------:
![Text](/images/python_pwmgr/firstLoginWindow.png "Setting a new master password")  |  ![Text](/images/python_pwmgr/secondLoginWindow.png "Logging in with the master password")


### Adding a New Record
By entering values for all mandatory fields (`Name`, `Username` and `Password`), a new password record can be added to the password manager's database.

![Text](/images/python_pwmgr/addRecord.gif)


### Generating a Secure Password 
The password manager also features a built-in secure password generator that allows the creation of passwords as per individual requirements in terms of complexity (e.g. length or special characters). The generated password can then be copied to the clipboard and used for an entry in the password manager's database.

![Text](/images/python_pwmgr/pwgenerator.gif)


### Viewing and Hiding Passwords
The presented passwords from the password records are by default shown illegibly as asterisks for improved privacy, but can be toggled to display as clear text via a click on the eye button.

![Text](/images/python_pwmgr/showHidePWs.gif)


### Updating a Record
By specifying a record's unique ID, its values can be changed afterwards.

![Text](/images/python_pwmgr/editRecord.gif)


### Deleting a Record
By specifying a record's unique ID, it can be deleted from the database.

![Text](/images/python_pwmgr/deleteRecord.gif)


## Disclaimer
This project only served to consolidate from scratch the understanding of the development and practical use of a symmetric encryption method and further security measures such as hashing and salting on the basis of a concrete use case, as well as the knowledge of the Python and SQL languages.


## What I Have Learned From This Project
During this project, I learned about the different steps it takes to develop a standalone program with all of the necessary actions, from conceptualizing the design to writing the code. What initially started as a simple terminal application to generate unsafe "random" passwords, is now a full password manager with many functionalities and enhanced security and privacy features. As my focus was to keep security in mind, I made sure to get in depth with topics around proper password storing and best practices. This also meant familiarizing myself with different concepts to enhance security (e.g. encryption, hashing, salting) as well as the best algorithms to use. Regarding the software engineering aspect of this project, I was able to really familiarize myself to a good extent with Python and many of its modules. For all the database communication, I sharpened my skills in SQL. Using git for version control and hosting the repository on GitHub also helped to manage the source code easier.