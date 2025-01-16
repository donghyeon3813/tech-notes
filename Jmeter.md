# JMeter란?  
Apache JMeter는 오픈소스 성능 테스트 도구로, 웹 애플리케이션 및 API의 **부하 테스트**, **스트레스 테스트**, **동시성 테스트**를 수행합니다.  
HTTP, REST, SOAP, JDBC 등 다양한 프로토콜을 지원하며, 스레드 그룹 설정으로 가상 사용자를 시뮬레이션할 수 있습니다.  
직관적인 GUI와 CLI를 제공하며, 테스트 결과를 실시간 시각화하거나 로그로 저장해 분석할 수 있습니다.  
분산 테스트와 플러그인을 통해 확장 가능하며, 클라이언트-서버 성능 병목을 평가하는 데 유용합니다.

# jMeter 설치 및 사용 방법(windows)
## 설치
https://jmeter.apache.org/download_jmeter.cgi
1. apache-jmeter-5.6.3.zip 다운로드
2. 압축 풀고 bin 폴더에 jmeter.bat 실행

## 사용 방법
### Thread Group 설정
- Number of Threads (Users): 동시에 작동할 가상 사용자 수.
- Ramp-Up Period (in seconds): 모든 사용자가 동작을 시작하기까지 걸리는 시간.
- Loop Count: 각 사용자가 작업을 반복 수행할 횟수.
### httpRequest 설정
- protocol
- Server Name or IP
- port
- HttpMethod
- path
- body or parameters
### result
- view Results Tree
- summary Report

