# Data Formatting Guidelines

Proper data formatting is crucial for successful data submissions on Hyvve. This guide explains how to format your data to pass verification and maximize your rewards.

## General Requirements

Regardless of the data type you're submitting, the following requirements apply:

1. **Authenticity**: All data must be authentic and original. Plagiarized or artificially generated content will be rejected by our AI verification system.
2. **Quality**: Data must be high-quality, well-formatted, and free of errors or inconsistencies.
3. **Relevance**: Submissions must be relevant to the campaign's specific requirements.
4. **Consent**: You must have the right to submit the data. Never submit confidential, private, or copyrighted data without proper permissions.

## Text Data Guidelines

### Plain Text

For campaigns requiring plain text submissions:

```
[Your text content here]
```

**Requirements:**

- Minimum word count as specified in the campaign
- Proper grammar and spelling
- No placeholder or lorem ipsum text
- No excessive formatting or special characters

### Structured Text (JSON)

For campaigns requiring structured text in JSON format:

```json
{
  "title": "Sample Title",
  "description": "This is a sample description providing context for the data.",
  "content": "The main content of your submission goes here...",
  "metadata": {
    "language": "English",
    "category": "Technology",
    "tags": ["AI", "Data", "Blockchain"]
  },
  "timestamp": "2023-06-15T14:30:00Z"
}
```

**Requirements:**

- Valid JSON format (use a validator to check)
- All required fields as specified in the campaign
- Consistent data types for each field
- Well-structured and properly nested objects

### CSV Data

For campaigns requiring structured data in CSV format:

```
id,name,age,occupation,income
1,John Doe,35,Software Engineer,95000
2,Jane Smith,42,Data Scientist,105000
3,Robert Jones,28,Teacher,65000
```

**Requirements:**

- First row must be a header with column names
- Consistent delimiter usage (usually comma)
- No missing values unless explicitly allowed
- String values with commas must be properly quoted

## Image Data Guidelines

### Image Requirements

When submitting image data:

**Technical Requirements:**

- **Formats**: JPG, PNG, or WEBP (as specified in the campaign)
- **Resolution**: Minimum 1280x720 pixels (or as specified)
- **Size**: Maximum 10MB per image (or as specified)
- **Aspect ratio**: As specified in the campaign

**Quality Requirements:**

- Well-lit, in-focus images
- No visible watermarks or text overlays (unless requested)
- No heavy filters or excessive editing
- No borders or frames (unless requested)

### Image Metadata

Some campaigns may require specific EXIF data or metadata. If requested, ensure your images contain:

- Geolocation data (if required and permitted)
- Date and time information
- Camera settings (if relevant)
- Description tags

## Multimodal Data Guidelines

For campaigns requiring combinations of text and images:

### Text + Image Pairs

```json
{
  "image_filename": "sample_image.jpg",
  "description": "A detailed description of what the image shows...",
  "labels": ["label1", "label2", "label3"],
  "context": "Additional context about when or where the image was captured"
}
```

**Requirements:**

- Image file must meet image guidelines above
- Text must accurately describe or relate to the image
- All required fields must be completed according to campaign specifications

## Common Verification Failures

Here are common reasons why submissions fail verification:

1. **Low Quality**:

   - Blurry or low-resolution images
   - Text with grammatical errors or poor coherence
   - Incomplete information

2. **Authenticity Issues**:

   - AI-generated content disguised as human-created
   - Duplicated submissions within the same campaign
   - Plagiarized content from external sources

3. **Formatting Problems**:

   - Incorrect file formats
   - Improperly structured JSON or CSV data
   - Missing required fields or metadata

4. **Content Policy Violations**:
   - Inappropriate or offensive content
   - Private or personal information without consent
   - Copyrighted material without permission

## How to Pass AI Verification

Our AI verification system analyzes your submissions for quality, authenticity, and compliance with campaign requirements. To maximize your chances of passing verification:

### For Text Verification:

1. **Write Naturally**: Our AI detects patterns of artificially generated text. Write naturally and authentically.
2. **Be Specific**: Include specific details relevant to the topic rather than generic information.
3. **Check Grammar**: Use grammar checking tools to ensure your text is error-free.
4. **Follow Structure**: Adhere to any structural requirements specified in the campaign.

### For Image Verification:

1. **Ensure Clarity**: Submit clear, well-focused images.
2. **Meet Specifications**: Verify that your images meet the technical requirements.
3. **Original Content**: Submit original photos rather than stock images or AI-generated visuals.
4. **Check EXIF Data**: If required, ensure EXIF data is present and accurate.

## Example Submission Workflow

Here's a step-by-step guide to formatting and submitting data:

1. **Review Campaign Requirements**: Carefully read all requirements and specifications.
2. **Prepare Your Data**: Format your data according to the guidelines above.
3. **Self-Verify**: Check your data for errors, inconsistencies, or formatting issues.
4. **Submit Through Platform**: Upload your data through the Hyvve platform interface.
5. **Monitor Verification Status**: Wait for the AI verification process to complete.
6. **Review Feedback**: If your submission fails verification, review the feedback provided.
7. **Make Corrections**: Address any issues and resubmit if allowed by the campaign rules.

## Advanced Formatting

For advanced users or specific campaign types, you may need to:

### Annotated Image Data

For image annotation campaigns:

```json
{
  "image_filename": "street_scene.jpg",
  "annotations": [
    {
      "type": "bounding_box",
      "label": "car",
      "coordinates": [100, 150, 300, 250]
    },
    {
      "type": "bounding_box",
      "label": "pedestrian",
      "coordinates": [420, 170, 480, 310]
    },
    {
      "type": "polygon",
      "label": "road",
      "points": [
        [0, 350],
        [640, 350],
        [640, 480],
        [0, 480]
      ]
    }
  ]
}
```

### Time-Series Data

For campaigns requiring time-series data:

```json
{
  "device_id": "sensor-001",
  "data_points": [
    {
      "timestamp": "2023-06-15T14:00:00Z",
      "temperature": 22.5,
      "humidity": 45.2
    },
    {
      "timestamp": "2023-06-15T14:05:00Z",
      "temperature": 22.7,
      "humidity": 45.0
    },
    {
      "timestamp": "2023-06-15T14:10:00Z",
      "temperature": 23.0,
      "humidity": 44.8
    }
  ],
  "metadata": {
    "location": "Building A, Room 101",
    "sensor_type": "Environmental"
  }
}
```

## Tools and Resources

To help you prepare your data:

- **JSON Validators**: [JSONLint](https://jsonlint.com/)
- **CSV Editors**: [CSV Editor Online](https://www.convertcsv.com/csv-editor.htm)
- **Image Editors**: [GIMP](https://www.gimp.org/) (free), [Photopea](https://www.photopea.com/) (online)
- **Image Metadata Viewers**: [EXIF Viewer](https://exifdata.com/)

## Need Help?

If you're struggling with data formatting:

1. Check the campaign's FAQ section
2. Refer to example submissions if provided
3. Contact the campaign creator through the platform
4. Visit our [community forum](https://community.hyvve.io) for peer support
