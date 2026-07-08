# 인증서 분석용 커맨드 모음

## OpenSSL
**1. PKCS#12 파일 확인 (.p12, .pfx)**
```
* 인증서/키 정보 확인
 openssl pkcs12 -info -in keystore.p12

* 개인키 제외, 인증서 정보만 출력
 openssl pkcs12 -info -in keystore.p12 -nokeys

* 인증서만 추출(PEM)
 openssl pkcs12 -in keystore.p12 -nokeys -out cert.pem

* 개인키만 추출(PEM)
 openssl pkcs12 -in keystore.p12 -nocerts -out key.pem

* 암호 없이 개인키 추출
 openssl pkcs12 -in keystore.p12 -nocerts -nodes -out key.pem
```
\
**2. 인증서(.crt, .cer, .pem) / 개인키(.key, .pem) 확인**
```
* 인증서 정보 확인
 openssl x509 -in cert.pem -text -noout
 openssl x509 -in server.crt -text -noout
** DER 형식
 openssl x509 -in cert.cer -inform DER -text -noout


* 개인키 정보 확인
 openssl rsa -in private.key -text -noout
 openssl ec -in private.key -text -noout
** 키 타입 불확실할 경우
 openssl pkey -in private.key -text -noout
```
\


## keytool
**1. 키스토어(.jks) 확인 **
```
* 키스토어 확인
 keytool -list -v -keystore keystore.jks
 keytool -list -v -keystore keystore.jks -storepass password
** provider 지정(with Bouncy Castle)
 keytool -list -v -keystore cert.bks -storetype BKS -provider org.bouncycastle.jce.provider.BouncyCastleProvider -providerpath bcprov-jdk..on-1....jar

* alias 목록 확인
 keytool -list -keystore keystore.jks
 keytool -list -v -keystore keystore.jks -alias alias-name
** p12 대상
 keytool -list -v -keystore keystore.p12 -storetype PKCS12
```
\
