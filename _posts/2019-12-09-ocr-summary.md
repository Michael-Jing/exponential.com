---
layout: post
title:  "OCR Summary"
date:   2019-12-09 16:33:05 +0800
categories: jekyll update
---
# Service Providers & Libraries
## Service Providers
* Alibaba (Table Recognition & Recognition based on custom templates)
* Google (Text Detection && Document Text Detection, table recognition in alpha phase)
* Amazon: (Textract, with table recognition capability)
* [ABBYY]:
* Ocrolus

## Open Source Libraries
* tesseract
* gocr
* Apache Tika
* FreeOCR(based on tesseract engine)

# Compare OCR Results
* Google(26)
* Amazon(63)
* Tesseract(73)
* Konvergen(74)

# Development of MLOCR Service (choose google ocr as the backend service provider)
## EndPoint
* document
* bulk
* async post/get

## Process
1. get file from client
2. upload file to google cloud storage
3. submit ocr task
4. fetch results from google cloud storage
5. convert to konvergen format

## CICD
1. Dockerfile
1. k8s
2. Spinnaker
3. Automate scripts

## Issues
1. Pod got killed when gunicorn is too busy to respond to health probe
2. Process got killed when memory exceeded limit (if you can't find clue in application log, check dmseg)
3. Asynronized worker is the best to choose

## A little page to visualize the difference between ocr results



[ABBYY]: https://www.youtube.com/83c53637-f2e9-4fb0-85b0-7457b5fccf59
