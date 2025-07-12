<!DOCTYPE html>
<html>
<head>
  <title>Identity Data Encoder</title>
  <script src="https://cdn.jsdelivr.net/npm/bwip-js@3.3.0/browser-bwipjs.min.js"></script>
</head>
<body>
  <h2>Identity Data Encoder</h2>
  <form id="identityForm">
    <label>First Name: <input type="text" id="fname" required></label><br><br>
    <label>Last Name: <input type="text" id="lname" required></label><br><br>
    <label>Date of Birth (ddmmyyyy): <input type="text" id="dob" required></label><br><br>
    <label>Issue Date (ddmmyyyy): <input type="text" id="issue" required></label><br><br>
    <label>Expiry Date (ddmmyyyy): <input type="text" id="expiry" required></label><br><br>
    <label>NIN: <input type="text" id="nin" required></label><br><br>
    <label>Card No: <input type="text" id="card" required></label><br><br>
    <button type="button" onclick="generateData()">Generate</button>
    <button type="button" onclick="downloadText()">Download .txt</button>
  </form>

  <h3>Generated Data:</h3>
  <textarea id="output" rows="10" cols="100"></textarea>

  <h3>PDF417 Barcode:</h3>
  <canvas id="barcodeCanvas"></canvas>

  <script>
    function base64Encode(input) {
      return btoa(input);
    }

    function generateData() {
      const fname = document.getElementById("fname").value.trim();
      const lname = document.getElementById("lname").value.trim();
      const dob = document.getElementById("dob").value.trim();
      const issue = document.getElementById("issue").value.trim();
      const expiry = document.getElementById("expiry").value.trim();
      const nin = document.getElementById("nin").value.trim();
      const card = document.getElementById("card").value.trim();

      const b64Fname = base64Encode(fname.toUpperCase());
      const b64Lname = base64Encode(lname.toUpperCase());

      const staticPart = "RPECrnBEDALrfEPfA0aEhAcDtoiB3gPapoSgA/NyRXUD+GJD7gQHloNLBESmQ7YEla6EJgSVsoTNBJVthS4E58aCBgT2qoKFBPauQscFQrZBvwVMJYUPBXqbhXoFy7RFQgXWqkL6BeXMhCYGDe+CgAY7ukSgBk92gooGocaEzQamjoMtBsRhhUwGySOEiwcWkIHEByoeQbAHZxFCXQeKikMjB490RD8HnxBCmQfH+kD0B+EbQc4H4Q5BaQfwFEQcCAR8gd4IDgWE8QgZEoKKCFb7g1UIdHlD+AiTBISgCLGFgbAIygJBmwj4hkU4CRIGRUIJK4hCzAlPgA==";
      const region = "[FNG]1;50;";
      const signature = "5Ccz6awm3fOyjexC+hHFPVct6R2/xWvHlsBdsAPmisUnrIw0quxeJRhlQpZkFE7OSUcJhcJWLvhXJ5m0HTag1HKGfnHrKNKjPeHDjAvy5kFnd7pvH9uC0pLS57+A+1cC/hhanKUHrMUaPCX0xg7hEVG05GezMYB83elsFCrvKIsXhtOyKT8i1Mm9uwbhYAu/d+ZJn9lxLNSV/kYB9Uxvo7i1TWdPcZP9cUvRIMomeHbkfXnE11w5+Hq1sAUv7JXjkCCvB45v7tb0CByY0uCxPQbCp80";

      const finalOutput = `${b64Fname};${b64Lname};;${dob};${issue};${expiry};${nin};${card};${staticPart}${region}${signature}`;

      document.getElementById("output").value = finalOutput;

      // Generate PDF417 Barcode
      try {
        bwipjs.toCanvas("barcodeCanvas", {
          bcid:        "pdf417",    // Barcode type
          text:        finalOutput, // Text to encode
          scale:       2,           // 2x scaling factor
          height:      10,          // Bar height, in mm
          includetext: false        // Do not include human-readable text
        });
      } catch (e) {
        alert("Error generating barcode: " + e);
      }
    }

    function downloadText() {
      const content = document.getElementById("output").value;
      const blob = new Blob([content], { type: "text/plain;charset=utf-8" });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "identity_data.txt";
      a.click();
      URL.revokeObjectURL(url);
    }
  </script>
</body>
</html>
