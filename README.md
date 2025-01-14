<!DOCTYPE html>
<html lang="hi">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="+2 उतक्रमित माध्यमिक विद्यालय भालपट्टी: शिक्षा के क्षेत्र में उत्कृष्टता।">
    <meta name="keywords" content="भालपट्टी, स्कूल, शिक्षा, माध्यमिक विद्यालय, दरभंगा">
    <meta name="author" content="+2 उतक्रमित माध्यमिक विद्यालय भालपट्टी">
    <title>+2 उतक्रमित माध्यमिक विद्यालय भालपट्टी</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f9f9f9;
        }

        header {
            background-color: #004080;
            color: white;
            padding: 15px 0;
            text-align: center;
        }

        nav ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
            display: flex;
            justify-content: center;
            background-color: #0066cc;
        }

        nav ul li {
            margin: 0 15px;
        }

        nav ul li a {
            color: white;
            text-decoration: none;
            font-weight: bold;
        }

        nav ul li a:hover {
            text-decoration: underline;
        }

        main {
            padding: 20px;
        }

        section {
            margin-bottom: 40px;
        }

        footer {
            background-color: #004080;
            color: white;
            text-align: center;
            padding: 10px 0;
            position: relative;
            bottom: 0;
            width: 100%;
        }

        .gallery img {
            width: 100%;
            max-width: 300px;
            margin: 10px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
        }

        .cta {
            text-align: center;
            margin-top: 20px;
        }

        .cta a {
            background-color: #ff6600;
            color: white;
            padding: 10px 20px;
            text-decoration: none;
            border-radius: 5px;
        }

        .cta a:hover {
            background-color: #e65c00;
        }

        #library {
            background-color: #f9f9f9;
            padding: 20px;
            margin-top: 20px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }

        #library h2 {
            text-align: center;
            color: #4CAF50;
        }

        #library table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }

        #library th, #library td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: center;
        }

        #library th {
            background-color: #4CAF50;
            color: white;
        }

        #library img {
            max-width: 100px;
            height: auto;
        }

section {
    margin: 20px;
    padding: 15px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: #f9f9f9;
}
<?php
if ($_SERVER['REQUEST_METHOD'] == 'POST' && isset($_FILES['pdfFile'])) {
    $uploadDir = 'uploads/'; // Directory jahan file store hogi
    $uploadFile = $uploadDir . basename($_FILES['pdfFile']['name']);

    // Ensure upload directory exists
    if (!is_dir($uploadDir)) {
        mkdir($uploadDir, 0777, true);
    }

    // Check if the file is a PDF
    if (mime_content_type($_FILES['pdfFile']['tmp_name']) == 'application/pdf') {
        if (move_uploaded_file($_FILES['pdfFile']['tmp_name'], $uploadFile)) {
            echo "The file " . basename($_FILES['pdfFile']['name']) . " has been uploaded successfully!";
        } else {
            echo "Error uploading your file. Please try again.";
        }
    } else {
        echo "Only PDF files are allowed.";
    }
} else {
    echo "No file uploaded.";
}
?>

    </style>
</head>

<body>
    <header>
        <h1>+2 उतक्रमित माध्यमिक विद्यालय भालपट्टी</h1>
        <p>शिक्षा के क्षेत्र में उत्कृष्टता</p>
    </header>

    <nav>
        <ul>
            <li><a href="#about">हमारे बारे में</a></li>
            <li><a href="#admissions">प्रवेश</a></li>
            <li><a href="#facilities">सुविधाएं</a></li>
            <li><a href="#gallery">गैलरी</a></li>
            <li><a href="#contact">संपर्क करें</a></li>
        </ul>
    </nav>

    <main>
        <section id="about">
            <h2>हमारे बारे में</h2>
            <p>+2 उतक्रमित माध्यमिक विद्यालय भालपट्टी का स्थापना 1995 में हुआ था। यह स्कूल शिक्षा के क्षेत्र में अपने उच्च मानकों और समर्पित शिक्षकों के लिए जाना जाता है।</p>
        </section>

        <section id="admissions">
            <h2>प्रवेश प्रक्रिया</h2>
            <p>प्रवेश के लिए ऑनलाइन आवेदन करें। अधिक जानकारी के लिए नीचे दिए गए संपर्क पर संपर्क करें।</p>
        </section>

        <section id="facilities">
            <h2>हमारी सुविधाएं</h2>
            <ul>
                <li>सुसज्जित पुस्तकालय</li>
                <li>आधुनिक कंप्यूटर लैब</li>
                <li>खेल के विस्तृत मैदान</li>
                <li>वैज्ञानिक प्रयोगशालाएं</li>
            </ul>
        </section>
<!-- Image Upload Section -->
    <section id="upload">
        <h2>छवि अपलोड करें</h2>
        <div class="image-upload">
            <label for="file-input"></label>
            <input id="file-input" type="file" accept="image/*" onchange="previewImage(event)" />
        </div>
        <div class
 <!-- Classes Section -->
    <section id="classes">
        <h2>कक्षाएं</h2>
        <ul>
            <li>कक्षा 11 - 12: उच्च शिक्षा</li>
        </ul>
    </section>
