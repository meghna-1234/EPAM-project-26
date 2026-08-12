# Assignment 1 - JavaScript Case Study: Bank Customer Page

---

## Course Completion Certificate

![Infosys Springboard - JavaScript Case Study - Bank Customer Page Certificate](certificate-assignment1.png)

### Certificate Details
- **Awarded To:** Meghna Patel
- **Course Title:** JavaScript Case Study - Bank Customer Page
- **Issuing Organization:** Infosys Springboard (*Infosys Limited*)
- **Completion Date:** August 1, 2026
- **Issued Date:** Sunday, August 2, 2026
- **Authorized Signatory:** Satheesha B. Nanjappa (*Senior Vice President and Head, Education, Training and Assessment, Infosys Limited*)
- **Certificate Verification:** [https://verify.onwingspan.com](https://verify.onwingspan.com)
- **Adobe Acrobat Certificate Link:** [https://acrobat.adobe.com/id/urn:aaid:sc:AP:33e3e5c8-9dd4-4301-bd8d-e2b1df2bebae](https://acrobat.adobe.com/id/urn:aaid:sc:AP:33e3e5c8-9dd4-4301-bd8d-e2b1df2bebae)
- **Download Original PDF:** [Certificate - JavaScript Case Study - Bank Customer Page.pdf](Certificate%20-%20JavaScript%20Case%20Study%20-%20Bank%20Customer%20Page.pdf)

---

## Case Study Overview: Meg Bank Customer Portal

The objective of this case study is to build an interactive web page for **Meg Bank** using vanilla JavaScript, DOM manipulation, and event handling.

### Features & Workflow
1. **Interactive Greeting Banner**:
   - When a user hovers over the welcome banner (`welcomeBanner`), an alert greeting `"Welcome to Meg Bank !!!"` is triggered via `addEventListener("mouseover", ...)`.
2. **Registration / Guide Box Toggle**:
   - A step-by-step guideline box explains how to generate Diwali coupon codes.
   - Clicking the registration button dynamically hides the instructions (`display = 'none'`).
3. **Customer ID Validation (`verifyMember`)**:
   - Checks if the customer ID contains the designated prefix `"Meg"`.
   - Validates for empty/blank inputs and alerts/writes status messages accordingly.
4. **Voucher / Coupon Code Generation (`createVoucher`)**:
   - Validates eligibility based on customer ID containing `"Meg"`.
   - Generates and displays a unique coupon by appending `"789456"` to the Customer ID.

---

## Source Code (`Assignment 1 - JavaScript Case Study-Bank Customer Page.html`)

```html
<html>

<head>

<title>Meg Bank</title>

<script type="text/javascript">

function initWelcome()
{
    document.getElementById("welcomeBanner").addEventListener("mouseover", notifyUser);

    function notifyUser()
    {
        alert("Welcome to Meg Bank !!!");
    }
}

</script>
<script>
function verifyMember()
{
    var memberID = document.getElementById("accountID").value;
    var isEligible = memberID.includes("Meg");
    if (isEligible)
    {
        document.write("Customer ID is valid,you can proceed further to generate coupon");
    }
    else if(memberID=="")
    {
        document.write("Customer ID can't be blank");
    }
    else
    {
        document.write("You are not a valid customer");
    }
}

function createVoucher()
{
    var memberID = document.getElementById("accountID").value;
    var offerCode = memberID + "789456";
    var isEligible = memberID.includes("Meg");
    if (isEligible)
    {
        document.write("Your coupon is:" + offerCode);
    }
    else
    {
        document.write("Invalid customer ID so coupon can't be generated");
    }
}

</script>
</head>

<body onload="initWelcome();">

<p id="welcomeBanner">Welcome customer !!!</p>
<p id="guideBox">Follow the below Steps to get Diwali coupon:-<br>
1)Enter your customer ID in the textbox<br>
2)Click on validate customer button<br>
3)Click on Register button to hide all the above steps and can apply for coupons<br>

</p>

<button type="button" onclick="document.getElementById('guideBox').style.display='none'">Click here to Register</button><br><br><br>
Enter Customer ID:
<input id="accountID" type="text" name="customerID"><br><br>

<button type="button" onclick="verifyMember()">Validate customer</button>
<button type="button" onclick="createVoucher()">Click here to generate coupon</button>

</body>
</html>
```
