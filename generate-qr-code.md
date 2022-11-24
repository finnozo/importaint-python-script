```python
from PIL import Image, ImageFont, ImageDraw
import qrcode

import csv

with open('qrdata.csv', mode ='r')as file:
    csvFile = csv.reader(file)
    for lines in csvFile:
        fileName = lines[0]
        img=qrcode.make(fileName)
        img.save(str(fileName)+'.png')
        my_image = Image.open(str(fileName)+'.png')
        title_font = ImageFont.truetype('aa.ttf', 37)
        title_text = str(fileName)
        image_editable = ImageDraw.Draw(my_image)
        image_editable.text((34,245), title_text, font=title_font)
        my_image.save(str(fileName)+'.png')
        image = Image.open(str(fileName)+'.png')
        print(f"Original size : {image.size}") # 5464x3640
        h,w = image.size
        sunset_resized = image.resize((int(h/2), int(w/2)))
        sunset_resized.save(str(fileName)+'.png')
```
