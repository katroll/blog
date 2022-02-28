---
title: "Importing Excel File with Pictures to React"
date: 2022-02-28T15:35:29-05:00
draft: false
---


As my final capstone project for the Flatiron School boot camp I am working on building a testing app for an Aunt of mine who runs a small computer training school in Kolkata, India.

Part of the functionality of the website is for her to be able to upload prewritten test files in excel, containing image columns, to create the tests. After doing some google searching and research I found the ExcelJS package, which makes the process quite simple. 

To begin, install ExcelJS to your react project: 

    npm install exceljs

Then import it to your component:

    import * as Excel from "exceljs";

To create an upload button for the user interface, I used  an <input> html tag with `type=”file”`. You will need an onChange event handler to recognize when a file has been uploaded.

    <label>Upload: </label>
    <input 
        type="file" 
        id="upload" 
        name="upload" 
        onChange={loadQuizFile} />

From there, we need to write the event handler `loadQuizFile()`:

    function loadQuizFile(e) {
        const selectedFile = e.target.files[0];
        const wb = new Excel.Workbook();
        const reader = new FileReader();

        reader.readAsArrayBuffer(selectedFile)

        reader.onload = () => {
            const buffer = reader.result;
            wb.xlsx.load(buffer).then(workbook => {
                workbook.eachSheet((sheet, id) => {
                    const quizQuestions = [];
                    sheet.eachRow((row, rowIndex) => {
                        if(rowIndex === 1 ) return;                        
                        const questionObj = {
                            question: row.values[1],
                            bengali: row.values[2],
                            choices: [row.values[3], row.values[4], row.values[5], row.values[6]],
                            answer: row.values[7],
                            number: rowIndex - 1
                            };
                        quizQuestions.push(questionObj);
                    })

                    const images = sheet.getImages();
                    images.forEach(image => {
                        const row = image.range.tl.row;
                        const img = workbook.model.media.find(m => m.index === image.imageId);
                        const imgBase64 = Base64.encode(img.buffer);
                        quizQuestions[row - 1].imageBase64 = imgBase64;
                    })
                    setQuestions(quizQuestions);
                })
            })
        }
    }


This function first sets the uplaoded data to a `const` variable. Then, using ExcelJS, creates a new excel workbook and `FileReader`.

It then instructs the reader to read the data as a ArrayBuffer which is a way to store binary data. The `reader.onLoad()` function runs upon completion of the `FilerReader` proccessing the data.

This is where each sheet is looped over, and each row is looped over within. I am only working on uploading excel files with one sheet, so there is nothing do to for each sheet. Just above the rows loop I define a `const` variable to collect data in the correct form to save to my database. 

The next part is where the image collecting happens. With each image attatched to a cell in the excel file, ExcelJs can load those images and their locations into javascript. 

I do a similar thing with the images as I did with the sheets: loop over each one, find the row they below to, and add the image to the correct object. 

My `quizQuestions` array is being built in a way that it is ready to be sent to my rails database, so I need to make sure that the image is in a compatible format to do so. 

This can be done with the  `base64-arraybuffer` package, installed to a project with:

    npm install base64-arraybuffer

Then importing it to your component with: 

    import * as Base64 from "base64-arraybuffer"

This package has to functions: `encode(buffer)` and `decode(string)`. With these fuctions I am able to encode the ArrayBuffer and store it as a string in my rails database table. Then using `decode()`, I can convert a string to an ArrayBuffer and then create an imageUrl upon loading a table into my app. 

And with that one funciton my file loader event handler can load an excel sheet into my React app!


