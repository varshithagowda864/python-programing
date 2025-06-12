# python-programing
// Create a PutObjectRequest
PutObjectRequest put_object_request;
put_object_request.SetBucket(bucket_name);
put_object_request.SetKey(file_name);

// Set file stream for uploading the file
std::shared_ptr<Aws::IOStream> file_stream = Aws::MakeShared<Aws::FStream>("UploadFileToS3",
    file_name, std::ios_base::in | std::ios_base::binary);

if (!file_stream->good()) {
    std::cerr << "Error opening file " << file_name << std::endl;
    return;
}

put_object_request.SetBody(file_stream);

// Upload the file
auto outcome = s3_client.PutObject(put_object_request);

if (outcome.IsSuccess()) {
    std::cout << "Successfully uploaded file " << file_name << " to bucket " << bucket_name << std::endl;
} else {
    std::cerr << "Error uploading file: " << outcome.GetError().GetMessage() << std::endl;
}
}

int main() { // Initialize the AWS SDK Aws::SDKOptions options; Aws::InitAPI(options);

// Specify the bucket name and file name
const char* bucket_name = "your-bucket-name";  // Replace with your bucket name
const char* file_name = "example.txt";  // Replace with your file path

// Upload the file to S3
UploadFileToS3(bucket_name, file_name);

// Shut down the AWS SDK
Aws::ShutdownAPI(options);

return 0;
}
