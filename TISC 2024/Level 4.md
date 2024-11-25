# AlligatorPay
## TISC LEVEL 4
### Description
In the dark corners of the internet, whispers of an elite group of hackers aiding our enemies have surfaced. The word on the street is that a good number of members from the elite group happens to be part of an exclusive member tier within AlligatorPay (agpay), a popular payment service.

Your task is to find a way to join this exclusive member tier within AlligatorPay and give us intel on future cyberattacks. AlligatorPay recently launched an online balance checker for their payment cards. We heard it's still in beta, so maybe you might find something useful.

https://agpay.chals.tisc24.ctf.sg/

#### Source code
````
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>AlligatorPay Balance Checker</title>
    <link
    ...
    OMITTED
    ...
    </link>
    <style>
      ... 
      OMITTED
      ...
    </style>
  </head>

  <body class="dark-mode">
    <audio id="backgroundAudio" loop>
      <source src="song.mp3" type="audio/mpeg" />
      Your browser does not support the audio element.
    </audio>

    <div class="mute-button" id="muteButton">
      <i class="fas fa-volume-up"></i>
    </div>

    <div class="container">
      <h1 class="mb-4 title">AlligatorPay</h1>
      <!-- banner advertisement for AGPay Exclusive Club promo for customers with exactly $313371337 balance -->
      <img
        src="ad.gif"
        class="advertisement"
        alt=""
        style="width: 100%; padding-bottom: 30px"
      />
      <input type="file" id="fileInput" class="form-control mb-3" />
      <button class="btn btn-primary" id="parseButton">Upload Card</button>
      <!-- Dev note: test card for agpay integration can be found at /testcard.agpay  -->
      <div class="card-container">
        <div class="card" id="card">
          <img src="albert.png" alt="Overlay Image" class="overlay-image" />
          <img src="chip.png" alt="Overlay Image" class="overlay-chip" />
          <img
            src="moostercard.png"
            alt="Overlay Image"
            class="overlay-moostercard"
          />
          <img src="agpay.png" alt="Overlay Image" class="overlay-agpay" />
          <div class="card-number" id="cardNumber">0000 0000 0000 0000</div>
          <div class="card-expiry" id="cardExpiryDate">VALID THRU 00/00</div>
        </div>
        <div class="balance-display" id="balance">$0.00</div>
      </div>
    </div>

    <script>
      ...
      OMITTED
      ...

      async function parseFile() {
        const fileInput = document.getElementById("fileInput");
        const file = fileInput.files[0];
        if (!file) {
          alert("Please select a file");
          return;
        }

        const arrayBuffer = await file.arrayBuffer();
        const dataView = new DataView(arrayBuffer);

        const signature = getString(dataView, 0, 5);
        if (signature !== "AGPAY") {
          alert("Invalid Card");
          return;
        }
        const version = getString(dataView, 5, 2);
        const encryptionKey = new Uint8Array(arrayBuffer.slice(7, 39));
        const reserved = new Uint8Array(arrayBuffer.slice(39, 49));

        const footerSignature = getString(
          dataView,
          arrayBuffer.byteLength - 22,
          6
        );
        if (footerSignature !== "ENDAGP") {
          alert("Invalid Card");
          return;
        }
        const checksum = new Uint8Array(
          arrayBuffer.slice(arrayBuffer.byteLength - 16, arrayBuffer.byteLength)
        );

        const iv = new Uint8Array(arrayBuffer.slice(49, 65));
        const encryptedData = new Uint8Array(
          arrayBuffer.slice(65, arrayBuffer.byteLength - 22)
        );

        const calculatedChecksum = hexToBytes(
          SparkMD5.ArrayBuffer.hash(new Uint8Array([...iv, ...encryptedData]))
        );x

        if (!arrayEquals(calculatedChecksum, checksum)) {
          alert("Invalid Card");
          return;
        }

        const decryptedData = await decryptData(
          encryptedData,
          encryptionKey,
          iv
        );

        const cardNumber = getString(decryptedData, 0, 16);
        const cardExpiryDate = decryptedData.getUint32(20, false);
        const balance = decryptedData.getBigUint64(24, false);

        document.getElementById("cardNumber").textContent =
          formatCardNumber(cardNumber);
        document.getElementById("cardExpiryDate").textContent =
          "VALID THRU " + formatDate(new Date(cardExpiryDate * 1000));
        document.getElementById("balance").textContent =
          "$" + balance.toString();
        console.log(balance);
        if (balance == 313371337) {
          function arrayBufferToBase64(buffer) {
            let binary = "";
            const bytes = new Uint8Array(buffer);
            const len = bytes.byteLength;
            for (let i = 0; i < len; i++) {
              binary += String.fromCharCode(bytes[i]);
            }
            return window.btoa(binary);
          }

          const base64CardData = arrayBufferToBase64(arrayBuffer);

          const formData = new FormData();
          formData.append("data", base64CardData);

          try {
            const response = await fetch("submit", {
              method: "POST",
              body: formData,
            });

            const result = await response.json();
            if (result.success) {
              alert(result.success);
            } else {
              alert("Invalid Card");
            }
          } catch (error) {
            alert("Invalid Card");
          }
        }
      }

      function getString(dataView, offset, length) {
        let result = "";
        for (let i = offset; i < offset + length; i++) {
          result += String.fromCharCode(dataView.getUint8(i));
        }
        return result;
      }

      function arrayEquals(a, b) {
        if (a.length !== b.length) return false;
        for (let i = 0; i < a.length; i++) {
          if (a[i] !== b[i]) return false;
        }
        return true;
      }

      function hexToBytes(hex) {
        const bytes = [];
        for (let c = 0; c < hex.length; c += 2) {
          bytes.push(parseInt(hex.substr(c, 2), 16));
        }
        return new Uint8Array(bytes);
      }

      async function decryptData(encryptedData, key, iv) {
        const cryptoKey = await crypto.subtle.importKey(
          "raw",
          key,
          { name: "AES-CBC" },
          false,
          ["decrypt"]
        );
        const decryptedBuffer = await crypto.subtle.decrypt(
          { name: "AES-CBC", iv: iv },
          cryptoKey,
          encryptedData
        );
        return new DataView(decryptedBuffer);
      }

      function formatCardNumber(cardNumber) {
        return cardNumber.replace(/(.{4})/g, "$1 ").trim();
      }

      function formatDate(date) {
        const month = (date.getMonth() + 1).toString().padStart(2, "0");
        const year = date.getFullYear().toString().slice(2);
        return `${month}/${year}`;
      }
    </script>
  </body>
