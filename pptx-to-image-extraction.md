```python
from pptx import Presentation
from pptx.enum.shapes import MSO_SHAPE_TYPE
import os

def iter_picture_shapes(prs):
    scount=0
    for slide in prs.slides:
        scount = scount + 1
        i = 0
        il = [];
        data = ''
        for shape in slide.shapes:
            print(shape)
            if shape.shape_type == MSO_SHAPE_TYPE.AUTO_SHAPE:
                d = shape.text
                if d:
                    data = d
            if shape.shape_type == MSO_SHAPE_TYPE.TABLE:
                d = shape.table
                for column in d.rows:
                    for cell in column.cells:
                        print(cell.text)
                        data = data +' '+ cell.text
            if shape.shape_type == MSO_SHAPE_TYPE.PICTURE:
                image = shape.image
                # ---get image "file" contents---
                image_bytes = image.blob
                # ---make up a name for the file, e.g. 'image.jpg'---
                i = i + 1
                image_filename = 'slide_'+ str(scount)+'_'+ str(i) +'.'+ image.ext
                il.append(image_filename)
                with open(image_filename, 'wb') as f:
                    f.write(image_bytes)
        f = open("data.txt", "a")
        f.write('\n')
        f.write(data)
        f.write('\n')
        for im in il:
            iName = im
            print(iName)
            print(data)
            if 'Code – ' in data:
               temp = data.replace(' ','')
               lindex = len(temp)
            #    if 'Qty:' in temp:
            #         lindex = temp.index("Qty:")
               code = temp[(temp.index('Code–'))+5:lindex].strip()
               print(code)
               iName = iName.replace('slide',code)
               print(iName)
               os.rename(im, iName)
            elif 'Code - ' in data:
               temp = data.replace(' ','')
               lindex = len(temp)
            #    if 'Qty:' in temp:
            #         lindex = temp.index("Qty:")
               code = temp[(temp.index('Code-'))+5:lindex].strip()
               print(code)
               iName = iName.replace('slide',code)
               print(iName)
               os.rename(im, iName)
            elif 'CODE' in data:
               temp = data.replace(' ','')
               lindex = len(temp)
            #    if 'Qty:' in temp:
            #         lindex = temp.index("Qty:")
               code = temp[(temp.index('CODE'))+4:lindex].strip()
               print(code)
               iName = iName.replace('slide',code)
               print(iName)
               os.rename(im, iName)
            f.write(iName+" ")
        f.write('\n------------------------------------------')
        f.close()



for i in ['1']:
    iter_picture_shapes(Presentation(i+'.pptx'))
```
