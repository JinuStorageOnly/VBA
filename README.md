You can add text to multiple PDF invoices using a tool like Python with the PyPDF2 or reportlab libraries. Here's a simple script using PyPDF2 to add a watermark or text stamp to all pages of each PDF:

1. **Install PyPDF2** (if not already installed):
   ```bash
   pip install PyPDF2
   ```

2. **Script to add text to PDFs**:
   ```python
   import os
   from PyPDF2 import PdfReader, PdfWriter
   from PyPDF2.generic import NameObject, TextStringObject

   def add_text_to_pdf(input_pdf, output_pdf, text):
       reader = PdfReader(input_pdf)
       writer = PdfWriter()
       
       # Iterate through all the pages
       for i in range(len(reader.pages)):
           page = reader.pages[i]
           
           # Add text to a specific location (example at top of the page)
           page.merge_page(
               PdfReader("watermark.pdf").pages[0]
           )
           
           # Add text as metadata (an alternative way)
           page.add_annotation({
               NameObject("/Type"): NameObject("/Annot"),
               NameObject("/Subtype"): NameObject("/Text"),
               NameObject("/Contents"): TextStringObject(text)
           })
           
           writer.add_page(page)
       
       # Write to a new PDF
       with open(output_pdf, "wb") as out:
           writer.write(out)

   # Path to your directory containing PDFs
   pdf_dir = "/path/to/your/pdfs"

   # Output directory
   output_dir = "/path/to/output/pdfs"

   # Text to add
   text_to_add = "ID: XYZ123"

   # Ensure output directory exists
   if not os.path.exists(output_dir):
       os.makedirs(output_dir)

   # Process all PDFs in the directory
   for pdf_file in os.listdir(pdf_dir):
       if pdf_file.endswith(".pdf"):
           input_pdf_path = os.path.join(pdf_dir, pdf_file)
           output_pdf_path = os.path.join(output_dir, pdf_file)
           add_text_to_pdf(input_pdf_path, output_pdf_path, text_to_add)

   print("Text added to all PDFs.")
   ```

3. **Create a watermark.pdf**:
   Create a PDF file with the text "ID: XYZ123" placed where you want it to appear on your invoices. You can use tools like Adobe Acrobat, online PDF editors, or even reportlab in Python to create this watermark file.

This script will add the specified text to all PDFs in a given directory. Make sure to replace `"/path/to/your/pdfs"` and `"/path/to/output/pdfs"` with your actual directories.
