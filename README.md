Our code starts by a menu with three options:
  1. BIP39
  2. BIP32
  3. Exit
 
 You can write "BIP39", "BIP32" to execute them or press any key to exit.
 
 For BIP39 and BIP32:
    
When you have to enter a 12 words seed, the program checks if the seed is correct.
Indeed, it counts the number of words which should be equal to 12.
Then, it looks if all the words are in the dictionnary.

For BIP39:
    
First, we generate 128 bits of entropy using secrets and random in Python. 

    import random
    import secrets
That's our generate entropy. In order to better secure the seed, we add a checksum to the end of the entropy. To get the checksum, we first take the SHA-256 hash of our entropy. Then, we take the first N/32 bits of the hash and append it to the entropy. In our case, 128/32 bits gives us a 4 bit checksum size.
The final step of the process involves dividing our checksummed bits into “chunks” and mapping those chunks to the mnemonic words from the dictionary. The BIP39 standard specifies that the chunks will always be 11 bits long. So, we divide our 132 bit checksummed entropy into 12 chunks of 11 bits each. Each of these 11 bit chunks can be interpreted as an unsigned 11 bit integer value ranging from 0-2047. This “maps” to a word from the dictionary of 2048 words directly! These are standardized and listed in alphabetic order. So, we can take the 11 bit chunk as an index in the dictionary to extract the words we need. Finally, we get our 12 words.

For BIP32:

First we use our functions to find the entropy of 128 bits of the seed (without the checksum). Then we convert these bits into bytes. We put these bytes in the HMAC-SHA512 and we get an output in hexadecimal with a length of 128. We convert this output from hex to binary to get our 512 bits. We keep the first 256 bits to generate our Master Private Key and the last 256 to get our Master Chain Code.
