# Traffic Sign OCR Annotation with Bounding Boxes

## Project Overview

This project demonstrates traffic sign text detection and Optical Character Recognition (OCR) annotation using bounding boxes. The dataset is designed to help computer vision models detect text-bearing traffic signs, locate text regions and recognize the text displayed on road signs.

The sample image contains multiple street name signs, directional sign panels and hotel information signs in an urban environment.

**Annotation type:** Bounding Box + Text Transcription  
**Task:** Traffic Sign Text Detection and OCR  
**Primary objective:** Create accurate, structured annotations for OCR model training and evaluation.  
**Export format:** JSON  
**Annotation tool:** CVAT or another compatible bounding box annotation tool

## Objectives

- Detect traffic signs containing readable text.
- Draw accurate bounding boxes around individual sign panels.
- Transcribe the visible text exactly as it appears.
- Preserve the original capitalization, spelling and punctuation.
- Export annotations in a consistent JSON structure.
- Maintain high annotation quality for computer vision applications.

## Annotation Guidelines

### 1. Identify eligible objects

Annotate individual traffic sign panels or information panels containing readable or partially readable text.

In the reference image, eligible examples include:

- `Glattpark`
- `2`
- `Flughafenstrasse`
- `Dufaux-Strasse`
- `Kamera Grand Hotel`
- `Radisson Hotel & Suites`

These examples represent visible text in the reference image. The actual transcription must always be verified against the original image.

**Do not annotate:**
- Traffic signs that contain only symbols and no text, such as the yellow diamond priority-road sign.
- Buildings, vehicles, streetlights or other background objects.
- Text on buildings or advertisements that is not part of an eligible sign panel.
- Unrelated road markings.

### 2. Draw bounding boxes

Draw one rectangular bounding box around each individual sign panel containing text.

**Bounding box rules:**

1. Include the complete sign panel containing the text.
2. Keep the box as tight as possible around the panel's outer edges.
3. Include all visible text within that panel.
4. Exclude nearby sign panels, poles, trees and background objects.
5. Do not create separate boxes for individual words when they belong to the same panel.
6. Annotate adjacent or stacked sign panels separately.
7. Follow the visible panel boundaries, even when the sign is tilted or photographed at an angle.
8. Do not extend boxes beyond the sign boundaries to include shadows or surrounding scenery.

For example, `Dufaux-Strasse` should receive its own bounding box, separate from `Flughafenstrasse` and the hotel information panels below it.

### 3. Transcribe the text

For every annotated sign panel, enter the visible text exactly as displayed.

**Transcription rules:**

- Preserve uppercase and lowercase letters.
- Preserve spaces, hyphens, ampersands and other visible punctuation.
- Keep numbers as displayed.
- Do not translate text into another language.
- Do not correct spelling or rewrite the text.
- Do not infer words hidden by trees, glare or other objects.
- Do not add descriptions such as "street sign" to the transcription field.
- Preserve the original reading order for text spanning multiple lines.

If a panel contains multiple lines, combine them using a newline character (`\n`) in the text field unless the project specification requires another format.

### 4. Handle unclear or partially visible text

- Annotate a sign if some of its text is readable.
- Transcribe only the clearly identifiable text when the project permits partial transcription.
- Use an empty transcription or a predefined uncertainty flag if the text cannot be reliably read.
- Do not guess missing characters.
- Record occlusion or readability issues in the relevant metadata field when required.

For consistency, use the same uncertainty policy across the entire dataset.

## Annotation Class

Use the following class for this project:

| Class | Description |
|---|---|
| `traffic_sign_text` | A text-bearing traffic sign panel or directional information panel |

The class describes the annotated object. The `text` field contains the actual OCR transcription.

If a future project requires separate categories for street names, directional signs or informational signs, introduce those categories only after updating the annotation specification.

## Bounding Box Coordinate Format

Use the `xyxy` format in the JSON deliverable.

| Field | Description |
|---|---|
| `x_min` | Left coordinate of the bounding box |
| `y_min` | Top coordinate of the bounding box |
| `x_max` | Right coordinate of the bounding box |
| `y_max` | Bottom coordinate of the bounding box |

