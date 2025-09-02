ya tengo esta configuracion en el pipeline. jenkins

#!groovy
@Library('pipeline-utils') _
def customImage
pipeline {
  agent any
  options {
    ansiColor('xterm')
    timestamps()
    timeout(time: 30, unit: 'MINUTES')
    buildDiscarder(logRotator(numToKeepStr: '10'))
    disableConcurrentBuilds()
  }
  tools {
    jdk 'OpenJDK 17'
    gradle 'Gradle 8.5'
  }

  parameters {
    booleanParam (
            defaultValue: true,
            name: 'Sonar',
            description: 'Ejecutar Sonar')
    booleanParam (
            defaultValue: true,
            name: 'Fortify',
            description: 'Ejecutar Fortity SSC')
    booleanParam (
            defaultValue: true,
            name: 'BlackDuck',
            description: 'Ejecutar BlackDuck')
    booleanParam (
            defaultValue: true,
            name: 'Deploy',
            description: 'Deployar aplicacion')
  }

  stages {
    stage('Build') {
      steps {
        sh '''
          gradle clean build
          '''
      }
    }
    /*stage ('Code Quality') {
      steps {
        script {
          codeQuality.runCodeQuality( params.Fortify, params.BlackDuck, params.Sonar )
        }
      }
    }*/
    stage('Site Build') {
      steps {
        script{
          customImage = dfkaniko.buildJavaApp('ib-advisor-monitor-service-0.0.1.jar', '17')
          currentBuild.description = "${customImage}"
        }
      }
    }
    stage ('manifest and deploy'){
      steps{
        script{
          utilsAnthos.buildAndDeployManifest()
        }
      }
    }
  }
  post {
    always {
      echo 'Cleaning workspace...'
      cleanWs()
    }
  }
}


y esta en el buid gradle

buildscript {
    repositories {
        maven {
            url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeyBnsCache}"
        }
        maven {
            url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeyReleaseLocal}"
        }
        maven {
            url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeyDevLocal}"
        }
        maven {
            url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKey}"
        }
        maven {
            url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeySnapshots}"
        }
    }
    dependencies {
        classpath "org.springframework.boot:spring-boot-gradle-plugin:3.3.6"
        classpath "io.spring.gradle:dependency-management-plugin:1.1.6"
        classpath "org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:5.0.0.4638"
        classpath "au.com.dius.pact.provider:gradle:4.6.15"
        classpath "org.openapitools:openapi-generator-gradle-plugin:6.6.0"
    }
}

plugins {
    id 'java'
    id 'jacoco'
}

wrapper {
    gradleVersion = '8.5'
}

apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'
apply plugin: 'org.sonarqube'
apply plugin: 'au.com.dius.pact'
apply plugin: 'org.openapi.generator'

def isPactConsumer = false
def isPactProvider = false

base {
    group = 'com.scotiabank.ib.advisor.monitor.service'
    version = '0.0.1'
    description = 'ib-advisor-monitor-service'
}

java {
    sourceCompatibility = '17'
    targetCompatibility = '17'
}

repositories {
    maven {
        url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeyReleaseLocal}"
    }
    maven {
        url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeyDevLocal}"
    }
    maven {
        url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKey}"
    }

    maven {
        url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeySnapshots}"
    }
    maven {
        url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeyMavenCentralCache}"
    }
    maven {
        url "${artifactory_dfchile_contextUrl}/${artifactory_dfchile_projectRepoKeyReleaseToronto}"
    }
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    implementation 'org.springframework.cloud:spring-cloud-starter-openfeign'
    implementation 'org.springframework.boot:spring-boot-starter-aop'

    // Open Api
    implementation group: 'org.springdoc', name: 'springdoc-openapi-starter-webmvc-ui', version: '2.6.0'
    implementation group: 'org.openapitools', name: 'jackson-databind-nullable', version: '0.2.6'

    // Security:
    //implementation 'org.springframework.boot:spring-boot-starter-oauth2-resource-server'
    //implementation 'com.bns.ib.ibarch:ib-security-spring-lib:1.4'

    // Open Telemetry
    implementation 'io.opentelemetry.instrumentation:opentelemetry-spring-boot-starter'
    implementation 'io.opentelemetry:opentelemetry-extension-trace-propagators:1.40.0'

    //Trace
    implementation 'io.micrometer:micrometer-tracing-bridge-otel'

    // Lombok
    compileOnly 'org.projectlombok:lombok:1.18.30'
    annotationProcessor 'org.projectlombok:lombok:1.18.30'

    // Serializing and deserializing JSON
    implementation 'com.google.code.gson:gson:2.11.0'
    implementation 'org.jsoup:jsoup:1.18.1'

    implementation group: 'org.apache.commons', name: 'commons-lang3', version: '3.11'
    implementation group: 'org.json', name: 'json', version: '20231013'
    implementation 'com.fasterxml.jackson.core:jackson-annotations:2.17.1'
    implementation 'com.fasterxml.jackson.core:jackson-databind'
    implementation 'com.fasterxml.jackson.datatype:jackson-datatype-jsr310'
    implementation 'commons-io:commons-io:2.14.0'

    // Pact
    testImplementation 'au.com.dius.pact.consumer:java8:4.1.36'
    testImplementation group: 'au.com.dius.pact.consumer', name: 'junit5', version: '4.6.14'
    testImplementation(group: 'au.com.dius.pact.provider', name: 'junit5spring', version: '4.6.14') {
        exclude group: "org.springframework", module: "spring-webflux"
        exclude group: "org.springframework", module: "spring-test"
    }

    // Test
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testCompileOnly 'org.projectlombok:lombok:1.18.30'
    testAnnotationProcessor 'org.projectlombok:lombok:1.18.30'
    testImplementation 'org.mockito:mockito-core:4.2.0'

    // JUnit
    testImplementation platform('org.junit:junit-bom:5.9.1')
    testImplementation 'org.junit.jupiter:junit-jupiter'
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:2023.0.4"
        mavenBom "io.opentelemetry.instrumentation:opentelemetry-instrumentation-bom:2.6.0"
    }
}

