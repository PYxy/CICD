import java.text.SimpleDateFormat;
	
	pipeline {
	  agent {
	    kubernetes {
	      yaml """
	apiVersion: v1
	kind: Pod
	metadata:
	  labels:
	    jenkins: worker
	spec:
	  containers:
	  - name: kaniko
	    image: gcr.io/kaniko-project/executor:debug
	    command:
	    - sleep
	    args:
	    - 99999
	    tty: true
	    volumeMounts:
	      - name: docker-secret
	        mountPath: /kaniko/.docker
	        readOnly: true
	  volumes:
	  - name: docker-secret
	    secret:
	      secretName: regcred
	"""
	    }
	  }
	  environment {
	    DATED_GIT_HASH = "${new SimpleDateFormat("yyMMddHHmmss").format(new Date())}${GIT_COMMIT.take(6)}"
	  }
	  stages {
	    stage('Configure') {
	      steps {
	        echo "hello, starting1111"
	      }
	    }    
	    stage('Build with Kaniko') {
	      steps {
	        container('kaniko') {
            sh 'sleep 300'
            
	          sh '/kaniko/executor -f `pwd`/Dockerfile -c `pwd`/src --cache=true \
	          --destination=core.harbor.domain/httpservice/httpserver:${DATED_GIT_HASH} \
	                  --insecure \
	                  --skip-tls-verify  \
	                  -v=debug'
	        }
	      }
	    }  
	  }
	}
