# How-to-download-protected-view-only-files-from-google-drive??

1. Open or Preview Any view-only or protected files from google drive.

2. Open Developer Console.
    If you are previewing in Google Chrome or Firefox
    Press Shift + Ctrl + J ( on Windows / Linux) or Option + ⌘  + J (on Mac)
    If you are previewing in Microsoft Edge 
    Press Shift + Ctrl + I 
    If you are previewing in Apple Safari
    Press Option + ⌘ + C
    Then you will find yourself inside the developer tools.
    
3.  Navigate to the "Console" tab.

4.  Paste below code and press Enter:
5.  Updated: Fix: Uncaught TypeError: Failed to set the 'src' property on 'HTMLScriptElement': This document requires 'TrustedScriptURL' assignment.
```
     // Create a Trusted Types policy if available
const policy = window.trustedTypes 
  ? trustedTypes.createPolicy('default', { createScriptURL: url => url })
  : null;

let jspdf = document.createElement("script");
jspdf.onload = function () {
  let pdf = new jsPDF();
  let elements = document.getElementsByTagName("img");
  for (let i in elements) {
    let img = elements[i];
    if (!/^blob:/.test(img.src)) {
      continue;
    }
    let canvasElement = document.createElement('canvas');
    let con = canvasElement.getContext("2d");
    canvasElement.width = img.width;
    canvasElement.height = img.height;
    con.drawImage(img, 0, 0, img.width, img.height);
    let imgData = canvasElement.toDataURL("image/jpeg", 1.0);
    pdf.addImage(imgData, 'JPEG', 0, 0);
    pdf.addPage();
  }
  pdf.save("download.pdf");
};

// Use the policy to set the src attribute if available
jspdf.src = policy 
  ? policy.createScriptURL('https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js')
  : 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js';

document.body.appendChild(jspdf);
```
        
6. Now, the pdf file start to download. This might take a few minutes depending on the file size.

Another code to try: Source: https://www.youtube.com/watch?v=J5w6HYmzyzU

   ```
	let trustedURL;
	if (window.trustedTypes && trustedTypes.createPolicy) {
		// Create a trusted policy for the script URL
		const policy = trustedTypes.createPolicy('myPolicy', {
	    	createScriptURL: (input) => input
		});
		trustedURL = policy.createScriptURL('https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js');
	} else {
		console.warn("Trusted Types are not supported in this browser. Falling back to using a plain URL.");
		trustedURL = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js';
	}
	
	let jspdf = document.createElement("script");
	jspdf.onload = function () {
		// Create the PDF document once jsPDF is loaded
		let pdf = new jsPDF();
		let elements = document.getElementsByTagName("img");
		for (let i in elements) {
	    	let img = elements[i];
	    	if (!/^blob:/.test(img.src)) {
	        	continue;
	    	}
	    	let canvasElement = document.createElement('canvas');
	    	let con = canvasElement.getContext("2d");
	    	canvasElement.width = img.width;
	    	canvasElement.height = img.height;
	    	con.drawImage(img, 0, 0, img.width, img.height);
	    	let imgData = canvasElement.toDataURL("image/jpeg", 1.0);
	    	pdf.addImage(imgData, 'JPEG', 0, 0);
	    	pdf.addPage();
		}
		pdf.save("download.pdf");
	};
	
	// Use the trusted URL as the script source
	jspdf.src = trustedURL;
	document.body.appendChild(jspdf);

   ```
