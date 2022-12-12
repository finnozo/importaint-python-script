```python
 from PyPDF2 import PdfReader
import os
files = os.listdir()
for f in files:
    if f.endswith(".pdf") or f.endswith(".PDF"):
        reader = PdfReader(f)
        for page in reader.pages:
            text = page.extract_text()
            if text.__contains__('Invoice No:'):
                inv=text[text.index('Invoice No:')+11:text.index('Customer No:')]
                f1 = open("data.txt", "a")
                f1.write(str(f)+'----->'+str(inv))
                print(str(f)+"----->"+str(inv))
                f1.write('\n')
                f1.close()
                break
```
