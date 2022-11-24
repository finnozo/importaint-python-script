```python
import os
import natsort
from fpdf import FPDF

pdf = FPDF()
pdf.add_page()
directory = 'Ground'
i=0
j=0
k=0
s=1
d = natsort.natsorted(os.listdir(directory),reverse=False)
for filename in d:
    if filename.endswith('.png'):
        pdf.image(directory+'/'+filename,j,k,25,25)
        j=j+25
        i=i+1
        if i == 8:
            i=0
            j=0
            k=k+30
        if s % 80 == 0:
            i=0
            j=0
            k=0
            pdf.add_page()
        s=s+1


pdf.output("GroundFloor.pdf", "F")

    
```
