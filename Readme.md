 Test that the app works

 test upload by running 

 helm test widgetapi --filter name=widgetapi-test-download --logs  


 test download by running

 helm test widgetapi --filter name=widgetapi-test-download --logs  



 
 # Notes for the platform team

 * We'll want to design an approach to auto-increment the version of our helm chart any time we touch the files, either by autoincrementing the `version` field in Chart.yaml. or by running `helm package` in a CI pipeline and passing in version based on a git tag, git commit height, or a conventional commit message format.. but all of that would require our own helm registry, so further discussion is needed.
 * Kubernetes v.1.30 is EOL as of June 28th 2025, we should look into updating our clusters to a later version
 * We need to work with developers to configure liveness and readines probes so that we can ensure the app is up and running before we start shipping traffic to it
 * The template has the template all ready for ingress, we do need to deploy and test it in our cluster. 