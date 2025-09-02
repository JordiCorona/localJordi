openapi: "3.0.2"
info:
  title: Advisor Service API
  version: "1.0"
servers:
  - url: http://localhost:8080
security:
  - bearerAuth: []
paths:
  /advisors/status:
    post:
      tags:
        - "advisors"
      summary: Get advisor statuses by IDs
      description: Receives a list of advisor IDs and returns their statuses (mocked data for now).
      operationId: getAdvisorStatusByIds
      security:
        - bearerAuth: []
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AdvisorRequest'
            examples:
              ApiRequestAdvisors:
                $ref: '#/components/examples/ApiRequestAdvisors'
      responses:
        '200':
          description: Successful response with advisor statuses
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AdvisorResponse'
              examples:
                ApiResponsePostAdvisors:
                  $ref: '#/components/examples/ApiResponsePostAdvisors'
        '400':
          description: Bad Request
          content:
            application/json:
              examples:
                ApiResponseError400Example:
                  $ref: '#/components/examples/ApiResponseError400Example'
        '500':
          description: Internal Server Error
          content:
            application/json:
              examples:
                ApiResponseError500Example:
                  $ref: '#/components/examples/ApiResponseError500Example'
        '401':
          description: Unauthorized
          content:
            application/json:
              examples:
                ApiResponseError401Example:
                  $ref: '#/components/examples/ApiResponseError401Example'

components:
  schemas:
    AdvisorRequest:
      type: object
      properties:
        advisors:
          type: array
          items:
            type: object
            properties:
              id:
                type: string
                example: "s123456"
    AdvisorResponse:
      type: object
      properties:
        advisors:
          type: array
          items:
            type: object
            properties:
              id:
                type: string
                example: "s123456"
              status:
                type: string
                enum: ["online", "offline"]
                example: "online"
  examples:
    ApiRequestAdvisors:
      value:
        advisors:
          - id: "s123456"
          - id: "s123457"
          - id: "s123458"
    ApiResponsePostAdvisors:
      value:
        advisors:
          - id: "s123456"
            status: "online"
          - id: "s123457"
            status: "offline"
          - id: "s123458"
            status: "online"
    ApiResponseError400Example:
      value:
        timestamp: "2025-01-21T14:29:00.000000"
        message: "Bad Request"
        details: ["Bad Request"]
    ApiResponseError401Example:
      value:
        timestamp: "2025-01-21T14:29:00.000000"
        message: "Unauthorized"
        details: ["Unauthorized"]
    ApiResponseError500Example:
      value:
        timestamp: "2025-01-21T14:29:00.000000"
        message: "Internal Server Error"
        details: ["Internal Server Error"]




        19:10:43  + curl --request POST --user 's6742985:****' --header 'Content-Type: application/json' --data '{"state":"INPROGRESS","key":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/","name":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/22107//console","url":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/22107/","description":"feature/IBPLATF-28863-despliegue-ist - 22107"}' --silent --insecure --url https://bitbucket.agile.bns/rest/build-status/1.0/commits/0e62897b942e4ec65ac5f3f4a057950a0df80e5f
[Pipeline] echo
19:10:43  Error:The JSON input text should neither be null nor empty.


19:13:35  Applying command with credential: ib-atlasian-sa-creds-secret
[Pipeline] sh (Curl url: https://bitbucket.agile.bns/rest/api/1.0/projects/IBSRVC/repos/ib-advisor-monitor-service/)
19:13:36  + curl --request POST --user 's6742985:****' --header 'Content-Type: application/json' --data '{"text":null}' --silent --insecure --url https://bitbucket.agile.bns/rest/api/1.0/projects/IBSRVC/repos/ib-advisor-monitor-service/pull-requests/4/comments
[Pipeline] }
[Pipeline] // withCredentials
[Pipeline] echo
19:13:36  responseJson: [errors:[[context:null, message:Text must be provided when adding a comment., exceptionName:null]]]
[Pipeline] echo
19:13:36  
19:13:36              latestCommit: 0e62897b942e4ec65ac5f3f4a057950a0df80e5f
19:13:36              bitbucketSourceBranch: feature/IBPLATF-28863-despliegue-ist
19:13:36              description: feature/IBPLATF-28863-despliegue-ist - 22107
19:13:36          
[Pipeline] withCredentials
19:13:36  Masking supported pattern matches of $password
[Pipeline] {
[Pipeline] echo
19:13:36  Applying command with credential: ib-atlasian-sa-creds-secret
[Pipeline] sh (Curl url: https://bitbucket.agile.bns/rest/build-status/1.0/commits/0e62897b942e4ec65ac5f3f4a057950a)
19:13:37  + curl --request POST --user 's6742985:****' --header 'Content-Type: application/json' --data '{"state":"FAILED","key":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/","name":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/22107//console","url":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/22107/","description":"feature/IBPLATF-28863-despliegue-ist - 22107"}' --silent --insecure --url https://bitbucket.agile.bns/rest/build-status/1.0/commits/0e62897b942e4ec65ac5f3f4a057950a0df80e5f
[Pipeline] echo
19:13:37  Error:The JSON input text should neither be null nor empty.
[Pipeline] }