</html>
````
Mentioned in the source code, a test card can be found at /testcard.agpay
The goal is to make sure the balance for the card is exactly $313371337.

The Javascript code that is used to decrypt the data in the test card can also be found in the source code.

With the test card
- Card No: 123456789
- Balance: $12345678

Upon examining the script, the following can be determined:

| Start | End | Description |
|-------|-----|-------------|
|0      | 5   | Contains the AGPAY header |
|7      | 39  | The key used to encrypt the data |
|49     | 65  | The Initialization Vector (IV) |
|65     | -22 | Contains the encrypted data |
|-22    | -16 | Contains the ENDAGP footer |
| -16   | -0  | Checksum, used to determine the file's integrity |

- The file is first converted into a hex array 
- The key and IV is used to  decrypt the encrypted data in the file.
- A checksum is calculated using MD5 with the IV and the encrypted data concatenated to the back of it
- Matches the calculated checksum with the one attached in the file to determine its integrity

<br>

#### Test card in Hex format

##### Encryption Key
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| 62 | 61 | 60 | BF | F6 | E0 | B8 | A4 | 7C | F9 | 8F | EE | E4 | 83 | 5D | 3C | 
| 05 | A1 | 85 | 4E | 16 | 3B | BC | 8A | 27 | 3B | F5 | CB | 9F | C4 | 09 | 29 |

##### IV
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
|72 |EE |0D |F3 |B8 |2D |3D |C0 |84 |60 |5D |06 |9B |68 |CE |78 |E6 | FF

##### Encrypted Data
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
E6 | FF | DF | 4A | B0 | 49 | 8C | D5 | 68 | C0 | AA | 94 | FF | 13 | E1 | 6A | 0B | 20 | B4 | B7 | 8B | CC | C9 | F5 | 03 | 96 |
C6 | 5C | FF | 74 | 22 | 6F | A2 | 48 | 05 | 2D | 24 | 65 | C5 |
5D | 62 | EF | 17 | 0B | 71 | 76 | 8C | AE

Using CyberChef with AES-CBC to decrypt using the IV and Key provided above:
1234567890123456bOf=, 
which contains all the details of the card, the balance, expiry date, and the number.

<br> 
<br>

##### Convert 1234567890123456bOf= back to Hex
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 62 | 82 | 4f | 80 | 66 | 3d | 18 | 80 | 00 | 00 | 00 | 00 | 00 | bc | 61 | 4e

By converting it back to hex, the following can be determined:

##### Card number from 1234567890123456bOf=
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 30 | 31 | 32 | 33 | 34 | 35 | 36 | 

##### Balance from 1234567890123456bOf=
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
00 | 00 | 00 | 00 | 00 | BC | 61 | 4E

##### Expiry date from 1234567890123456bOf=
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
66 | 3D | 18 | 80

##### Thus $12345678 in hex form is 
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
BC  | 61 | 4E | 

##### And $313371337 in hex form will be 
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
12  | AD | AA | C9 | 

So the balance can now be freely changed. 
However, it will need to be encrypted again using the same ID and Key. 
A new checksum will also need to be calculated as the current checksum attached to the file will not be the same as the one calculated by the server as we have tampered with the balance.

Now using the same Cyberchef formula to encrypt the new data to hex form first:

##### New encrypted data
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
e6 | ff | df | 4a | b0 | 49 | 8c | d5 | 68 | c0 | aa | 94 | ff | 13 | e1 | 6a | 
56 | f5 | 3c | 1a | 99 | bb | 37 | c0 | a9 | 69 | 7c | b6 | af | 16 | 86 | ef | 
d9 | 1b | 54 | af | 75 | 9a | f8 | 30 | 12 | 3f | 08 | 86 | ea | 97 | 9d | 9c

Then converting it back to ASCII, 
1234567890123456bOf=

To calculate the new checksum, use MD5 with the IV and 1234567890123456bOf= added to the back of it, 

##### New checksum
|00|01|02|03|04|05|06|07|08|09|0A|0B|0C|0D|0E|0F|
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
| 4D | 02 | F9 | 68 | 09 | 57 | 07 | 3C | 31 | 38 | F5 | AC | AF | 52 | DA | EA |
| 77 | 02 | 249| 104| 9  | 87 | 07 | 60 | 49 | 56 | 245| 172| 175| 82 | 218| 234|

Now with the new encrypted data and new checksum, edit the existing test card file and replace the hex values of the encrypted data and checksum with the new ones.

Then upload the file onto the website.

Flag: TISC{533_Y4_L4T3R_4LL1G4T0R_a8515a1f7004dbf7d5f704b7305cdc5d}