A simple `nslookup` command won't directly check a **single DNS record** that holds both **plaintext data** and an **A record**. This is because:

1.  **DNS Record Types are Distinct:** DNS records are fundamentally separated by type (e.g., A, CNAME, MX, TXT, SRV). An **A record** is *always* for mapping a domain name to an **IPv4 address**. A **TXT record** is *always* for storing **plaintext strings**. They are different records at the DNS level.
2.  **`nslookup` Limitations:** While `nslookup` can query different record types, it can only query **one type at a time** for a given name.

To get the information you described, you need to perform **two separate `nslookup` queries**: one for the **A record** and one for the **TXT record**.

Here's how to do it using the `nslookup` command:

## Using `nslookup` to Query A and TXT Records

You'll use the `set type=` option in `nslookup` for each query. Replace `example.com` with the actual domain name you're interested in.

### 1\. Query for the A Record (The IP Address)

This will retrieve the IPv4 address associated with the domain name.

```bash
nslookup -type=A example.com
```

**Example Output:**

```
Server:     8.8.8.8
Address:    8.8.8.8#53

Name:       example.com
Address:    93.184.216.34  <-- This is the A Record (the IP address)
```

-----

### 2\. Query for the TXT Record (The Plaintext Data)

This will retrieve the plaintext data (e.g., SPF records, domain verification codes) associated with the domain name.

```bash
nslookup -type=TXT example.com
```

**Example Output:**

```
Server:     8.8.8.8
Address:    8.8.8.8#53

example.com    text = "v=spf1 include:_spf.google.com ~all" <-- This is the Plaintext Data (TXT Record)
example.com    text = "google-site-verification=abc-12345"
```

-----

## Alternative: Using `dig` (Recommended for Advanced DNS Checks)

The `dig` command is often preferred by network administrators as it provides more detailed and cleaner output for specific record types. It works similarly to `nslookup` but with different syntax.

### A Record with `dig`

```bash
dig A example.com
```

### TXT Record with `dig`

```bash
dig TXT example.com
```

-----

### **Summary of Concept**

The term "a record that has plaintext and a record" is a bit of a simplification. In reality, you're looking for **two separate DNS records** that happen to share the same domain name:

  * **A Record:** Stores the IP address (`93.184.216.34`).
  * **TXT Record:** Stores the plaintext (e.g., an SPF string `"v=spf1..."`).
