pipeline {
    agent any // Use any available agent라네요

    tools {
        maven 'maven 3.9.11' // Specify Maven version(이거 jenkins에서 설정한 이름이랑 완전히 똑같이 써야 에러가 안난대요)
    }

    environment {
        // 배포에 필요한 환경 변수 설정
        DOCKER_IMAGE = 'demo-app' // 도커 이미지 이름
        CONTAINER_NAME = 'springboot-container' // 컨테이너 이름
        JAR_FILE_NAME = 'app.jar' // 복사할 JAR 파일 이름
        PORT = '8081' // 컨테이너와 연결할 포트

        REMOTE_USER = 'ec2-user' // 원격 서버 사용자 이름
        REMOTE_HOST = '13.125.109.217' // 원격 서버(spring-server) IP(public) 주소
        REMOTE_DIR = '/home/ec2-user/deploy' // 원격 서버에 파일을 복사할 디렉토리

        SSH_CREDENTIALS_ID = '35f9b68a-724e-4f4f-899a-bcaf2569a443' // Jenkins에 저장된 SSH 자격 증명 ID

    }

    // 파이프라인의 각 단계를 정의(stages -> stage -> steps)
    stages {
        stage('Git Checkout') {
            steps { // 스테이지 내에서 실제로 수행할 작업 정의
                // 현재 파이프라인이 정의된 저장소에서 (최신!)코드 체크아웃
                // SCM: Source Code Management(소스 코드 관리)
                checkout scm 
                echo 'checkout...(코드췕!)'
            }
        }
        stage('Maven Build') {
            steps {
                // 테스트는 생략하고 바로 Maven 빌드 수행
                sh 'mvn clean package -DskipTests'
                echo 'Mavne Building...(건물주 되고 싶다...)'
            }
        }
        stage('Prepare Jar') {
            steps {
                // 빌드된 JAR 파일을 지정된 이름(app.jar)으로 복사
                sh "cp target/demo-0.0.1-SNAPSHOT.jar ${JAR_FILE_NAME}" // 혹시 안되면 "" -> ' ' 로 바꿔보기
                echo 'Preparing JAR...(JAR 준비 완료!)'
            }
        }
    }
}