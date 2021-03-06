# CompareFaces<a name="API_CompareFaces"></a>

Compares a face in the *source* input image with each of the 100 largest faces detected in the *target* input image\. 

**Note**  
 If the source image contains multiple faces, the service detects the largest face and compares it with each face detected in the target image\. 

You pass the input and target images either as base64\-encoded image bytes or as a references to images in an Amazon S3 bucket\. If you use the Amazon CLI to call Amazon Rekognition operations, passing image bytes is not supported\. The image must be either a PNG or JPEG formatted file\. 

In response, the operation returns an array of face matches ordered by similarity score in descending order\. For each face match, the response provides a bounding box of the face, facial landmarks, pose details \(pitch, role, and yaw\), quality \(brightness and sharpness\), and confidence value \(indicating the level of confidence that the bounding box contains a face\)\. The response also provides a similarity score, which indicates how closely the faces match\. 

**Note**  
By default, only faces with a similarity score of greater than or equal to 80% are returned in the response\. You can change this value by specifying the `SimilarityThreshold` parameter\.

 `CompareFaces` also returns an array of faces that don't match the source image\. For each face, it returns a bounding box, confidence value, landmarks, pose details, and quality\. The response also returns information about the face in the source image, including the bounding box of the face and confidence value\.

If the image doesn't contain Exif metadata, `CompareFaces` returns orientation information for the source and target images\. Use these values to display the images with the correct image orientation\.

If no faces are detected in the source or target images, `CompareFaces` returns an `InvalidParameterException` error\. 

**Note**  
 This is a stateless API operation\. That is, data returned by this operation doesn't persist\.

For an example, see [Comparing Faces in Images \(SDK\)](faces-compare-images.md)\.

This operation requires permissions to perform the `rekognition:CompareFaces` action\.

## Request Syntax<a name="API_CompareFaces_RequestSyntax"></a>

```
{
   "SimilarityThreshold": number,
   "SourceImage": { 
      "Bytes": blob,
      "S3Object": { 
         "Bucket": "string",
         "Name": "string",
         "Version": "string"
      }
   },
   "TargetImage": { 
      "Bytes": blob,
      "S3Object": { 
         "Bucket": "string",
         "Name": "string",
         "Version": "string"
      }
   }
}
```

## Request Parameters<a name="API_CompareFaces_RequestParameters"></a>

The request accepts the following data in JSON format\.

 ** SimilarityThreshold **   
The minimum level of confidence in the face matches that a match must meet to be included in the `FaceMatches` array\.  
Type: Float  
Valid Range: Minimum value of 0\. Maximum value of 100\.  
Required: No

 ** SourceImage **   
The input image as base64\-encoded bytes or an S3 object\. If you use the AWS CLI to call Amazon Rekognition operations, passing base64\-encoded image bytes is not supported\.   
Type: [Image](API_Image.md) object  
Required: Yes

 ** TargetImage **   
The target image as base64\-encoded bytes or an S3 object\. If you use the AWS CLI to call Amazon Rekognition operations, passing base64\-encoded image bytes is not supported\.   
Type: [Image](API_Image.md) object  
Required: Yes

## Response Syntax<a name="API_CompareFaces_ResponseSyntax"></a>

```
{
   "FaceMatches": [ 
      { 
         "Face": { 
            "BoundingBox": { 
               "Height": number,
               "Left": number,
               "Top": number,
               "Width": number
            },
            "Confidence": number,
            "Landmarks": [ 
               { 
                  "Type": "string",
                  "X": number,
                  "Y": number
               }
            ],
            "Pose": { 
               "Pitch": number,
               "Roll": number,
               "Yaw": number
            },
            "Quality": { 
               "Brightness": number,
               "Sharpness": number
            }
         },
         "Similarity": number
      }
   ],
   "SourceImageFace": { 
      "BoundingBox": { 
         "Height": number,
         "Left": number,
         "Top": number,
         "Width": number
      },
      "Confidence": number
   },
   "SourceImageOrientationCorrection": "string",
   "TargetImageOrientationCorrection": "string",
   "UnmatchedFaces": [ 
      { 
         "BoundingBox": { 
            "Height": number,
            "Left": number,
            "Top": number,
            "Width": number
         },
         "Confidence": number,
         "Landmarks": [ 
            { 
               "Type": "string",
               "X": number,
               "Y": number
            }
         ],
         "Pose": { 
            "Pitch": number,
            "Roll": number,
            "Yaw": number
         },
         "Quality": { 
            "Brightness": number,
            "Sharpness": number
         }
      }
   ]
}
```