Coordinates must be measured in pixels relative to the original image, with the top-left corner as `(0, 0)`.

The following conditions must always be satisfied:

- `x_min < x_max`
- `y_min < y_max`
- All coordinates must remain within the image boundaries.
- Coordinates must correspond to the original image dimensions.

Do not resize the image before annotating unless the project specification explicitly allows it.

## JSON Annotation Structure

The following example shows a recommended JSON structure for this project. The bounding box coordinates are illustrative placeholders and must be replaced with the actual coordinates obtained from the annotation tool.

```json
{
  "image": {
    "file_name": "traffic_sign_001.jpg",
    "width": 319,
    "height": 563
  },
  "annotations": [
    {
      "id": 1,
      "class": "traffic_sign_text",
      "bbox": [150, 145, 218, 205],
      "bbox_format": "xyxy",
      "text": "Glattpark",
      "attributes": {
        "occluded": false,
        "readability": "clear"
      }
    },
    {
      "id": 2,
      "class": "traffic_sign_text",
      "bbox": [150, 145, 218, 205],
      "bbox_format": "xyxy",
      "text": "2",
      "attributes": {
        "occluded": false,
        "readability": "clear"
      }
    },
    {
      "id": 3,
      "class": "traffic_sign_text",
      "bbox": [80, 208, 153, 250],
      "bbox_format": "xyxy",
      "text": "Flughafenstrasse",
      "attributes": {
        "occluded": false,
        "readability": "clear"
      }
    }
  ]
}
```

**Important:** The example coordinates are not verified annotations. Replace them with measured coordinates before using the JSON as a dataset deliverable. The `Glattpark` and `2` entries illustrate two possible text regions within one panel. If the project defines the entire panel as the detection unit, use one box for the panel and store its multiline transcription in a single annotation instead. Do not mix these approaches within one dataset.

## Quality Assurance Checklist

Before exporting the annotations, verify the following:

- [ ] Every eligible text-bearing sign panel has been annotated.
- [ ] Each bounding box tightly covers its intended sign panel.
- [ ] Adjacent sign panels have separate annotations.
- [ ] Transcriptions match the visible text.
- [ ] Capitalization, punctuation and spelling are preserved.
- [ ] Unreadable or partially obscured text follows the agreed policy.
- [ ] No unrelated background objects are annotated.
- [ ] Bounding box coordinates are valid and within image dimensions.
- [ ] Annotation IDs are unique within each image.
- [ ] JSON syntax is valid and can be parsed.
- [ ] The exported file matches the client's required schema.

## Expected Deliverables

The final dataset should contain:

1. **Original images:** The source images used for annotation.
2. **JSON annotations:** One JSON file per image or a combined JSON file, depending on client requirements.
3. **Bounding boxes:** Pixel coordinates for each annotated sign panel.
4. **OCR transcriptions:** Exact text associated with each bounding box.
5. **Quality-checked output:** Annotations reviewed for completeness, coordinate accuracy and transcription consistency.

## Validation Requirements

The JSON output must be valid, machine-readable and consistent across all images.

A validation script should check:

- Required keys are present.
- Bounding box coordinates are numeric.
- Bounding box dimensions are positive.
- Coordinates do not exceed image dimensions.
- Annotation IDs are unique.
- Text fields use the required data type.
- The file can be parsed by a standard JSON parser.

For client delivery, the annotation schema should be confirmed before production. A valid JSON file is not automatically compatible with every OCR training pipeline. Some clients require COCO, YOLO, ICDAR or a custom schema.

## Applications

This annotation approach can support:

- Traffic sign text detection
- OCR model training and evaluation
- Road scene understanding
- Intelligent transportation systems
- Navigation and mapping applications
- Autonomous driving research

## About Annomax AI

Annomax AI focuses on structured AI data annotation and quality assurance for computer vision and machine learning applications.

This sample demonstrates our approach to bounding box annotation, text transcription, dataset consistency and quality validation.

**Precision Annotation That Trains Better AI.**
