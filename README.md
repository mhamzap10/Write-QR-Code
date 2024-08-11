# **Writing QR Codes with IronQR**

The IronQR library provides a powerful and flexible API for generating and manipulating QR codes within .NET applications. This technical documentation will provide a comprehensive overview of the "Writing QR Codes" functionalities available in IronQR. We will cover everything from basic QR code generation to advanced customization, including styling, encoding, and fault tolerance.

This documentation is structured to give developers a clear understanding of how to utilize IronQR's features effectively. Each section is accompanied by practical code examples to demonstrate real-world usage.

## **Table of Contents**

1. **Write to Document Types**
    - System.Drawing Images
    - IronDrawing Images (AnyBitmap)
    - Images (jpg, png, gif, tiff, bmp)
    - Stream (MemoryStream, byte\[\])
    - PDF (Stamp on Existing PDF)
2. **Encoding Data in QR Codes**
    - Text, URLs, Bytes, and Numbers
3. **Styling QR Codes**
    - Resizing
    - Margins & Borders
    - Recoloring
    - Adding Logos to QR Codes
4. **Checking Fault Tolerance**
    - Choosing QR Version
    - Null Checking
    - Checksums
    - Format Awareness
    - Detailed Error Messages
    - Custom QR Error Correction

### **Install with NuGet Package Manager Console**

Use the following command to install the IronQR Library
```
Install-Package IronOcr
```

## **1\. Write to Document Types**

IronQR allows QR codes to be written to various document types, including popular image formats, PDF documents, and streams. This flexibility ensures that QR codes can be integrated into a wide range of applications.

### **System.Drawing Images**

QR codes can be created as System.Drawing.Image objects, allowing further manipulation using the System.Drawing library:
```
using System.Drawing;
using IronQr;


QrCode qrCode = QrWriter.Write("Example Data");
Bitmap qrImage = qrCode.Save();
```

### **IronDrawing Images (AnyBitmap)**

The AnyBitmap format is a flexible image type within IronDrawing, compatible with various drawing libraries. QR codes can be saved as AnyBitmap images for advanced graphic manipulation:

```
using IronQr;
using IronSoftware.Drawing;


QrCode qrCode = QrWriter.Write("Example Data");
AnyBitmap qrImage = qrCode.Save();
```

### **Images (jpg, png, gif, tiff, bmp)**

IronQR supports generating QR codes directly as image files in various formats. The following code example demonstrates how to create a QR code and save it as a PNG image:
```
QrCode qrCode = QrWriter.Write("Example Data");
AnyBitmap qrImage = qrCode.Save();
qrImage.SaveAs("myQR.png");
```

### **Stream (MemoryStream, byte\[\])**

For scenarios where QR codes need to be written to a stream, such as in web applications, IronQR supports writing to MemoryStream or byte arrays:
```
QrCode qrCode = QrWriter.Write("Example Data");
AnyBitmap qrImage = qrCode.Save();
MemoryStream memoryStream = qrImage.ToStream();
Byte[] data = memoryStream.ToArray();
```

### **PDF (Stamp on Existing PDF)**

IronQR also allows QR codes to be stamped onto existing PDF documents. This is particularly useful for adding QR codes to invoices, tickets, or other official documents:
```
QrCode qrCode = QrWriter.Write("<https://ironsoftware.com/csharp/qr/>");
qrCode.StampToExistingPdfPage("SamplePDF.pdf", 100, 100, 1);
```

The '**StampToExistingPdfPage**' method in IronQR allows you to embed a QR code into an existing PDF document. It requires the file path to the PDF (filePath), the x and y coordinates (x, y) specifying where the QR code should be positioned on the page, and the page number (pageNumber) where the QR code will be stamped. This method is useful for adding QR codes to PDFs for purposes like validation, tracking, or providing quick access to online resources.