## Response Elements<a name="API_CompareFaces_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** FaceMatches **   
An array of faces in the target image that match the source image face\. Each `CompareFacesMatch` object provides the bounding box, the confidence level that the bounding box contains a face, and the similarity score for the face in the bounding box and the face in the source image\.  
Type: Array of [CompareFacesMatch](API_CompareFacesMatch.md) objects

 ** SourceImageFace **   
The face in the source image that was used for comparison\.  
Type: [ComparedSourceImageFace](API_ComparedSourceImageFace.md) object

 ** SourceImageOrientationCorrection **   
 The orientation of the source image \(counterclockwise direction\)\. If your application displays the source image, you can use this value to correct image orientation\. The bounding box coordinates returned in `SourceImageFace` represent the location of the face before the image orientation is corrected\.   
If the source image is in \.jpeg format, it might contain exchangeable image \(Exif\) metadata that includes the image's orientation\. If the Exif metadata for the source image populates the orientation field, the value of `OrientationCorrection` is null and the `SourceImageFace` bounding box coordinates represent the location of the face after Exif metadata is used to correct the orientation\. Images in \.png format don't contain Exif metadata\.
Type: String  
Valid Values:` ROTATE_0 | ROTATE_90 | ROTATE_180 | ROTATE_270` 

 ** TargetImageOrientationCorrection **   
 The orientation of the target image \(in counterclockwise direction\)\. If your application displays the target image, you can use this value to correct the orientation of the image\. The bounding box coordinates returned in `FaceMatches` and `UnmatchedFaces` represent face locations before the image orientation is corrected\.   
If the target image is in \.jpg format, it might contain Exif metadata that includes the orientation of the image\. If the Exif metadata for the target image populates the orientation field, the value of `OrientationCorrection` is null and the bounding box coordinates in `FaceMatches` and `UnmatchedFaces` represent the location of the face after Exif metadata is used to correct the orientation\. Images in \.png format don't contain Exif metadata\.
Type: String  
Valid Values:` ROTATE_0 | ROTATE_90 | ROTATE_180 | ROTATE_270` 

 ** UnmatchedFaces **   
An array of faces in the target image that did not match the source image face\.  
Type: Array of [ComparedFace](API_ComparedFace.md) objects

## Errors<a name="API_CompareFaces_Errors"></a>

 **AccessDeniedException**   
You are not authorized to perform the action\.  
HTTP Status Code: 400

 **ImageTooLargeException**   
The input image size exceeds the allowed limit\. For more information, see [Limits in Amazon Rekognition](limits.md)\.   
HTTP Status Code: 400

 **InternalServerError**   
Amazon Rekognition experienced a service issue\. Try your call again\.  
HTTP Status Code: 500

 **InvalidImageFormatException**   
The provided image format is not supported\.   
HTTP Status Code: 400

 **InvalidParameterException**   
Input parameter violated a constraint\. Validate your parameter before calling the API operation again\.  
HTTP Status Code: 400

 **InvalidS3ObjectException**   
Amazon Rekognition is unable to access the S3 object specified in the request\.  
HTTP Status Code: 400

 **ProvisionedThroughputExceededException**   
The number of requests exceeded your throughput limit\. If you want to increase this limit, contact Amazon Rekognition\.  
HTTP Status Code: 400

 **ThrottlingException**   
Amazon Rekognition is temporarily unable to process the request\. Try your call again\.  
HTTP Status Code: 500

## Example<a name="API_CompareFaces_Examples"></a>

### Example Request<a name="API_CompareFaces_Example_1"></a>

The following example shows a request that compares a source image \(people\.img\) with a target image \(family\.jpg\)\.

#### Sample Request<a name="API_CompareFaces_Example_1_Request"></a>