ext {
    country = System.getenv('COUNTRY')?: 'defaultCountry'
}

task printCountry {
    doLast {
        println "Building for country: ${country}"
    }
}

build.dependsOn printCountry

tasks.named('test') {
    useJUnitPlatform()
}

def openApiOutputDir = "$buildDir/generated".toString()

openApiGenerate {
    generatorName = "spring"
    inputSpec = "$projectDir/src/main/resources/api.yml".toString()
    outputDir = openApiOutputDir
    configOptions = [
            useSpringBoot3  : "true",
            useJakartaEa    : "true",
            dateLibrary     : "java8",
            modelPackage    : "com.scotiabank.ib.advisor.monitor.service.model",
            apiPackage      : "com.scotiabank.ib.advisor.monitor.service.api",
            sourcefolder    : "",
            useTags         : "true",
            interfaceOnly   : "true",
            skipDefaultINterface : "true",
    ]
    globalProperties = [
            modelDocs : "false",
    ]
}

compileJava.dependsOn tasks.openApiGenerate
sourceSets.main.java.srcDirs openApiOutputDir

configurations.configureEach {
    resolutionStrategy.eachDependency { details ->
        [
                ['commons-io', 'commons-io', '2.17.0', 'Blackduck issue'],
                ['org.bouncycastle', 'bcprov-jdk18on', '1.78.1', 'Blackduck issue'],
                ['org.json', 'json', '20240303', 'Blackduck issue'],
                ['xerces', 'xercesImpl', '2.12.2', 'Blackduck issue'],
                ['com.google.guava', 'guava', '33.3.1-jre', 'Blackduck issue']
        ].forEach { constraint ->
            if (details.requested.group === constraint[0] && constraint[1] === '*') {
                details.useVersion constraint[2]
                details.because constraint[3]
            } else if (details.requested.group === constraint[0] && constraint[1] !== '*') {
                details.useVersion constraint[2]
                details.because constraint[3]
            }
        }
    }
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:2023.0.4"
        mavenBom "io.opentelemetry.instrumentation:opentelemetry-instrumentation-bom:2.6.0"
    }
}

import org.yaml.snakeyaml.Yaml
def loadYaml = { file ->
    def yaml = new Yaml()
    def inputStream = file.newInputStream()
    def data = yaml.load(inputStream)
    inputStream.close()
    return data
}

def pactProperties = loadYaml(file('src/main/resources/application.yml'))

test {
    systemProperty 'pact.provider.version', 'git rev-parse --verify --short HEAD'.execute().text.trim()+ '-PROVIDER'
    systemProperty 'pact.provider.branch', 'git rev-parse --abbrev-ref HEAD'.execute().text.trim()
    systemProperty 'pact.verifier.publishResults', 'true'
    systemProperty 'pactbroker.url', "${pactProperties['pactbroker']['url']}"
}

pact {
    broker {
        pactBrokerUrl = System.getProperty("pactbroker.url")
        retryCountWhileUnknown = 2
        retryWhileUnknownInterval = 5
    }
}

jacocoTestReport {
    dependsOn test
    reports {
        xml.required = true
        csv.required = false
        html.required = true
    }

    afterEvaluate {
        classDirectories = files(classDirectories.files.collect {
            fileTree(dir: it,
                    exclude: [
                            '**/DynamicLayoutAdapterApplication.class',
                            '**/config/**',
                            '**/constant/**',
                            '**/exception/**',
                            '**/feigns/**',
                            '**/dto/**',
                            '**/api/**'
                    ])
        })
    }
}

jacocoTestCoverageVerification {
    violationRules {
        rule {
            element = 'CLASS'
            excludes = [
                    '**.*DynamicLayoutAdapterApplication*',
                    '**/config/**',
                    '**/constant/**',
                    '**/exception/**',
                    '**/feigns/**',
                    '**/dto/**',
                    '**/api/**'
            ]

            limit {
                counter = 'LINE'
                value = 'COVEREDRATIO'
                minimum = 0.90
            }
        }
    }
}


springBoot {
    buildInfo()
}

sonarqube {
    properties {
        properties["sonar.exclusions"] = [
                '**/config/**/*',
                "**/feigns/**/*",
                '**/dto/**/*',
                '**/api/**/*'
        ]
    }
}

test.finalizedBy jacocoTestReport
