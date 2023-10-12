# GCloud-Steps

## GSP037
## Detect Labels, Faces and Landmarks in images with the Cloud Vision API:
## 1. CREATE AN API_KEY > APIs & Services > Credentials > Create Credentials>API key
## 2. Run in cloudshell
```cmd
gcloud alpha services api-keys create --display-name="CloudHustlers" 
KEY_NAME=$(gcloud alpha services api-keys list --format="value(name)" --filter "displayName=CloudHustlers")
export API_KEY=$(gcloud alpha services api-keys get-key-string $KEY_NAME --format="value(keyString)")
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
gsutil mb gs://$PROJECT_ID
git clone https://github.com/siddharth7000/practice.git
gsutil cp practice/donuts.png gs://$PROJECT_ID
gsutil cp practice/selfie.png gs://$PROJECT_ID
gsutil cp practice/city.png gs://$PROJECT_ID
gsutil acl ch -u AllUsers:R gs://$PROJECT_ID/donuts.png
gsutil acl ch -u AllUsers:R gs://$PROJECT_ID/selfie.png
gsutil acl ch -u AllUsers:R gs://$PROJECT_ID/city.png
```
## # GSP038
## 1. CREATE AN API_KEY > APIs & Services > Credentials > Create Credentials > API key
## 2. Run in cloudshell 
```cmd
gcloud beta compute ssh  linux-instance
```
### 3. Press Y Enter > Enter > Enter > Press N Enter
```cmd
gcloud services enable apikeys.googleapis.com
gcloud alpha services api-keys create --display-name="CloudHustlers" 
KEY_NAME=$(gcloud alpha services api-keys list --format="value(name)" --filter "displayName=CloudHustlers")
API_KEY=$(gcloud alpha services api-keys get-key-string $KEY_NAME --format="value(keyString)")
echo $API_KEY
touch request.json
tee request.json <<EOF
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"Joanne Rowling, who writes under the pen names J. K. Rowling and Robert Galbraith, is a British novelist and screenwriter who wrote the Harry Potter fantasy series."
  },
  "encodingType":"UTF8"
}
EOF
cat request.json
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" -s -X POST -H "Content-Type: application/json" --data-binary @request.json > result.json
cat result.json
```

# GSP1151
```cmd
https://console.cloud.google.com/vertex-ai/workbench/user-managed?
```
### 1. Open JupyterLab
### 2. Generative-ai > Language > prompts > intro_prompt_design.ipynb (Run each cell one at a time)
### 3. examples > ideation.ipynb

# GSP1150
## 1. Open the link
```cmd
https://console.cloud.google.com/vertex-ai/workbench/user-managed?
```
### 2. Open JupyterLab
### 3. Generative-ai > Language > prompts > intro_prompt_design.ipynb (Run each cell one at a time)
### 4. Go back to language
### 5. getting-started >  intro_palm_api.ipynb (Run each cell one at a time)

# GSP1154

>search ```Vertex AI API``` > Enable

>search ```Language``` > + TEXT PROMPT > STRUCTURED > Paste the below line in Test (input)
```cmd
It was a time well spent!
```
>select the model as ```text-bison@001``` > Submit > Save > Prompt name ```Hustler1```

### 1. Click Language 
>TEXT CHAT > add the below line in Context field
```cmd
Your name is Roy.  
You are a support technician of an IT department.
You only respond with "Have you tried turning it off and on again?" to any queries.
```
>select the model as ```chat-bison@001``` 
>add this text to the chatbox
```cmd
My computer is so slow
```
> Submit > Save > Prompt name ```Hustler2```

## Complete ARC103 according to the GCloud page
## Complete ARC106 according to the GCloud page

# ARC122

## 1. Run in cloudshell
```cmd
gcloud alpha services api-keys create --display-name="CloudHustlers" 
KEY_NAME=$(gcloud alpha services api-keys list --format="value(name)" --filter "displayName=CloudHustlers")
export API_KEY=$(gcloud alpha services api-keys get-key-string $KEY_NAME --format="value(keyString)")
export PROJECT_ID=$(gcloud config list --format 'value(core.project)')
gsutil acl ch -u allUsers:R gs://$PROJECT_ID-bucket/manif-des-sans-papiers.jpg
cat > request.json <<EOF
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://$PROJECT_ID-bucket/manif-des-sans-papiers.jpg"
          }
        },
        "features": [
          {
            "type": "TEXT_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
EOF
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY} -o text-response.json
gsutil cp text-response.json gs://$PROJECT_ID-bucket
cat > request.json <<EOF
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://$PROJECT_ID-bucket/manif-des-sans-papiers.jpg"
          }
        },
        "features": [
          {
            "type": "LANDMARK_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
EOF
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY} -o landmark-response.json
gsutil cp landmark-response.json gs://$PROJECT_ID-bucket
```
