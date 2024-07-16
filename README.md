# Anti Apple Revoke
This is a simple adblock filter list to allow for Apple's more ***restricted*** platforms *(iOS/iPadOS/tvOS)* to use revoked enterprise certificates to sideload apps.


#### Current Revocation servers are (as of `07-16-2024`):
```
ocsp.apple.com
ocsp2.apple.com
valid.apple.com
crl.apple.com
certs.apple.com
appattest.apple.com
vpp.itunes.apple.com
```
 **If your device contacts any of these it will be prevented from installing apps using revoked certificates!**
> - NOTE: These may change as Apple attempts to remedy this bypass. 
 
## Pros vs Cons
- **Pros**
  - **Absolutely Free!**
  - More than 3+ Apps / No 7 Day Limit
  - Certificates last til passive expiration date, which is **`3` YEARS** for enterprise codesigning
  
  - Works with OS Updates, iCloud, Translations, etc.
- **Cons**
  - **Blocking must ALWAYS stay on!**  
    - > (⚠️ Warning: Mobile Hotspot will use other's connection! Make sure that device blocks these servers or it will fail! VPNs must also block these DNS or it will fail!)
  - If blocking fails, **You must <u>erase your phone and restore</u> to reinstall!**
  

### Why Revoked = Reinstall?
 Its because of the way modern certificates function. Apple stores all the revoked certificates' serial numbers in a "trust store"  on your device and prevents you from resetting this unless you ***restore***.

Want a bit more of an explanation on certificates? See:  **How Does It Work?**

---

# How to Use 
The `.txt` should be used with services like [PiHole]() or [Ad Guard Home]() to provide blocking to your devices on your home network. Or better yet use a service like [NextDNS]() to block the servers listed in here and install the block using a `.mobileconfig` [provisioning profile](https://developer.apple.com/documentation/technotes/tn3125-inside-code-signing-provisioning-profiles). 

**TODO:** Provide better install instructions.


## How Does It Work?
Simply put certificates have two states: `valid` and `expired`. However sometimes a certificate need blocked from use before expiration (say if private keys get leaked, usage changes, or YOU STOP PAYING A MULTI-BILLION DOLLAR CORPORATION $100/year TO INSTALL YOUR CODE. 

This is called **REVOCATION**. 

Due to certificates not having a built-in revocation there are two ***active*** ways to revoke certificates [**Certificate Revocation Lists (CRLs)**](https://en.wikipedia.org/wiki/Certificate_revocation_list) and [**Online Certificate Status Protocol (OCSP)**](https://en.wikipedia.org/wiki/Online_Certificate_Status_Protocol).

> As you can see above, both are in use for codesigning on iOS/iPadOS/tvOS devices.

#### CRLs vs OCSP
CRLs are static lists (of serial numbers) detailing invalid certificates and require clients to download and store these while OCSP provides on-demand, dynamic validation through a request-response system.