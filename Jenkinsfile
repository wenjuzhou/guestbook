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

        sh "echo "build 镜像" "
        sh "docker build -t ${imageName_frontend} -f php-redis/Dockerfile php-redis"
        sh "docker build -t ${imageName_backend} -f redis-slave/Dockerfile redis-slave"
    
    stage "Push"

    	sh "echo "push 镜像" "
        sh "docker login hub.yuansuan.cn -u admin -p Yskj2407"
        sh "docker tag ${imageName_backend} ${registryHost}${imageName_backend}"
        sh "docker push ${registryHost}${imageName_backend}"
        sh "docker tag ${imageName_frontend} ${registryHost}${imageName_frontend}"
        sh "docker push ${registryHost}${imageName_frontend}"

    stage "Deploy"

    	sh "echo "deploy 镜像" "
        sh "kubectl set image deploy redis-slave slave=${registryHost}${imageName_backend}"
        sh "kubectl set image deploy frontend php-redis=${registryHost}${imageName_frontend}"

}