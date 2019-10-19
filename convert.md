# 用SM4的ECB模式和CBC模式加密图片

使用ImageMagick和gmssl在命令行下完成。

原图片如下所示。

![](https://tva1.sinaimg.cn/large/006y8mN6ly1g83dtpj712j30k00k075n.jpg)

首先使用ImageMagick的convert功能将原图片转换成rgba的格式。

```shell
convert -depth 8 pku_logo.jpg pku_logo.rgba
```

接着使用gmssl，用SM4的ECB模式和CBC模式进行加密。

```shell
gmssl enc -sms4-ecb -e -in pku_logo.rgba -out pku_logo_ecb.rgba
	enter sms4-cbc encryption password:
	Verifying - enter sms4-cbc encryption password:

gmssl enc -sms4-cbc -e -in pku_logo.rgba -out pku_logo_cbc.rgba
	enter sms4-cbc encryption password:
  Verifying - enter sms4-cbc encryption password:
```

输入该命令后会让我们设置加密的密钥，设置完后即可完成加密。

随后将加密后的rgba格式的图片转成jpg格式。由于在转换时需要指出图片的大小和深度（即每个像素所用的比特数），所以要先使用`identify`命令查看原图片的信息。

```shell
identify pku_logo.jpg
out: pku_logo.jpg JPEG 720x720 720x720+0+0 8-bit sRGB 103497B 0.000u 0:00.000
```

得到图片的信息之后即可转换。

```shell
convert -size 720x720 -depth 8 pku_logo_ecb.rgba pku_logo_ecb.jpg

convert -size 720x720 -depth 8 pku_logo_cbc.rgba pku_logo_cbc.jpg
```

转换之后的图片如下。

- 用ECB模式加密后的图片

![pku_logo_ecb](https://tva1.sinaimg.cn/large/006y8mN6ly1g83e5r6z1xj30k00k042d.jpg)



- 用CBC模式加密后的图片

![pku_logo_cbc](https://tva1.sinaimg.cn/large/006y8mN6ly1g83e654miaj30k00k0dn5.jpg)