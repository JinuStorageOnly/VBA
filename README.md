# VBA

Yes, you can use an Excel VBA (macro) to automate the process of adding text to multiple PDF files. This involves using a third-party library like Adobe Acrobat if you have it installed, or using a library like PDFtk.

Here is an example using Adobe Acrobat with VBA:

### Steps:
1. **Ensure Adobe Acrobat is installed**: This script uses Adobe Acrobat Pro’s COM interface.
2. **Enable Developer Tab in Excel**: Go to `File` > `Options` > `Customize Ribbon`, and check the `Developer` tab.
3. **Add a Reference to Adobe Acrobat in VBA**: Go to `Developer` > `Visual Basic` > `Tools` > `References`, and check `Adobe Acrobat xx.x Type Library`.

### VBA Script to Add Text to PDFs:
```vba
Sub AddTextToPDFs()
    Dim folderPath As String
    Dim pdfFile As String
    Dim app As Object
    Dim avDoc As Object
    Dim pdDoc As Object
    Dim jso As Object
    Dim i As Integer
    
    ' Set the folder path containing the PDFs
    folderPath = "C:\path\to\your\pdfs\"
    
    ' Create Acrobat application object
    Set app = CreateObject("AcroExch.App")
    
    ' Get the first PDF file in the folder
    pdfFile = Dir(folderPath & "*.pdf")
    
    ' Loop through all PDF files in the folder
    Do While pdfFile <> ""
        ' Open the PDF
        Set avDoc = CreateObject("AcroExch.AVDoc")
        If avDoc.Open(folderPath & pdfFile, "") Then
            Set pdDoc = avDoc.GetPDDoc
            Set jso = pdDoc.GetJSObject
            
            ' Add text to the first page (can be adjusted for all pages)
            jso.addField "TextField", "text", 0, Array(10, 10, 100, 30)
            jso.getField("TextField").value = "ID: XYZ123"
            jso.getField("TextField").readonly = True
            
            ' Save and close the PDF
            pdDoc.Save 1, folderPath & "output_" & pdfFile
            avDoc.Close True
        End If
        
        ' Get the next PDF file
        pdfFile = Dir
    Loop
    
    ' Quit Acrobat
    app.Exit
    Set app = Nothing
End Sub
```

### Explanation:
1. **Folder Path**: Set the path where your PDF files are stored.
2. **Create Acrobat Object**: The script creates an instance of Adobe Acrobat.
3. **Open and Edit PDFs**: The script opens each PDF, adds a text field with the desired text, and saves the edited PDF with a new name.
4. **Loop Through PDFs**: The script processes each PDF in the specified folder.

### Running the Macro:
1. Open Excel and press `Alt + F11` to open the VBA editor.
2. Insert a new module (`Insert > Module`) and paste the script into the module.
3. Modify the `folderPath` to the location of your PDFs.
4. Close the VBA editor and run the macro (`Alt + F8`, select `AddTextToPDFs`, and click `Run`).

This method leverages Adobe Acrobat's capabilities via VBA. If you don't have Adobe Acrobat Pro, you might need a different approach using another PDF processing tool that provides a COM interface.
