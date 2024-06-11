# Introduction

Today we are going to try a new section called "JWT attacks" and its various labs.

Firstly we must to know what are JWT tokens, functions, values and the impact of these tokens if we find a vulnerability.

# What is JWT tokens? 

JSON Web Tokens are authentication tokens signed by JSON. These tokens can, theoretically, contain any kind of data although they are mostly used to manage sessions and send information about users as a part of the authentication process.

# Format of JWT tokens

JWT tokens are simple base64 encoded with specific notation.

1. Header

![[Pasted image 20221201121815.png]]
	As we can see all JWT tokens start with ´ey...´. in the left side we observe the overall structure of JWT tokens.
	In the right side there is the structure of JWT in [JWT Online](https://jwt.io/) . The header contains metadata about the token itself (the algorithm of the cipher and its type),  and later we are going to discuss the `alg` and `typ`  section.
	The  `alg` section specifies the type of algorithm used to sign the token, in JWT we have 2 basic types of signing algorithms.
	1. HS256: the token was signed symetrically
	2. RS256: the token was signed  asymetrically
	
 	
2. Payload

![[Pasted image 20221201125618.png]]
The second part of the token is the payload, in the right side there are some basic values. ATTENTION (the values of the payload section may depends on the requested data of the aplication). Anyone who has a pair of hands can modify de code of the toke however is not easy at all, JWT implements security measures to control this tokens, but sometimes some security controls are not implemented correctly AND WE KNOW THAT, THAT IS WHY WE ARE HERE, TO LEARN THIS!!.

3. Signature

![[Pasted image 20221201130829.png]]
The last part of the token is the signature. The signature is here for a reason, the proccess to sign a token is easy.
	1. The token generate the signature by hashing the header and payload.
This procces involves a secret signing key, this key is stored in th back-end, it's very usefull because this method verify that none of the data was tampered since it was used.

For now, we learnt about the structure of JWT and the process of signing.

Now we have a little break to explain 2 specifications of JWT tokens.

# Various types of JWT

Commonly when we are talking about JWT we are talking about a format for representing information as a JSON object indeed, we have other 2 specifications JWS and JWE.
	JWS: JSON Web Signature: The common JWT token.
	º
	JWE: JSON Web Encryption: Other type of JWT but the difference is that this token is encrypted.


# JWT Attacks (Vulnerabilities)

In this section we are going to learn some real STUFF, first we need to know what involve this attacks, the impact and how these vulnerabilities arise.

## Impact of JWT attacks

Mostly, the impact of these attacks are severed, if an attacker is able to create their own valid tokens with arbitrary values, the attacker is able to escalate privileges and take full control of all accounts.

## How JWT attacks arise?

It depends, there are many forms to bypass/attack this tokens in order to escalate privileges.
Now i will include some forms of how these vulnerabilities arise:

1. The signature is not verified properly >> Enable tampering.
2.  The secret key was leaked.

# JWT Attacks (based on the port swigger labs)


Now we are goingo to make some excercises with explanation, it's important yo have this 2 extensions in Burpsuite (this extensions are free) 
* [JWT EDITOR](https://portswigger.net/bappstore/26aaa5ded2f74beea19e2ed8345a93dd)
* [JSON WEB TOKENS](https://portswigger.net/bappstore/f923cbf91698420890354c1d8958fee6)
NOTE: For this exercises I'm using burpsuite professional, although it's not necessary in this section, this note may change depends on the topic.

## Exploiting flawed JWT signature verification

Link to exercice [EJ1](https://portswigger.net/web-security/jwt/lab-jwt-authentication-bypass-via-unverified-signature) 

In this exercice we need to bypass the unverified signature nd delete the user carlos, 
![[Pasted image 20221202192236.png]]

First we need to login, it's obvious because if we aren't logged on there's no JWT.
![[Pasted image 20221202192650.png]]

After we logged as wiener:peter we need to gain access to the admin panel located in /admin, then  change de URL
![[Pasted image 20221202193055.png]]
to
![[Pasted image 20221202193118.png]]
And send it into the repeater.
Unfortunately we don't have the neccesary permissions to do that.
![[Pasted image 20221202193417.png]]
However we have a capable user to do that: Administrator
![[Pasted image 20221202193440.png]]

We need to be administrator to do that but how? easy.
Changing the username in our JWT token
The process is simple, just select the payload section and change the username, EASY AT ALL
![[Pasted image 20221202194200.png]]
Proceed to change the username:
![[Pasted image 20221202194428.png]]
The next step in our conquest it's simple, acces the admin panel and search methods to delete the dammed user called CARLOS.

After changing the token we see this interface:
![[Pasted image 20221202194558.png]]
AND now the glorious moment
![[b9bc887c-d36d-44eb-a030-623e3eba8036_text.gif]]
Delete the user carlos BUT PREVIOUSLY, we need to change the token AGAIN!!! (oh jeez)
![[Pasted image 20221202194947.png]]
Lab completeee.
