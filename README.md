To add the text "Xyz123" to a bunch of PDF files in the specified directory using PyPDF2, you can follow the steps below. This involves reading each PDF file, modifying it to include the desired text, and then saving the modified PDF.

Steps to Add Text to PDFs Using PyPDF2
1. Install Required Libraries
Ensure you have PyPDF2 and reportlab installed. While PyPDF2 is used for reading and writing PDFs, reportlab is used to create new PDF content (e.g., adding text).

bash
Copy code
pip install PyPDF2 reportlab
2. Python Script to Add Text to PDFs
Here's a Python script that iterates over all PDF files in the specified directory, adds the text "Xyz123" to each PDF, and saves the modified PDFs to a new directory.

python
Copy code
import os
from PyPDF2 import PdfReader, PdfWriter
from reportlab.pdfgen import canvas
from reportlab.lib.pagesizes import letter
from io import BytesIO

def add_text_to_pdf(input_pdf_path, output_pdf_path, text, x=100, y=100):
    # Create a PDF with the text to add
    packet = BytesIO()
    can = canvas.Canvas(packet, pagesize=letter)
    can.drawString(x, y, text)
    can.save()

    # Move the packet's data to the beginning
    packet.seek(0)

    # Read the created PDF
    new_pdf = PdfReader(packet)

    # Read the existing PDF
    existing_pdf = PdfReader(input_pdf_path)
    output = PdfWriter()

    # Add the text to each page of the original PDF
    for page_number in range(len(existing_pdf.pages)):
        page = existing_pdf.pages[page_number]

        # Merge the new PDF (with text) onto the existing page
        page.merge_page(new_pdf.pages[0])

        # Add the modified page to the output
        output.add_page(page)

    # Write the final PDF to a file
    with open(output_pdf_path, "wb") as outputStream:
        output.write(outputStream)

def process_pdfs(input_directory, output_directory, text):
    # Ensure the output directory exists
    os.makedirs(output_directory, exist_ok=True)

    # Process each PDF file in the input directory
    for filename in os.listdir(input_directory):
        if filename.endswith(".pdf"):
            input_pdf_path = os.path.join(input_directory, filename)
            output_pdf_path = os.path.join(output_directory, filename)

            print(f"Processing {filename}...")

            # Add text to the PDF
            add_text_to_pdf(input_pdf_path, output_pdf_path, text)

# Define the input and output directories
input_directory = r"C:\Users\Admin\Desktop\Bunch PDF"
output_directory = r"C:\Users\Admin\Desktop\Bunch PDF\Modified"
text_to_add = "Xyz123"

# Process all PDFs in the input directory
process_pdfs(input_directory, output_directory, text_to_add)
Explanation
PDF Creation:

A new PDF is created using reportlab to overlay the text on the original PDF.
canvas.Canvas is used to draw text on the new PDF.
Merge Pages:

PyPDF2 is used to read each page of the original PDF and merge it with the page from the new PDF containing the text.
Output Directory:

The script writes the modified PDFs to a separate directory (Modified) within the input directory.
How to Run the Script
Save the Script:

Copy the above script into a file named add_text_to_pdfs.py.
Run the Script:

Open a terminal or command prompt.

Navigate to the directory where the script is saved.

Run the script using the command:

bash
Copy code
python add_text_to_pdfs.py
Check Output:

The modified PDFs will be saved in the Modified folder within the original directory.
Adjustments
Text Position: You can adjust the x and y coordinates in add_text_to_pdf to change the position of the text on the PDF pages.
Text Appearance: For more advanced text styling (e.g., font size, color), you can modify the reportlab drawing commands.
