![logo](images/logo.png)


==========

Platform independent Python3 tool to implement LSB/MSB image steganography and a basic detection technique. Features:

 - Embed within LSBs or MSBs.
 - Extract hidden data.
 - Basic analysis of images to detect LSB or MSB steganography.

How to use:

    $ python lsb.py 
    LSB steganogprahy. Hide files within least significant bits of images.
    $ python msb.py 
    MSB steganogprahy. Hide files within most significant bits of images.
    
    Exmaples below are shown for lsb.py, replace lsb.py with msb.py for MSB steganography
    
    Usage:
      lsb.py hide <img_file> <payload_file> <password>
      lsb.py extract <stego_file> <out_file> <password>
      lsb.py analyse <stego_file>


Hide
----

Hide an archive:

    $ python lsb.py hide samples/orig.jpg samples/secret.zip
    [*] Input image size: 640x425 pixels.
    [*] Usable payload size: 99.61 KB.
    [+] Payload size: 74.636 KB 
    [+] samples/secret.zip embedded successfully!


 
Original image:

![original image](images/orig.jpg)

Image with 75k archive embedded:

![Embedded archive](images/stego.jpg)
 
Extract
-------

    $ python lsb.py extract samples/orig.jpg-stego.png out
    [+] Image size: 640x425 pixels.
    [+] Written extracted data to out.
    
    $ file out 
    out: Extracted bytes, handle depending on what type of data was embedded

Detection
---------

A simple way to detect tampering with least significant bits of images is based on the observation that regions within tampered images will have the average of LSBs/MSBs around 0.5, because the LSBs/MSBs contain encrypted data, which is similar in structure with random data. So in order to analyse an image, we split it into blocks, and for each block calculate the average of LSBs/MSBs. To analyse a file, we use the following syntax (example of LSB used):

    $ python lsb.py analyse <stego_file>

**Example**

![Castle](images/castle.jpg)

Now let’s analyse the original:

    $ python lsb.py analyse castle.jpg

![Original iamge analysis](images/analysis-orig.png)

… and now the one containing  our payload:

    $ python lsb.py analyse castle.jpg-stego.png

![Stego image analysis](images/analysis-stego.png)


Notes
-----
 
 - It is entirely possible to have images with the mean of LSBs/MSBs already very close to 0.5. In this case, this method will produce false positives.
 - More elaborate theoretical methods also exist, mostly based on statistics. However, false positives and false negatives cannot be completely eliminated.

