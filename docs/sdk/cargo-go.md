# CARGO-GO SDK 使用文档

兼容 Amazon S3 云存储的 Cargo Go 客户端 SDK

Cargo Go Client SDK 提供简单的 API 来访问任何与 Amazon S3 兼容的对象存储。

本快速入门指南将向您展示如何下载 Cargo SDK、连接到 Cargo 并提供简单文件上传示例。

## 下载
```shell
GO111MODULE=on go get github.com/byteark-project/cargo-go/v7
```
## 初始化 cargoClient 对象
需要提供以下参数来兼容 Amazon S3 对象存储。

| Parameter  | Description|
| :---         |     :---     |
| endpoint   | 对象存储地址   |
| _cargo.Options_ | 其他 |

```go
package main

import (
	"log"

	cargo "github.com/byteark-project/cargo-go/v7"
	"github.com/byteark-project/cargo-go/v7/pkg/credentials"
)

func main() {
	endpoint := "cargo.byteark.cn"
	accessKeyID := "Q3AM3UQ867SPQQA43P2F"
	secretAccessKey := "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG"
	useSSL := true

	// 初始化
	cargoClient, err := cargo.New(endpoint, &cargo.Options{
		Creds:  credentials.NewStaticV4(accessKeyID, secretAccessKey, ""),
		Secure: useSSL,
	})
	if err != nil {
		log.Fatalln(err)
	}

	log.Printf("%#v\n", cargoClient) // 获取client操作对象
}
```
## 一个文件上传的示例
此示例程序连接到对象存储服务器，创建存储桶并将文件上传到存储桶。

### FileUploader.go
```go
package main

import (
	"context"
	"log"

	cargo "github.com/byteark-project/cargo-go/v7"
	"github.com/byteark-project/cargo-go/v7/pkg/credentials"
)

func main() {
	ctx := context.Background()
	endpoint := "cargo.byteark.cn"
	accessKeyID := "Q3AM3UQ867SPQQA43P2F"
	secretAccessKey := "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG"
	useSSL := true

	// 初始化
	cargoClient, err := cargo.New(endpoint, &cargo.Options{
		Creds:  credentials.NewStaticV4(accessKeyID, secretAccessKey, ""),
		Secure: useSSL,
	})
	if err != nil {
		log.Fatalln(err)
	}

	// 创建存储桶
	bucketName := "mymusic"
	location := "cn-hangzhou-1"

	err = cargoClient.MakeBucket(ctx, bucketName, cargo.MakeBucketOptions{Region: location})
	if err != nil {
		// 判断存储桶是否已经存在
		exists, errBucketExists := cargoClient.BucketExists(ctx, bucketName)
		if errBucketExists == nil && exists {
			log.Printf("We already own %s\n", bucketName)
		} else {
			log.Fatalln(err)
		}
	} else {
		log.Printf("Successfully created %s\n", bucketName)
	}

	// Upload the zip file
	objectName := "golden-oldies.zip"
	filePath := "/tmp/golden-oldies.zip"
	contentType := "application/zip"

	// 上传文件
	info, err := cargoClient.FPutObject(ctx, bucketName, objectName, filePath, cargo.PutObjectOptions{ContentType: contentType})
	if err != nil {
		log.Fatalln(err)
	}

	log.Printf("Successfully uploaded %s of size %d\n", objectName, info.Size)
}
```

### 运行
```sh
export GO111MODULE=on
go run file-uploader.go
2021/08/13 17:03:28 Successfully created mymusic
2016/08/13 17:03:40 Successfully uploaded golden-oldies.zip of size 16253413

cs ls devel_admin/mymusic/
[2021-08-13 16:02:16 PDT]  17MiB golden-oldies.zip
```