![image](https://github.com/user-attachments/assets/affe6e51-3aa9-4020-9a7a-606717de6307)


## **2\. Encoding Data in QR Codes**

IronQR supports encoding a variety of data types into QR codes, including text, URLs, byte arrays, and numerical values.

### **Text, URLs, Bytes, and Numbers**

- **Text & URLs:** The most common use case for QR codes is encoding text or URLs. The following example shows how to encode a simple text string:  
```
QrCode qrCode = QrWriter.Write("<https://ironsoftware.com/csharp/qr/>");
AnyBitmap qrCodeImage = qrCode.Save();
qrCodeImage.SaveAs("qrCode.png");
```

- **Byte Arrays:** For binary data, QR codes can also encode byte arrays:  
```
byte[] binaryData = Encoding.UTF8.GetBytes("Binary Data Example");
QrCode qrCode = QrWriter.Write(binaryData);
```
 - **Numbers:** Numerical data can be encoded efficiently, making QR codes ideal for serial numbers, IDs, or other numeric information:  
    QrCode qrCode = QrWriter.Write("1234567890");

## **3\. Styling QR Codes**

To customize the appearance of your QR code, you can use the QrStyleOptions class provided by IronQR. This class allows you to adjust various aspects of the QR code, such as dimensions, margins, colors, and even adding a logo to the center of the QR code.

Here is an example of how to style a QR code with a custom logo and specific dimensions:
```
using IronQr;

using IronSoftware.Drawing;

QrCode qrCode = QrWriter.Write("1234567890");

QrStyleOptions style = new QrStyleOptions

{

Dimensions = 300, // px

Margins = 10, // px

Color = Color.Gray,

Logo = new QrLogo

{

Bitmap = new AnyBitmap("logo.png"),

Width = 100,

Height = 100,

CornerRadius = 2

}

};

AnyBitmap qrImage = qrCode.Save(style);

qrImage.SaveAs("qrImage.png");
```

**'QrStyleOptions'** in IronQR is a configuration class that allows customization of a QR code's visual appearance. It includes properties like Dimensions to set the QR code size, Margins for spacing around the code, Color for the QR code's color, and Logo to embed an image, such as a company logo, within the QR code. This enables users to create branded or aesthetically customized QR codes.

![image](https://github.com/user-attachments/assets/4ae736ac-b225-419a-b5c9-fcd567580c32)


## **4\. Checking Fault Tolerance**

QR codes include error correction capabilities, which allow them to be scanned even if part of the code is damaged or obscured. IronQR provides QrOptions class to check and manage these capabilities.

### **Choosing QR Version**

IronQR allows developers to select the QR code version, which determines the size and data capacity:
```
QrOptions qrOptions = new QrOptions();
qrOptions.Version = 10;

QrCode qrCode = QrWriter.Write("<https://ironsoftware.com/csharp/qr/>", qrOptions);
AnyBitmap qrImage = qrCode.Save();
qrImage.SaveAs("qrImage_version10.png");
```

The '**QrOptions'** class in IronQR provides configuration settings for generating QR codes, including options like Version, which determines the size and data capacity of the QR code. The Version property ranges from 1 to 40, with higher versions allowing more data but resulting in a larger, more complex QR code. In this example, setting Version = 10 creates a QR code with increased capacity and size, which is then saved as "qrImage_version10.png".

### **QR Error Correction**

IronQR allows developers to customize the level of error correction, which is crucial for QR codes used in environments where they might be damaged:
```
QrOptions qrOptions = new QrOptions();
qrOptions.ErrorCorrectionLevel = QrErrorCorrectionLevel.Medium;
```

The QrOptions class in IronQR includes the ErrorCorrectionLevel property, which specifies the QR code's error correction capability. Setting ErrorCorrectionLevel = QrErrorCorrectionLevel.Medium configures the QR code to use medium error correction, allowing it to recover up to 15% of data if the code is damaged. QrErrorCorrectionLevel is an enumeration that defines different levels of error correction (Low, Medium, High, Highest) to balance data capacity and resilience against damage.

![image](https://github.com/user-attachments/assets/618463dd-e7eb-49d9-aa8e-98f326ebab77)


### **Checksums & Format Awareness**

IronQR automatically handles checksums and ensures that the QR code format is valid and compatible with standard QR readers.

### **Detailed Error Messages**

In case of errors, IronQR provides detailed messages to help developers debug and resolve issues efficiently.

## **Conclusion:**

In conclusion, IronQR provides a comprehensive set of methods for generating and customizing QR codes within .NET applications. Its comprehensive features allow developers to create QR codes with various data encodings, visual styles, and error correction levels. By supporting diverse output formats and integration into existing documents, IronQR enhances the flexibility and functionality of QR code generation. Whether you need basic QR codes or advanced, branded solutions, IronQR offers the necessary capabilities to meet your requirements efficiently.