<!-- Teachers Information Section -->
<section id="teachers-info">
    <h2>हमारे शिक्षक</h2>
    <p>हमारे शिक्षक विषय विशेषज्ञ हैं, जिनका शिक्षा क्षेत्र में वर्षों का अनुभव है।</p>
    <div class="teachers-section">
        <div class="teacher-item">
            <img src="images/teacher1.jpg" alt="संतोष सर">
            <h3>संतोष सर</h3>
            <p>विषय: भूगोल</p>
            <p>योग्यता:               ? (भूगोल)</p>
            <p>अनुभव:               ? वर्ष</p>
        </div>
        <div class="teacher-item">
            <img src="images/teacher2.jpg" alt="रंजीत सर">
            <h3>रंजीत सर</h3>
            <p>विषय: इतिहास</p>
            <p>योग्यता:              ?. (इतिहास)</p>
            <p>अनुभव:              ? वर्ष</p>
        </div>
        <div class="teacher-item">
            <img src="images/teacher3.jpg" alt="प्रिया मैम">
            <h3>प्रिया मैम</h3>
            <p>विषय: राजनीति शास्त्र</p>
            <p>योग्यता:             ?. (राजनीति शास्त्र)</p>
            <p>अनुभव:              ? वर्ष</p>
        </div>
        <div class="teacher-item">
            <img src="images/teacher4.jpg" alt="सोनू सर">
            <h3>सोनू सर</h3>
            <p>विषय: मनोविज्ञान</p>
            <p>योग्यता:            ? (मनोविज्ञान)</p>
            <p>अनुभव:             ? वर्ष</p>
        </div>
        <div class="teacher-item">
            <img src="images/teacher5.jpg" alt="डीएन सर">
            <h3>डीएन सर</h3>
            <p>विषय: कंप्यूटर साइंस</p>
            <p>योग्यता:               ? (कंप्यूटर साइंस)</p>
            <p>अनुभव:               ? वर्ष</p>
        </div>
        <div class="teacher-item">
            <img src="images/teacher6.jpg" alt="राखी मैम">
            <h3>राखी मैम</h3>
            <p>विषय: अंग्रेजी</p>
            <p>योग्यता:          🙄🙄   ? (अंग्रेजी)</p>
            <p>अनुभव:             ? वर्ष</p>
        </div>
        <div class="teacher-item">
            <img src="images/teacher7.jpg" alt="आशा मैम">
            <h3>आशा मैम</h3>
            <p>विषय: हिंदी</p>
            <p>योग्यता:            ? (हिंदी)</p>
            <p>अनुभव:            ? वर्ष</p>
        </div>
    </div>
</section>
         <!-- लाइब्रेरी सेक्शन -->
<section>
    <h2>लाइब्रेरी</h2>
    <p>यहाँ NCERT की आधिकारिक वेबसाइट का लिंक दिया गया है:</p>
    <a href="https://ncert.nic.in/" target="_blank">NCERT की वेबसाइट पर जाएं</a>
</section>
 
</section>
           
<section id="timetable">
<h2 style="text-aling: center;
font-family: arial, sans-serif;">School Timetanle</h2>
<table border="3" style="width: 100%;
border-collapse: collapse; text-align: center; font-size:16px;">
<thead style="background-color: #4CAF50;">
<tr>
<th>peiod</th>
<th>Time</th>
<th>Subject</th>
<th>teacher</th>
</tr>
</thead>
<thody>
<tr>
<td>1</td>
<td>12:20 PM - 1:00 PM</td>
<td>Geographt</td>
<td>Santosh sir</td>
</tr>
<tr style="background-color: #f2f2f2;">
<td>2</td>
<td>1:00 PM - 1:40 PM</td>
<td>computer science</td>
<td>DN sir</td>
</tr>
<td>3</td>
<td>1:40 PM - 2:20 PM</td>
<td>History</td>
<td>Ranjeet sir</td>
<tr style="background-color: #f2f2f2;">
<td>4</td>
<td>2:20 PM - 3:00 PM</td>
<td>political science</td>
<td>priya mam</td>
<tr>
<td colspan="5"style="text-align: center; background-color:  (#ffeb99)a;">Lunch Break (3:00 PM -3:30PM)</td>
</tr>
<td>5</td>
<tr style="background-color: #f2f2f2;">
<td>2</td>
<td>3:30 PM - 4:10 PM</td>
<td>English</td>
<td>Rakhi mam</td>
<tr style="background-color: #f2f2f2;">
<td>6</td>
<td>4:10 PM - 4:50 PM</td>
<td>Hindi</td>
<td>Asha Mam</td>
</tr>
</thody>
</table>
</section>
<section>
    <h2>Upload PDF</h2>
    <form action="" method="POST" enctype="multipart/form-data">
        <label for="pdfFile">Choose a PDF to upload:</label>
        <input type="file" id="pdfFile" name="pdfFile" accept="application/pdf" required>
        <button type="submit">Upload</button>
    </form>
</section>

        <section id="gallery" class="gallery">
            <h2>गैलरी</h2>
            <img src="school1.jpg" alt="स्कूल भवन">
<li></li>
<li><a href="https://ncert.nic.in/textbook.php" target="_blank">कक्षा 11 की NCERT किताबें</a></li>
            <img src="event1.jpg" alt="वार्षिक उत्सव">
            <img src="lab.jpg" alt="प्रयोगशाला">
        </section>

        <section id="contact">
            <h2>संपर्क करें</h2>
            <p>पता: भालपट्टी, दरभंगा, बिहार - 847239</p>
            <p>फोन:9504797910</p>
            <p>ईमेल: ajit91884270@gmail.com</p>
        </section>
    </main>

    <footer>
        <p>&copy; +2 उतक्रमित माध्यमिक विद्यालय भालपट्टी।</p>
    </footer>
</body>

</html>
