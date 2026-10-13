# Image to PDF in Ubuntu

1. First, reduce the quality to bring the size down and rotate the image if necessary using **Imagemagick convert** tool

`$ convert -density 300 -rotate 90 IMG_20261013_192504.jpg -quality 30 page1.pdf`

2..n-1. Repeat this for each image that needs to become a small PDF.

n. Finally merge them all using **ghostscript** tool

`$ gs -dBATCH -sDEVICE=pdfwrite -sOutputFile=merged.pdf page1.pdf page2.pdf`

For this one, you'd be expected to hit *ENTER* a couple of times

Done!
