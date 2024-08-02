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
