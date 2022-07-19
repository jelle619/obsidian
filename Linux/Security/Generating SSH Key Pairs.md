Choosing an algorithm# Generating secure key pairs for SSH Authentication
Public-key cryptography, or asymmetric cryptography, is a cryptographic system that uses pairs of keys. Each pair consists of a public key (which may be known to others) and a private key (which may not be known by anyone except the owner).

Secure Shell Protocol (SSH) can make use of key pairs for authentication. This is substantially more safe than using plain passwords. Below, we will outline the best practises for generating these keys. Make sure ssh-keygen is installed on the host system.

## Checking for existing keys
To check all available SSH keys on your computer, run the following command on your terminal:

```bash
$ for key in ~/.ssh/id_*; do ssh-keygen -l -f "${key}"; done | uniq
```

You may remove unneeded keys from the system by navigating to your `.ssh` folder, which can usually by found in your home directory.

## Choosing an algorithm
The following algorithms are available for SSH keys:

-   **DSA**: It’s unsafe and even no longer supported since OpenSSH version 7, you need to upgrade it!
-   **RSA**: It depends on key size. If it has 3072 or 4096-bit length, then you’re good. Less than that, you probably want to upgrade it. Bit-lengths of 1024-bit and lower are considered unsafe.
-   **ECDSA**: It depends on how well your machine can generate a random number that will be used to create a signature. There’s also a [trustworthiness concern](https://www.hyperelliptic.org/tanja/vortraege/20130531.pdf) on the NIST curves being used by ECDSA.
-   **Ed25519**: Considered secure and is more performant than RSA. It’s the most recommended public-key algorithm available today. However, it may not be compatible with some legacy systems.

In general, you should only use RSA (with an appropriate bit-length) or Ed25519.

## Generating a key
After you have chosen an algorithm you want to use, it is time to create a key using ssh-keygen.

### Ed25519
You can have multiple SSH keys on your machine. So if you are switching over from RSA, it is best to keep the old SSH keys and generate a new one that uses Ed25519. This way you can still log in to any of your remote servers. Then slowly replace the authorized key on your remote servers one by one with the newly generated Ed25519 public-key.

Open up your terminal and type the following command to generate a new SSH key that uses the Ed25519 algorithm:

```bash
ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519 -C "john@example.com"
```

You’ll be asked to enter a passphrase for this key, use the strong one. You can also use the same passphrase like any of your old SSH keys.

-   `-o` : Save the private-key using the new OpenSSH format rather than the PEM format. Actually, this option is implied when you specify the key type as `ed25519`.
-   `-a`: It’s the numbers of KDF (Key Derivation Function) rounds. Higher numbers result in slower passphrase verification, increasing the resistance to brute-force password cracking should the private-key be stolen.
-   `-t`: Specifies the type of key to create, in our case the Ed25519.
-   `-f`: Specify the filename of the generated key file. If you want it to be discovered automatically by the SSH agent, it must be stored in the default `.ssh` directory within your home directory.
-   `-C`: An option to specify a comment. It’s purely informational and can be anything. But it’s usually filled with `<login>@<hostname>` who generated the key.

### RSA
Open up your terminal and type the following command to generate a new SSH key that uses the RSA algorithm:

```bash
ssh-keygen -a 100 -t rsa -b 4096 -f ~/.ssh/id_rsa -C "john@example.com"
```

You’ll be asked to enter a passphrase for this key, use the strong one. You can also use the same passphrase like any of your old SSH keys.

-   `-a`: It’s the numbers of KDF (Key Derivation Function) rounds. Higher numbers result in slower passphrase verification, increasing the resistance to brute-force password cracking should the private-key be stolen.
-   `-t`: Specifies the type of key to create, in our case the Ed25519.
- `-b`: Specifies the bit-length of the key to create. For RSA, it is recommend to choose a length of 4096.
-   `-f`: Specify the filename of the generated key file. If you want it to be discovered automatically by the SSH agent, it must be stored in the default `.ssh` directory within your home directory.
-   `-C`: An option to specify a comment. It’s purely informational and can be anything. But it’s usually filled with `<login>@<hostname>` who generated the key.