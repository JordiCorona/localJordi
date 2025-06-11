Creado por Ivan Casanova Rojas, modificado por última vez en mar. 21, 2025



1. BJFT Instrumentation
Requirements for a Journey to emit ESLM Logs
2. BJFS Instrumentation
3. BDRQ Instrumentation
3. Standard Events Tags 
4. Examples
Currently, as the Regionalization team, we have implemented ESLM for three applications within our ecosystem: BJFT, BJFS and BDRQ. To provide context about which components are sending security logs to Balabit (ESLM) we have created the following context diagram.


 
ESLM for IB Regionalization Applications
As a reader you can go to the following sections for get information about how and what we implemented ESLM in each APM.

1. BJFT Instrumentation
At BJFT, we track user events that occur throughout our journey components. This approach is used because these components serve as the layer that receives requests from our front-ends, thus capturing data such as the browser or device used to make the requests, this data is a should  in the ESLM format.

Requirements for a Journey to emit ESLM Logs
If the journey or functionality save personal information of a prospective/potential customer
If the journey creates/modifies/deletes information of an user registered in the system.
If you want to inform that some process is finished in your journey.
If your journey meets any of those requirements, it must implement security logs as follow:

a) In src/common/constants folder create log.constants.ts file an put the following as a content

export const BUSINESS_TAG = {
  tags: ['SIEMEvent', 'SIEM_BUSINESS_LOG'],
  siem: true,
  success: true,
};
  
export const BUSINESS_TAG_FAILED = {
  tags: ['SIEMEvent', 'SIEM_BUSINESS_LOG'],
  siem: true,
  success: false,
};
 
export const TEMP_DATA_TAG = {
  tags: ['SIEMEvent', 'TEMP_DATA_TAG'],
  siem: true
};

You will use BUSINESS_TAG when is an user event or a finished step in the journey, BUSINESS_TAG_FAILED when something is wrong in the process and TEMP_DATA_TAG when temporaly personal data is stored.

b) In your Service, just below your logger object, create another one called loggerESLM just for ESLM like this

@Injectable()
export class AuthorizationService {
private readonly logger = new Logger(AuthorizationService.name);
private readonly loggerESLM = new Logger();
c) Log the event, if you have the user_id log it as a BUSINESS_TAG event, if not, use just the requestId with TEMP_DATA_TAG. Look the next examples

BUSINESS LOG
this.loggerESLM.log(
      `Authentication ${dataWorkflow?.finalStateReportedToOrchestrator} nextStep ${nextStep} for requestId: ${requestId}`,
      BUSINESS_TAG,
);
BUSINESS ERROR LOG
this.loggerESLM.log(
        `Authentication Failed ${codeError.code} ${codeError.description} for requestId: ${requestId}`,
        BUSINESS_TAG_FAILED,
      );
TEMPORAL PERSONAL DATA SAVED
this.loggerESLM.log(
        `Identity data have been updated for requestId: ${requestId}`,
        TEMP_DATA_TAG,
      );

Finally these are some examples of output logs.

2. BJFS Instrumentation
For BJFS that is the anthos cluster for IB, we're sending all audit logs that kubernetes generates. The configuration is located here:

An error occured

The Bitbucket resource could not be found. Check the file path provided and if the resource is from a private repo, ensure you have access. The Include Bitbucket configuration may need to be reviewed. Please contact your system administrator to do so.

. If you can't see the file there is the config:

[OUTPUT]
    name                 syslog
    match                k8s_audit.*
    host                 192.168.195.24
    port                 17235
    mode                 tcp
    syslog_format        rfc3164
    syslog_maxsize       2048
    syslog_hostname_key  k8s_host
    syslog_appname_key   k8s_pod_name
    syslog_message_key   log


3. BDRQ Instrumentation
This is an example of ESLM logs emitted for an application:


