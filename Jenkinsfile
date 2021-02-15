node {
    stage ('Checkout') {
        git 'https://github.com/aqulive/JavaVulnLabv3.git'
    }
 
    stage ('Build') {
        def mvnHome = tool 'mvn-default'
 
        sh "${mvnHome}/bin/mvn --batch-mode -V -U -e clean test -Dsurefire.useFile=false"
 
        junit testResults: '**/target/surefire-reports/TEST-*.xml'
 
        def java = scanForIssues tool: [$class: 'Java']
        def javadoc = scanForIssues tool: [$class: 'JavaDoc']
         
        publishIssues issues:[java]
        publishIssues issues:[javadoc]
    }
 
    stage ('Analysis') {
        def mvnHome = tool 'mvn-default'
 
        sh "${mvnHome}/bin/mvn -batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs spotbugs:spotbugs"
 
        def checkstyle = scanForIssues tool: [$class: 'CheckStyle'], pattern: '**/target/checkstyle-result.xml'
        publishIssues issues:[checkstyle]
    
        def pmd = scanForIssues tool: [$class: 'Pmd'], pattern: '**/target/pmd.xml'
        publishIssues issues:[pmd]
         
        def cpd = scanForIssues tool: [$class: 'Cpd'], pattern: '**/target/cpd.xml'
        publishIssues issues:[cpd]
         
        def findbugs = scanForIssues tool: [$class: 'FindBugs'], pattern: '**/target/findbugsXml.xml'
        publishIssues issues:[findbugs]
 
        def spotbugs = scanForIssues tool: [$class: 'SpotBugs'], pattern: '**/target/spotbugsXml.xml'
        publishIssues issues:[spotbugs]
    }
}