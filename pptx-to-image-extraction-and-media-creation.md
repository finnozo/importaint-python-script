```python

from pptx import Presentation
from pptx.enum.shapes import MSO_SHAPE_TYPE
import os
import time
from pathlib import Path
import requests

def iter_picture_shapes(prs,place,floor):
    scount=0
    for slide in prs.slides:
        scount = scount + 1
        i = 0
        il = [];
        data = ''
        for shape in slide.shapes:
            if shape.shape_type == MSO_SHAPE_TYPE.AUTO_SHAPE:
                d = shape.text
                if d:
                    data = d
            if shape.shape_type == MSO_SHAPE_TYPE.TABLE:
                d = shape.table
                for column in d.rows:
                    for cell in column.cells:
                        cv=cell.text.replace(' ', '').replace('\t', '').replace('\n', '')
                        print(cv)
                        data = data +' '+ cv
            if shape.shape_type == MSO_SHAPE_TYPE.PICTURE:
                image = shape.image
                # ---get image "file" contents---
                image_bytes = image.blob
                # ---make up a name for the file, e.g. 'image.jpg'---
                i = i + 1
                image_filename = str(place)+'_slide_'+ str(scount)+'_'+ str(i) +'.'+ image.ext
                print(image_filename)
                il.append(image_filename)
                with open(image_filename, 'wb') as f:
                    f.write(image_bytes)
                if Path(image_filename).stat().st_size/1024 < 12 and image.ext == 'png':
                    print(image_filename)
                    il.remove(image_filename)
                    os.remove(image_filename)
        f = open("data.txt", "a")
        f.write('\n')
        f.write(data)
        f.write('\n')
        imagesList = []
        thumbnail=''
        for im in il:
            iName = im
            if 'Code – ' in data:
               temp = data.replace(' ','')
               lindex = len(temp)
            #    if 'Qty:' in temp:
            #         lindex = temp.index("Qty:")
               code = temp[(temp.index('Code–'))+5:lindex].strip()
               print(code)
               iName = iName.replace('slide',code)
               os.rename(im, iName)
            elif 'Code - ' in data:
               temp = data.replace(' ','')
               lindex = len(temp)
            #    if 'Qty:' in temp:
            #         lindex = temp.index("Qty:")
               code = temp[(temp.index('Code-'))+5:lindex].strip()
               print(code)
               iName = iName.replace('slide',code)
               os.rename(im, iName)
            elif 'CODE' in data or 'CODE ' in data or ' CODE' in data or ' CODE ' in data:
               temp = data.replace(' ','')
               lindex = len(temp)
            #    if 'Qty:' in temp:
            #         lindex = temp.index("Qty:")
               code = temp[(temp.index('CODE'))+4:lindex].strip()
               iName = iName.replace('slide',code)
               print(iName)
               os.rename(im, iName)
            f.write(iName+" ")
            imagesList.append(iName)
            thumbnail = iName
        f.write('\n------------------------------------------')
        if imagesList:
            jsonDATA = {"address":"NA","alias":"NA","desc":"NA","floorPlanId":floor,"height":0,"illuminationTypeId":1,"mediaFilePath":imagesList,"mediaGroupId":33,"mediaTypeId":151,"minimumContractDuration":0,"placeId":place,"rackPrice":0,"rackPriceUnit":1,"thumbnail":"thumbnail/"+thumbnail,"width":0}
            print(jsonDATA)
            f.write(str(jsonDATA))
            f.write("\n------------------JSON----------------")
            r = requests.post('http://localhost:8080/p-api/v1/place/place-media', json=jsonDATA)
            print(r.status_code)
            print(r.json())
        f.close()


iter_picture_shapes(Presentation('1.pptx'),2846,726)
fileName = input("Enter file name: ")
place = input("Enter place Id: ")
floor = input("Enter Floor Id: ")
place = int(place)
floor = int(floor)

iter_picture_shapes(Presentation(fileName+'.pptx'),place,floor)

```
