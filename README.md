# EX-NO-9-RSA-Algorithm

## AIM:
To Implement RSA Encryption Algorithm in Cryptography

## Algorithm:


Step 1: Design of RSA Algorithm  
The RSA algorithm is based on the mathematical difficulty of factoring the product of two large prime numbers. It involves generating a public and private key pair, where the public key is used for encryption, and the private key is used for decryption.

Step 2: Implementation in Python or C 
This algorithm can be implemented in languages like Python or C by performing large integer calculations for key generation, encryption, and decryption, utilizing libraries for modular arithmetic if necessary.

Step 3: Algorithm Description  
1. Key Generation:
   - Select two large prime numbers \( p \) and \( q \).
   - Calculate \( n = p \times q \), which will be used as the modulus.
   - Compute the totient \( \phi(n) = (p - 1)(q - 1) \).
   - Choose a public exponent \( e \) such that \( e \) is coprime with \( \phi(n) \).
   - Compute the private key \( d \), which is the modular inverse of \( e \) mod \( \phi(n) \).

2. Encryption:
   - Convert the plaintext message \( M \) into a numerical form \( m \) (such that \( 0 \le m < n \)).
   - Compute the ciphertext \( c \) using the formula: \( c = m^e \mod n \).

3. Decryption:
   - Use the private key \( d \) to recover \( m \) from \( c \) using: \( m = c^d \mod n \).
   - Convert \( m \) back into the original message \( M \).

Step 4: Mathematical Representation  
- Encryption: \( E(m) = m^e \mod n \)
- Decryption: \( D(c) = c^d \mod n \)

Step 5: **Security Foundation  
The security of RSA relies on the difficulty of factoring large numbers; thus, choosing sufficiently large prime numbers for \( p \) and \( q \) is crucial for security.

## Program:
```
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

/* Euclidean GCD */
int gcd(int a, int b) {
    a = abs(a); b = abs(b);
    while (b != 0) {
        int t = b;
        b = a % b;
        a = t;
    }
    return a;
}

/* Modular exponentiation: (base^exp) % mod */
long long mod_exp(long long base, long long exp, long long mod) {
    long long result = 1 % mod;
    base %= mod;
    while (exp > 0) {
        if (exp & 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}

/* Modular inverse of e modulo phi using extended Euclidean algorithm.
   Returns -1 if inverse does not exist. */
int mod_inverse(int e, int phi) {
    int r0 = e, r1 = phi;
    int s0 = 1, s1 = 0;
    while (r1 != 0) {
        int q = r0 / r1;
        int r2 = r0 - q * r1; r0 = r1; r1 = r2;
        int s2 = s0 - q * s1; s0 = s1; s1 = s2;
    }
    /* now r0 = gcd(e,phi), s0 is the coefficient for e: s0*e + t0*phi = r0 */
    if (r0 != 1) return -1;           /* inverse doesn't exist */
    if (s0 < 0) s0 += phi;            /* make positive */
    return s0;
}

int main(void) {
    /* Example small primes (educational only) */
    int p = 61;
    int q = 53;

    int n = p * q;
    int phi = (p - 1) * (q - 1);

    int e = 17; /* public exponent */
    if (gcd(e, phi) != 1) {
        printf("Error: e and phi(n) are not coprime.\n");
        return 1;
    }

    int d = mod_inverse(e, phi);
    if (d == -1) {
        printf("Error: modular inverse for e does not exist.\n");
        return 1;
    }

    printf("Public Key:  (e = %d, n = %d)\n", e, n);
    printf("Private Key: (d = %d, n = %d)\n\n", d, n);

    char message[100];
    printf("Enter a message to encrypt (ASCII characters): ");
    if (!fgets(message, sizeof(message), stdin)) return 1;

    /* Remove trailing newline if present and recompute length */
    size_t len = strlen(message);
    if (len > 0 && message[len - 1] == '\n') {
        message[len - 1] = '\0';
        len--;
    }

    if (len == 0) {
        printf("Nothing to encrypt.\n");
        return 0;
    }

    /* Encrypt each character (ASCII) as a separate number */
    long long *encrypted = malloc(len * sizeof(long long));
    if (!encrypted) {
        perror("malloc");
        return 1;
    }

    printf("\nEncrypted Message (numeric values):\n");
    for (size_t i = 0; i < len; i++) {
        int m = (unsigned char)message[i];         /* plaintext number */
        encrypted[i] = mod_exp(m, e, n);          /* c = m^e mod n */
        printf("%lld ", encrypted[i]);
    }
    printf("\n");

    /* Decrypt */
    printf("\nDecrypted Message:\n");
    for (size_t i = 0; i < len; i++) {
        long long dec_val = mod_exp(encrypted[i], d, n); /* m = c^d mod n */
        printf("%c", (char)dec_val);
    }
    printf("\n");

    free(encrypted);
    return 0;
}

```



## Output:
<img width="867" height="438" alt="image" src="https://github.com/user-attachments/assets/f84cf1d5-b254-4564-893e-8290ccfa7f0e" />




## Result:
 The program is executed successfully.
