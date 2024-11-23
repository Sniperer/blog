---
layout: default
title: Zero-knowledge Proof
parent: Cryptology
grand_parent: Notes
nav_order: 0
---
# User identification problem

1. original
    Register on verifier's system. Then we have a secret, i.e. password, which is known by each other. Submit password and check in database. If submit in plaintext, of course doesn't make sense. Maybe we can entrypt it. But it's still not a good idea.
2. improvement
    Actually, what a prover do in identification problem is try to convince verifier that prover have information of identical secret. In original idea, we just give some pieces of information of secret directly. A better method could be we don't leak information of secret, but try to convince the proposal "prover know secret" is true.

    An example is a user generate a public key and secret key. Like what we do in github, we submit public key to verifier. And verifier choose a random message, encrypt with public key and give ciphertext to prover. If prover own the private key, he can get the right message.
3. zero-knowledge
    Although prover don't leak information about his secret, he leak something else, the message verifier choose. If there is honest verifier, everything is fine. But if there is a attacker, he try to fake your identification and send the ciphertext from real verifier to you. There is a problem.

    Thus we must make sure another must know the plaintext before someone decide to reveal message.

    Then a zero-knowledge version comes out.
    1. V choose a random bit, use public key to encrypt, give it to P.
    2. P decrypt with private key, and sends a commitment to the plaintext Commitment(m';r) to V;
    3. V send m to P
    4. P check whether m=m'; If true, open commitment; Otherwise, stop protocol.
    5. V check whether accept or reject.

    A principle of zero-knowledge is have the prover demonstrate something V already knows and make sure V does indeed knows in advance.

# zero-knowledge

Give a definition. We just care about that the prover's claim is true. If it's false, prover just tell nonsense to verifier. Of course, there is no knowledge.

Thus zero-knowledge intuitively is everything V saw during the protocol can be simulate by X efficiently. X is the face that the prover's claim is true.

Formmal definition is for any PPT V*, there is PPT simulator Sv* on input x, such that Sv* and (P, V*) is comput indis, where on input x and $\delta$ only for V*.

Note that Sv* can rewrite the conversion, but (P, V*) must generate conversion with correct time ordering and one by one. That means sometimes, if it's neccessary we can rewind Sv*.

# Complexity Class of IP

# A zero-knowledge proof system: Graph Isomorphism

# proof

其实这个设计的原则就是，我们准备一些问题，同时这个问题集合的大小是polynomial，欺骗者无法回答全部问题。但simulator可以通过rewinding的手段，准备好问题的答案。如果这个问题的集合过大，将使rewind失效，因为会超polynomial，就像schnorr protocol。

# A zero-knowledge proof system: Graph NonIsomorphism
