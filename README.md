# README

Credit to a guide by Alexei Czeskis

NOTE: all RSA keys must be in .pem format
NOTE: all commands are run in terminal using a zsh (or other equivalent) shell
NOTE: need to generate keys before anything else ($ ./generate_keys)

1. Public key exchange between individuals (verify keys over the phone or other method)
2. Sender encrypts file
	$ ./encrypt_file <file> <receiver public_key>
3. Sender verifies the encrypted file
	$ ./sign <file> <sender private_key>
4. Sender shares all '.enc' files and the 'signature.sha256' with the receiver
5. Receiver verifies signature
	$ ./verify <file> <signature> <receiver public_key>
6. Receiver decrypts file
	$ ./decrypt_file <file.enc> <receiver private_key> <key.bin.enc>

# Sending:
- Sender needs the original file and the receiver's public key.
- Encrypt file using receiver's public key
- $ ./encrypt_file info.txt receiver.pub.pem
	-> Generates key.bin, key.bin.enc, and info.txt.enc
- Sign encrypted file with sender's private key.
- $ ./sign.sh info.txt.enc sender.pem
	-> Generates signature.sha256
- Send info.txt.enc, key.bin.enc, and signature.sha256 to Bob

# Receiving:
- Receiver needs the encrypted files, the signature, and Sender's public key
- Verify the file using the signature and Sender's public key
- $ ./verify.sh info.txt.enc signature.sha256 sender.pub.pem
	-> Will say 'Verified OK' or 'Verification Failure'
- Decrypt file using Receiver's private key
- $ ./decrypt_file info.txt.enc receiver.pem key.bin.enc
	-> Generates original info.txt and original key.bin