ESLM example
{
  "ts": "2023-09-27T18:55:03.886Z",
  "uuid": "1064b6c1-2283-4b91-a4b6-c122837b9123",
  "type": "INFO",
  "msg": "",
  "class": "com.bns.chassis.siem.util.SiemLoggerUtility",
  "pii": false,
  "thread": "http-nio-8080-exec-4",
  "mdc": {},
  "data": {},
  "tags": [
    "SIEMEvent",
    "SIEM_ACCESS_LOG"
  ],
  "siemmsg": "customer-indicators:\"UID:f293fc99-2cbc-40bd-8d17-cdd787da73e0\",\"LTSP:2023-09-27 14:55:03-0886 EDT\",\"ELCS:0D\",\"ETSP:2023-09-27 14:55:03-0886 EDT\",\"ETYP:20\",\"EPRI:00\",\"ECAT:00\",\"EMSG:Accessing Resource : /health, HTTP METHOD : GET, Configured Parameters : {}\",\"SHST:127.0.0.6\",\"SIPA:127.0.0.6\",\"DHST:customer-cards-418a6bc-fdf5b9546-wzw88\",\"DIPA:10.118.156.72\",\"ACTN:NULL\",\"SPRT:58855\",\"PRCN:1@customer-cards-418a6bc-fdf5b9546-wzw88 Java\",\"SID:NULL\",\"ACCA:00\",\"SHA256:cd12260b3456be49f01fc60e3bc7ae238a91e0cd250eaf8d35456532d6a8f0cf\"",
  "siem": true,
  "trace": null,
  "span": null
}




3. Standard Events Tags 
The next tags must be used depending on the kind of event you want to log, bear in mind that each msg must have the requestId for traceability purposes.



Tag	Description	Msg	Description
Tag	Description	Msg	Description
SIEM_BUSINESS_LOG	Tag for business events or errors	
KYC_ADDRESS failed Error en paso de edicion de dirección de domicilio por parte del cliente for requestId XXX

Error
KYC_JOB_INFO failed Error al capturar información laboral for requestId XXX

Error
Authentication undefined nextStep v1/regulatory/init for requestId XXX

Authentication Error
CURRENT_STEP nextStep NEXT_STEP for requestId XXX

User have finished some CURRENT_STEP and it is heading to a NEXT_STEP
TEMP_DATA_TAG











Information for prospects customers	Cellphone and email have been saved for requestId XXX	
Notifies event in identity verification step

KYC_JOB_INFO have been saved for requestId XXX

Notifies event in KYC step

KYC_REGULATORY_DATA have been saved for requestId XXX

Notifies event in KYC step

Personal data have been saved for requestId XXX

Notifies event in Customer Validation

Account information have been saved for requestId: XXXXXXX

Customer information have been saved for requestId: XXXXXXX

Participants information have been saved for requestId: XXXXXX

Notifies events in Fulfulment step

Enrollment for userId XXX requestId: XXX has succeeded

Notifies that prospect customer have finished the onboarding

4. Examples
{""level"":30,""time"":""2025-03-21T15:36:00.369Z"",""pid"":1,""hostname"":""ib-kyc-journey-6f64ddd746-65dqd"",""name"":""ib-platform-ib-kyc-journey-consumer"",""req"":{""id"":49689,""method"":""POST"",""url"":""/job-information/complete""},""context"":{""tags"":[""SIEMEvent"",""SIEM_BUSINESS_LOG""],""siem"":true,""action_status"":""success""},""spanId"":""c78c8a5768404711"",""traceId"":""0000000000000000da3dadd1b9bc8712"",""msg"":""KYC_JOB_INFO nextStep JOB_INFO_EMPLOYMENT_STEP for requestId: 720250321153558832934""}","EPM=BJFT,APP=Customer_Onboarding_Regional_Platform_MVP_Anthos,PORT=17225"
{""level"":30,""time"":""2025-03-21T15:35:54.699Z"",""pid"":1,""hostname"":""ib-kyc-journey-6f64ddd746-65dqd"",""name"":""ib-platform-ib-kyc-journey-consumer"",""req"":{""id"":49625,""method"":""POST"",""url"":""/job-information/complete""},""context"":{""tags"":[""SIEMEvent"",""SIEM_BUSINESS_LOG""],""siem"":true,""action_status"":""success""},""spanId"":""bc8f52338c3dadc3"",""traceId"":""0000000000000000d3aa085a10b1ca63"",""msg"":""KYC_JOB_INFO nextStep JOB_INFO_EMPLOYMENT_STEP for requestId: 720250321153552937663""}","EPM=BJFT,APP=Customer_Onboarding_Regional_Platform_MVP_Anthos,PORT=17225"
2025-03-21 00:00:00.000000 UTC,2025-03-21T15:35:54-04:00,ib-ist-pool-1-ext-84b76fc5d7-xfsqv,ib-kyc-journey-6f64ddd746-65dqd,"
