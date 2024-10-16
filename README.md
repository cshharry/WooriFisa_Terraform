
# 📦 **Terraform S3: 파일 업로드 및 수정 작업**

### ✨ **개요**
**Terraform**을 활용해 S3 버킷 생성 및 파일 업로드 작업을 수행합니다. 아래와 같이 각 작업을 **별도의 Terraform 설정 파일로 분리**하여 구현합니다.

---

## 📝 **구성 파일**
- **provider.tf**: AWS 공급자 설정
- **bucket_creation.tf**: S3 버킷 생성
- **upload_new_index.tf**: 새로운 `index.html` 업로드
- **modify_existing_index.tf**: 기존 `index.html` 수정
- **upload_main.tf**: 새로운 `main.html` 업로드

---

## 🚀 **미션 수행 단계**

### 1️⃣ **S3 버킷 생성**  
`bucket_creation.tf` 파일을 사용해 S3 버킷을 생성합니다.

```hcl
resource "aws_s3_bucket" "bucket1" {
  bucket = "<버킷명>"
}

resource "aws_s3_bucket_public_access_block" "public_access_block" {
  bucket = aws_s3_bucket.bucket1.id
  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false

  depends_on = [aws_s3_bucket.bucket1]
}
```

---

### 2️⃣ **새로운 index.html 업로드**  
`upload_new_index.tf` 파일을 사용해 새롭게 만든 `index.html` 파일을 업로드합니다.

```hcl
resource "aws_s3_bucket_object" "new_index_file" {
  bucket        = aws_s3_bucket.bucket1.id
  key           = "index.html"
  source        = "index.html"
  content_type  = "text/html"
}
```

**index.html 파일 예시:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Index Page</title>
</head>
<body>
    <h1>Welcome to the Index Page</h1>
</body>
</html>
```

---

### 3️⃣ **기존 index.html 수정 및 업로드**  
`modify_existing_index.tf` 파일을 사용해 기존의 `index.html` 파일을 수정하고 재업로드합니다.

```hcl
resource "aws_s3_bucket_object" "modified_index" {
  bucket        = aws_s3_bucket.bucket1.id
  key           = "index.html"
  source        = "index.html"
  content_type  = "text/html"
}
```

**수정된 index.html 파일 예시:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Modified Index Page</title>
</head>
<body>
    <h1>Welcome to the Modified Index Page</h1>
    <p><a href="main.html">Go to Main Page</a></p>
</body>
</html>
```

---

### 4️⃣ **main.html 업로드**  
`upload_main.tf` 파일을 사용해 새로운 `main.html` 파일을 업로드합니다.

```hcl
resource "aws_s3_bucket_object" "main_file" {
  bucket        = aws_s3_bucket.bucket1.id
  key           = "main.html"
  source        = "main.html"
  content_type  = "text/html"
}
```

**main.html 파일 예시:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Main Page</title>
</head>
<body>
    <h1>Welcome to the Main Page</h1>
</body>
</html>
```

---

## ⚙️ **Terraform 명령어**

1. **초기화**  
   ```bash
   terraform init
   ```

2. **플랜 확인**  
   ```bash
   terraform plan
   ```

3. **적용**  
   ```bash
   terraform apply -auto-approve
   ```

---

## 🌐 **정적 웹사이트 확인**

S3 버킷의 정적 웹사이트 URL을 통해 HTML 페이지를 확인합니다:

```
http://<버킷명>.s3-website-ap-northeast-2.amazonaws.com/index.html
```
