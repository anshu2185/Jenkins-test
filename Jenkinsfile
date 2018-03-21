node {
   echo 'Onboarding started'
   def service_type = params.service_type
   def service_name = params.service_name
   def description = params.description
   def domain = params.domain
   def runtime = params.runtime
   def username = params.username
   def password = params.password
   def credentialid = params.credentialid
   def repo_protocol				= "http://"
   def var_github_repo = repo_protocol + "github.com/anshu2185" + "/"
   def service_template
   echo "Starting new service on-boarding.."
	//echo "params : $params"
	
	stage('Set service Template')
	{
	   if(service_type == "pkmst" || service_type == "dropwizard"){
	   if(runtime == "nodejs" || runtime  == "python" || runtime == "java" || runtime == "" ) {
	   
	   switch (service_type) {
	   
	     case "pkmst":
						if(runtime == "java" )
						{
							service_template = "api-template-pkmst"
						}
						break
		case "dropwizard":
						if(runtime == "java" )
						{
							service_template = "api-template-dropwizard"
						}
						break
	   }
	   }
	   else{
	            error "Invalid runtime"
	       }
	   }
	   else{
	            error "Invalid Service Type"
	       }
	
	}
	
	stage ('Get Service Template'){
		try{
			sh 'rm -rf *'
			sh 'rm -rf .*'
		}
		catch(error){
			//do nothing
		}
		try{
		sh 'mkdir ' + service_template
			dir(service_template){
		     // git url: var_github_repo+service_template
			 checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[ url: var_github_repo + service_template + '.git']]])
		 }
		}catch(error){
		
		}
	
	
	}
	
	stage ('Uploading templates to code repository'){
	
		withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: credentialid, passwordVariable: password, usernameVariable: username]]) {
			def encoded_password = URLEncoder.encode(password, "utf-8")
            sh "git clone http://$username:$encoded_password@" + "github.com/anshu2185" + service_name +".git"
		}
	
			try{
				sh 'rm -rf ' + service_name
			sh 'mkdir ' + service_name
			sh "mv -nf " + service_template + "/* " + service_name + "/"
			//sh "mv -nf " + service_template + "/.* " + service_name + "/"
				}catch (error)
					{
					}
		
		dir(service_name)
		{
		   withCredentials([[$class: 'UsernamePasswordMultiBinding',credentialsId: credentialid, passwordVariable: password, usernameVariable: username]]) {
			try{
					sh "git add --all"
					sh "git commit -m 'Code from the standard template'"
					sh "git remote -v"
					sh "git push -u origin master "
				}catch (error)
					{
					}
					}
		}
	
}
}
