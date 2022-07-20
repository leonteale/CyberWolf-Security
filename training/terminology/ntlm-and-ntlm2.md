# NTLM & NTLM2

NTLM hashes are stored in the Security Account Manager (SAM) database and in Domain Controller's NTDS.dit database. They look like this:

```
aad3b435b51404eeaad3b435b51404ee:e19ccf75ee54e06b06a5907af13cef42
              LM                :             NT
```

Contrary to what you'd expect, the LM hash is the one _before \_the semicolon and the NT hash is the one \_after_ the semicolon. Starting with Windows Vista and Windows Server 2008, by default, only the NT hash is stored.

Net-NTLM hashes are used for network authentication (they are derived from a challenge/response algorithm and are based on the user's NT hash). Here's an example of a Net-NTLMv2 (a.k.a NTLMv2) hash:

```
admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030
```

From a pentesting perspective:

* You **CAN** perform Pass-The-Hash attacks with **NTLM** hashes.
* You **CANNOT** perform Pass-The-Hash attacks with **Net-NTLM** hashes. However, tehy can be used to perfom relay attacks.

You get NTLM hashes when dumping the SAM database of any Windows OS, a Domain Controller's NTDS.dit database or from MimikatzMimikatz (Fun fact, although you can't get clear-text passwords from Mimikatz on Windows >= 8.1 you can get NTLM hashes from memory). Some tools just give you the NT hash (e.g. ) and that's perfectly fine: obviously you can still Pass-The-Hash with just the NT hash.

You get Net-NTLMv1/v2 (a.k.a NTLMv1/v2) hashes when using tools like Responder or Inveigh.

## References <a href="#references" id="references"></a>

[https://byt3bl33d3r.github.io/practical-guide-to-ntlm-relaying-in-2017-aka-getting-a-foothold-in-under-5-minutes.html](https://byt3bl33d3r.github.io/practical-guide-to-ntlm-relaying-in-2017-aka-getting-a-foothold-in-under-5-minutes.html)
