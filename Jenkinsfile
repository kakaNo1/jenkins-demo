node('jenkinsbuildnode') {
stage('Prepare') {
    echo "1.Clone Stage"
    git url: "https://github.com/kakaNo1/jenkins-demo.git"
    script {
        build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
    }
}
stage('Build') {
    echo "3.Build Docker Image Stage"
    sh "docker build -t 172.16.3.188:32596/zqm/jenkins-demo:${build_tag} ."
}
stage('Push') {
    echo "4.Push Docker Image Stage"
    sh "docker login http://172.16.3.188:32596 -u admin -p Harbor12345"
    sh "docker push 172.16.3.188:32596/zqm/jenkins-demo:${build_tag}"
}
stage('YAML') {
    echo "5. Change YAML File Stage"
    sh "sed -i 's/<BUILD_TAG>/${build_tag}/' k8s.yaml"
    sh "sed -i 's/<BRANCH_NAME>/${env.BRANCH_NAME}/' k8s.yaml"
}
stage('Deploy') {
    echo "6. Deploy Stage"
    if (env.BRANCH_NAME == 'master') {
        input "确认要部署线上环境吗？"
    }
    sh "kubectl apply -f k8s.yaml -n cicd"
}
}
