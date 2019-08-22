node{
	checkout scm
	env.DOCKER_API_VERSION="1.39"

	sh "git rev-parse --short HEAD > commit-id"
	tag = readFile('commit-id').replace("\n", "").replace("\r", "")

    registryHost = "hub.yuansuan.cn/xx/"

	appName_backend = "guestbook_backend"
	appName_frontend = "guestbook_frontend"

	imageName_backend = "${appName_backend}:${tag}"
	imageName_frontend = "${appName_frontend}:${tag}"

	stage "Build"
    
        sh "docker build -t ${imageName_frontend} -f php-redis/Dockerfile php-redis"
        sh "docker build -t ${imageName_backend} -f redis-slave/Dockerfile redis-slave"
    
    stage "Push"

        sh "docker login hub.yuansuan.cn -u admin -p Yskj2407"
        sh "docker tag ${appName_frontend} ${registryHost}${appName_frontend}"
        sh "docker push ${registryHost}${appName_frontend}"
        sh "docker tag ${appName_backend} ${registryHost}${appName_backend}"
        sh "docker push ${registryHost}${appName_backend}"
        
    stage "Deploy"

        sh "kubectl set image deploy redis-slave slave=${registryHost}${appName_backend}"
        sh "kubectl set image deploy frontend php-redis=${registryHost}${appName_frontend}"

}