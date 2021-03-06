# StreamProcessor<a name="API_StreamProcessor"></a>

An object that recognizes faces in a streaming video\. An Amazon Rekognition stream processor is created by a call to [CreateStreamProcessor](API_CreateStreamProcessor.md)\. The request parameters for `CreateStreamProcessor` describe the Kinesis video stream source for the streaming video, face recognition parameters, and where to stream the analysis resullts\. 

## Contents<a name="API_StreamProcessor_Contents"></a>

 **Name**   
Name of the Amazon Rekognition stream processor\.   
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `[a-zA-Z0-9_.\-]+`   
Required: No

 **Status**   
Current status of the Amazon Rekognition stream processor\.  
Type: String  
Valid Values:` STOPPED | STARTING | RUNNING | FAILED | STOPPING`   
Required: No

## See Also<a name="API_StreamProcessor_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:

+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/rekognition-2016-06-27/StreamProcessor) 

+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/rekognition-2016-06-27/StreamProcessor) 

+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/rekognition-2016-06-27/StreamProcessor) 

+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/rekognition-2016-06-27/StreamProcessor) 