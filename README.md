Note: This document describes the setup for recognizing characters from a known
font. It works especially well when the characters follow a logic that can be
auto-generated.

For a more general tutorial, see two tutorials by Dan Vanderkam
[here](http://www.danvk.org/2015/01/09/extracting-text-from-an-image-using-ocropus.html)
and [there](http://www.danvk.org/2015/01/11/training-an-ocropus-ocr-model.html).

# Steps for generating your own recognizer for a known font for use with ocropus

1. Generate some text using the chars you want (in my case, a hexdump-like
   format, for paperkey):
   `cat /dev/urandom | hexdump -e '"%_ad:" 22/1 " %02X" " " 3/1 "%02X" "\n"' | head -n 1000 | sed 's/^\([0-9][0-9][0-9]\).*:/\1:/' > train.txt`
2. Generate the training data (here with high degradations as it's so
   restricted):
   `ocropus-linegen -t train.txt -e hi -f OCR-A.ttf`
3. Generate the model:
   `ocropus-rtrain -o model linegen/*/*.png`

# Steps for using your own recognizer with ocropus

1. Binarize:
   `ocropus-nlbin input.jpg -o input`
2. Segment interesting parts:
   `ocropus-gpageseg input/*.bin.png`
3. Perform recognition:
   `ocropus-rpred -m model-SOMETHING.pyrnn.gz input/*/*.bin.png`
4. Put everything together:
   `ocropus-hocr input/*.bin.png -o result.html`
