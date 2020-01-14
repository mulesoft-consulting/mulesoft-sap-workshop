@Library('gest-global-library')_
node(label: 'mule-builder') {
  def map = [:]
    map.put('ymlFile', "sap-workshop.yaml")
    map.put('bucket',"sap.tools.mulesoft.com")
    map.put("region", "us-east-1")
    buildDocumentation(map)
  
}