```
POST https://rekognition.us-west-2.amazonaws.com/ HTTP/1.1
Host: rekognition.us-west-2.amazonaws.com
Accept-Encoding: identity
Content-Length: 170
X-Amz-Target: RekognitionService.CompareFaces
X-Amz-Date: 20170105T165437Z
User-Agent: aws-cli/1.11.25 Python/2.7.9 Windows/8 botocore/1.4.82
Content-Type: application/x-amz-json-1.1
Authorization: AWS4-HMAC-SHA256 Credential=XXXXXXXX/20170105/us-west-2/rekognition/aws4_request,
 SignedHeaders=content-type;host;x-amz-date;x-amz-target, Signature=XXXXXXXX
{
   "TargetImage":{
      "S3Object":{
         "Bucket":"example-photos",
         "Name":"family.jpg"
      }
   },
   "SourceImage":{
      "S3Object":{
         "Bucket":"example-photos",
         "Name":"people.jpg"
      }
   }
```

#### Sample Response<a name="API_CompareFaces_Example_1_Response"></a>

```
{
    "FaceMatches": [{
        "Face": {
            "BoundingBox": {
                "Width": 0.5521978139877319,
                "Top": 0.1203877404332161,
                "Left": 0.23626373708248138,
                "Height": 0.3126954436302185
            },
            "Confidence": 99.98751068115234,
            "Pose": {
                "Yaw": -82.36799621582031,
                "Roll": -62.13221740722656,
                "Pitch": 0.8652129173278809
            },
            "Quality": {
                "Sharpness": 99.99880981445312,
                "Brightness": 54.49755096435547
            },
            "Landmarks": [{
                    "Y": 0.2996366024017334,
                    "X": 0.41685718297958374,
                    "Type": "eyeLeft"
                },
                {
                    "Y": 0.2658946216106415,
                    "X": 0.4414493441581726,
                    "Type": "eyeRight"
                },
                {
                    "Y": 0.3465650677680969,
                    "X": 0.48636093735694885,
                    "Type": "nose"
                },
                {
                    "Y": 0.30935320258140564,
                    "X": 0.6251809000968933,
                    "Type": "mouthLeft"
                },
                {
                    "Y": 0.26942989230155945,
                    "X": 0.6454493403434753,
                    "Type": "mouthRight"
                }
            ]
        },
        "Similarity": 100.0
    }],
    "SourceImageOrientationCorrection": "ROTATE_90",
    "TargetImageOrientationCorrection": "ROTATE_90",
    "UnmatchedFaces": [{
        "BoundingBox": {
            "Width": 0.4890109896659851,
            "Top": 0.6566604375839233,
            "Left": 0.10989011079072952,
            "Height": 0.278298944234848
        },
        "Confidence": 99.99992370605469,
        "Pose": {
            "Yaw": 51.51519012451172,
            "Roll": -110.32493591308594,
            "Pitch": -2.322134017944336
        },
        "Quality": {
            "Sharpness": 99.99671173095703,
            "Brightness": 57.23163986206055
        },
        "Landmarks": [{
                "Y": 0.8288310766220093,
                "X": 0.3133862614631653,
                "Type": "eyeLeft"
            },
            {
                "Y": 0.7632885575294495,
                "X": 0.28091415762901306,
                "Type": "eyeRight"
            },
            {
                "Y": 0.7417283654212952,
                "X": 0.3631140887737274,
                "Type": "nose"
            },
            {
                "Y": 0.8081989884376526,
                "X": 0.48565614223480225,
                "Type": "mouthLeft"
            },
            {
                "Y": 0.7548204660415649,
                "X": 0.46090251207351685,
                "Type": "mouthRight"
            }
        ]
    }],
    "SourceImageFace": {
        "BoundingBox": {
            "Width": 0.5521978139877319,
            "Top": 0.1203877404332161,
            "Left": 0.23626373708248138,
            "Height": 0.3126954436302185
        },
        "Confidence": 99.98751068115234
    }
}
```

## See Also<a name="API_CompareFaces_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/rekognition-2016-06-27/CompareFaces) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/rekognition-2016-06-27/CompareFaces) 