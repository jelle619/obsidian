# Generating a PGP key with GNU Privacy Guard
1.  Download and install [the GPG command line tools](https://www.gnupg.org/download/) for your operating system. We generally recommend installing the latest version for your operating system.
    
2.  Open Terminal.
    
3.  Generate a GPG key pair. Since there are multiple versions of GPG, you may need to consult the relevant [_man page_](https://en.wikipedia.org/wiki/Man_page) to find the appropriate key generation command. The below command works on version 2.1.17 and greater.

```shell
$ gpg --full-generate-key
```

4.  At the prompt, specify the kind of key you want, or press `Enter` to accept the default. For help in choosing, consult the [SSH key pair page on choosing an algorithm](Generating%20SSH%20Key%20Pairs.md#Choosing%20an%20algorithm).

5.  At the prompt, specify the key size you want, or press `Enter` to accept the default. If you selected RSA, make sure your key is 4096 bits.

6.  Enter the length of time the key should be valid. Press `Enter` to specify the default selection, indicating that the key doesn't expire. Unless you require an expiration date, we recommend accepting this default.

7.  Verify that your selections are correct.

8.  Enter your user ID information.

*Note: When asked to enter your email address, ensure that you enter the verified email address for your GitHub account. To keep your email address private, use your GitHub-provided `no-reply` email address. For more information, see "[Verifying your email address](https://docs.github.com/en/articles/verifying-your-email-address)" and "[Setting your commit email address](https://docs.github.com/en/articles/setting-your-commit-email-address)."*

9.  Type a secure passphrase.

## Using the key to sign Git commits
You can use a PGP key to sign Git commits. In this example, we will describe on how to do so for [GitHub](https://github.com). To do so, follow the steps below.

1.  Use the `gpg --list-secret-keys --keyid-format=long` command to list the long form of the GPG keys for which you have both a public and private key. A private key is required for signing commits or tags.

```shell
$ gpg --list-secret-keys --keyid-format=long
```

*Note: Some GPG installations on Linux may require you to use `gpg2 --list-keys --keyid-format LONG` to view a list of your existing keys instead. In this case you will also need to configure Git to use `gpg2` by running `git config --global gpg.program gpg2`.*

2.  From the list of GPG keys, copy the long form of the GPG key ID you'd like to use. In this example, the GPG key ID is `3AA5C34371567BD2`:
    
```shell
$ gpg --list-secret-keys --keyid-format=long
/Users/hubot/.gnupg/secring.gpg
------------------------------------
sec   4096R/3AA5C34371567BD2 2016-03-10 [expires: 2017-03-10]
uid                          Hubot 
ssb   4096R/42B317FD4BA89E7A 2016-03-10
```

3.  Paste the text below, substituting in the GPG key ID you'd like to use. In this example, the GPG key ID is `3AA5C34371567BD2`:

```shell
$ gpg --armor --export 3AA5C34371567BD2
# Prints the GPG key ID, in ASCII armor format
```

4.  Copy your GPG key, beginning with `-----BEGIN PGP PUBLIC KEY BLOCK-----` and ending with `-----END PGP PUBLIC KEY BLOCK-----`.

5.  [Add the GPG key to your GitHub account](https://docs.github.com/en/articles/adding-a-gpg-key-to-your-github-account).