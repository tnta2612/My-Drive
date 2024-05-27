MobSF is a mobile application security testing framework capable of performing static and dynamic analysis when supplied with an APK.
How to use: https://mobsf.github.io/docs/#/mobsf_docker?id=download

```
docker run -it --rm -p 8000:8000 -p 1337:1337 -e MOBSF_ANALYZER_IDENTIFIER=127.0.0.1:6555 opensecurity/mobile-security-framework-mobsf:latest
```