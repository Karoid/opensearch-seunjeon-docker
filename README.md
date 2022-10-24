# opensearch-seunjeon
opensearch로 한국어 검색 환경을 구축할 수 있는 Plugin 설치
opensearch로 한국어 검색 환경을 구축할 수 있는 Docker image builder

# Usage
## Making plugin
```bash
# download plugin
bash <(curl -s https://bitbucket.org/eunjeon/seunjeon-opensearch/raw/master/opensearch/scripts/downloader.sh) -e <opensearch-version> -p <plugin-version>
  에제) $ bash <(curl -s https://bitbucket.org/eunjeon/seunjeon-opensearch/raw/master/opensearch/scripts/downloader.sh) -e 2.3.0 -p 2.3.0.0

# install plugin
./bin/opensearch-plugin install file://`pwd`/opensearch-analysis-seunjeon-<plugin-version>.zip
  예제) $ ./bin/opensearch-plugin install file://`pwd`/opensearch-analysis-seunjeon-2.3.0.0.zip
```
* downloader.sh 가 하는 일은 opensearch-analysis-seunjeon-<plugin-version>.zip 파일을 내려받은 후 plugin-descriptor.properties 의 opensearch.version 을 변경하여 재압축합니다. * opensearch가 버전 업 될때마다 플러그인을 재배포하는데 어려움이 있어 스크립트를 제공합니다.
### Usable Plugin Release
| opensearch-analysis-seunjeon | target opensearch | release note |
| ------------------------------- | ---------------------| ------------ |
| 2.3.0.0                         | 2.3.0                |  |

## docker 단독 사용
```bash
docker run --rm .
```

## docker-compose.yml으로 사용
```yml

```

# Building new version of plugin file
만약 opensearch 버전이 올라가고 [opensearch-seunjeon](https://bitbucket.org/soosinha/seunjeon-opensearch/src/main/opensearch/) 프로젝트에 최신 버전의 플러그인 코드가 공개된다면 다음 명령으로 플러그인을 다시 빌드할 수 있습니다.
```bash
mkdir -p build
docker run -it --name plugin-builder -v $(pwd)/build:/tmp/build -v $(pwd)/scripts:/tmp/scripts mozilla/sbt bash
# container bash 진입 > dictionary 생성
./tmp/scripts/zip-builder.sh
# zip 생성
sbt
sbt:opensearch-analysis-seunjeon> project opensearch
sbt:opensearch-analysis-seunjeon> opensearchZip
```
다음 명령이 성공하면 `build/opensearch-analysis-seunjeon/seunjeon-opensearch/opensearch/target/opensearch-analysis-seunjeon-assembly-2.3.0.jar`에 파일이 만들어진다

만약 opensearchZip을 빌드할 때 사용하는 opensearch의 버전을 올리고 싶으면 `scripts/zip-builder.sh`의 다음 두 부분의 2.3.0을 변경하면 된다
```sbt
sed -i '/val opensearchVersion = "1.0.0"/c\val opensearchVersion = "2.3.0"' build.sbt
sed -i '/val opensearchJarVersion = "1.0.0-beta1"/c\val opensearchJarVersion = "2.3.0"' build.sbt
```

# References
[seunjeon](https://bitbucket.org/eunjeon/seunjeon/src/master/elasticsearch/): 한국어 형태소분석기를 elasticsearch에 사용할 수 있도록 만든 plugin입니다.  
[opensearch-seunjeon](https://bitbucket.org/soosinha/seunjeon-opensearch/src/main/opensearch/): 기존의 elasticsearch에서 사용 가능하던 한국어 형태소 분석기를 opensearch에서 사용할 수 있도록 변경한 plugin입니다.