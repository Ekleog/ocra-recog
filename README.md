Note: This document describes the setup for recognizing characters from a known
font. It works especially well when the characters follow a logic that can be
auto-generated.

For a more general tutorial, see two tutorials by Dan Vanderkam
[here](http://www.danvk.org/2015/01/09/extracting-text-from-an-image-using-ocropus.html)
and [there](http://www.danvk.org/2015/01/11/training-an-ocropus-ocr-model.html).

# Steps for generating your own recognizer for a known font for use with ocropus

1. Generate some text using the chars you want (in my case, a hexdump-like format):
   `cat /dev/urandom | hexdump | sed 's/ /: /' | head -c 100000 > train.txt'
2. Generate the training data:
   `ocropus-linegen -t train.txt -f OCR-A.ttf`
3. Generate the model:
   `ocropus-rtrain -o model linegen/*/*.png`
