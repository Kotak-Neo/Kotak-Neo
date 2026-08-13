# Static IP Whitelisting for Retail Algo Trading

# Effective From: **1 April 2026**

To comply with the **SEBI circular on retail algorithmic trading participation**, Kotak Neo Trade APIs will require **Static IP Whitelisting** for order execution.

This requirement ensures that **orders are placed only from verified infrastructure**, improving security and regulatory compliance.

From **1 April 2026**, order APIs will accept requests **only from whitelisted static IPs with a valid session created from the same IP**.

---

# What This Change Means

If you are using Kotak Neo APIs to automate trading:

You must:

1. **Whitelist a static IP address**
2. **Create API session from that IP**
3. **Place orders from the same environment**

If requests originate from **non-whitelisted IPs** or **sessions created from another IP**, order APIs will reject the request.

---

# How to Whitelist Static IP

You can configure static IP from the **Kotak Neo platform**.

### Steps

1. Login to **Kotak Neo**
2. Go to **More**
3. Open **Trade API**
4. If you have not already created one → **Create API Application**
5. Click on Add IP and add your **Primary Static IP**
6. Optionally add **Secondary Static IP** from the default application details page as a fallback

---

# Static IP Rules

| Rule | Details |
| --- | --- |
| Maximum IPs | 2 (Primary + Secondary) |
| Change Frequency | Once every **7 days** |
| Session Requirement | New session required after IP change |
| Supported IP types | IPv4 and IPv6 |

---

# How IP Validation Works

IP validation is enforced **only on order APIs**.

### APIs With IP Validation

- Place Order
- Modify Order
- Cancel Order

### APIs Without IP Validation

- Login APIs
- Report APIs
- Portfolio APIs
- Data APIs
- Websocket streams

These APIs **will continue to work normally regardless of IP**.

---

# Session Binding Logic

IP is **bound to the API session**.

This means:

> The session must be created from the same IP that is sending order requests.
> 

### Correct Flow

1️⃣ Whitelist static IP

2️⃣ Run trading system on that environment

3️⃣ Create Neo API session

4️⃣ Place orders

---

# Error Responses

### Non-Whitelisted IP

If the request originates from an IP that is not whitelisted:

```
{
    "stCode": 100008,
    "errMsg": "unauthorized",
    "stat": "Not_Ok"
}
```

---

### Session IP Mismatch

If the IP is whitelisted but the session was created from another IP:

```
{
    "stCode": 1037,
    "errMsg": "session ip doesnt match with reqest ip",
    "stat": "Not_Ok"
}
```

---

# What Happens If IP Changes During Active Session?

Sessions do **not automatically terminate** if IP changes.

However:

- **Order APIs will fail**
- Other APIs will continue to work

Orders will return:

```
unauthorized
```

To resume order placement:

1. Ensure correct IP
2. Create a new API session

---

# Recommended Setup Workflow

### Step 1 — Obtain Static IP

You can get a static IP from:

- Your **Internet Service Provider**
- A **Cloud VPS**

Common choices:

- AWS
- DigitalOcean
- Azure
- Google Cloud

---

### Step 2 — Configure Your Strategy Environment

Run your trading algorithm from the system where the **static IP is configured**.

---

### Step 3 — Verify Your Static IP in your configured environment

You can check your active IP using:

```
GET https://api.ipify.org/
```

If this returns your whitelisted static IP, your environment is correctly configured.

---

### Step 4 — Create API Session

Create the Neo API session from the same environment.

---

### Step 5 — Place Orders

Once session and IP match:

- Place Order
- Modify Order
- Cancel Order

Requests should return **HTTP 200 OK**.

---

# Family Account IP Sharing

Kotak Neo allows **family members to share the same static IP**.

### Rules

- Up to **10 family members** can be added
- Sharing is **only for static IP usage**
- It **does not give account access**

Important:

Existing **login family relationships do not automatically apply here**.

You must add family members separately under **Trade API family management**.

---

# Permissions

### Parent Account

Can:

- Add family members
- Add static IP
- Change static IP

---

### Child Accounts

Can:

- Create API application

Cannot:

- Add or modify static IP

---

# Order Type Guidance

### Are Market Orders Allowed?

As per SEBI circular, **market orders are not allowed for retail algos**.

Kotak Neo recommends using **limit orders**.

If you still send a **market order**, the system will automatically according to below grid:

Price grid:

| **Security type** | **Price range (in ₹)** | **Percentage of the Last Traded Price (LTP)** |
| --- | --- | --- |
| EQ and FUT | Less than 100 | 2% |
| EQ and FUT | Between 100 and 500 | 1% |
| EQ and FUT | More than 500 | 0.50% |
| OPT | Less than 1 | .1 Rs (absolute) |
| OPT | Between 1 to 5 | 10% |
| OPT | Between 5 to 10 | 5% |
| OPT | Between 10 and 100 | 3% |
| OPT | Between 100 and 500 | 2% |
| OPT | More than 500 | 1% |

**Buy orders use a protection limit above the LTP; sell orders use a limit below the LTP.**

**Same logic would be valid for AMO market orders.**

**Please note,**

1. **in case of options, IF LTP is unavailable we have to reject the order. Make sure the rejection reason is clear.**
2. **Client place order in Market but in order book Limit order will get visible**

For **precise execution**, always use **limit orders**.

---

# Algo ID Requirement

### Do users need to send Algo ID in order payload?

No.

Kotak Neo APIs automatically append the **appropriate Algo ID**.

This is because:

- APIs are rate limited to **10 Orders Per Second**
- APIs are intended for **tech-savvy retail users**

The system automatically sends the required **exchange-compliant Algo ID**.

---

# Fintech Partner Users

### What if I use a fintech platform to place algo trades?

As per SEBI circular:

Your fintech partner must:

1️⃣ **Get empanelled with exchanges**

2️⃣ **Host their infrastructure on broker systems**

To continue using the service, confirm with your fintech partner:

- Are they **exchange empanelled?**
- Are they **hosted on Kotak infrastructure?**

---

# FAQs

### How many IP addresses can I whitelist?

Maximum **2 IP addresses**:

- Primary IP
- Secondary IP (fallback)

---

### How many sessions can I create?

You can create **multiple sessions**.

However:

Orders can only be placed from **2 sessions simultaneously**:

- Session created from Primary IP
- Session created from Secondary IP

---

### How often can I change my IP?

You can update IP **once every 7 days**.

After changing IP, **create a new API session**.

---

### Is there any delay after whitelisting IP?

No.

Changes apply **immediately**.

---

### Can I use IPv6?

Yes.

IPv6 can be whitelisted.

---

### Can multiple accounts use the same IP?

Yes.

Family accounts can share the same IP.

---

### Can I run multiple strategies from the same IP?

Yes.

Multiple strategies can run from the same IP if sessions are created from that environment.

---

### Do websocket streams require the same IP?

No.

Websocket connections are **not restricted by IP validation**.

---

# Need Help?

If you face issues configuring static IP or sessions, please contact **Kotak Neo API support**.

service.securities@kotak.com