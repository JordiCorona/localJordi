19:10:43  Masking supported pattern matches of $password
[Pipeline] {
[Pipeline] echo
19:10:43  Applying command with credential: ib-atlasian-sa-creds-secret
[Pipeline] sh (Curl url: https://bitbucket.agile.bns/rest/build-status/1.0/commits/0e62897b942e4ec65ac5f3f4a057950a)
19:10:43  + curl --request POST --user 's6742985:****' --header 'Content-Type: application/json' --data '{"state":"INPROGRESS","key":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/","name":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/22107//console","url":"https://jenkins.dev.cl-devops-infra.chl.bns/job/IBPLATF-DELIVERY/job/PR-validation/22107/","description":"feature/IBPLATF-28863-despliegue-ist - 22107"}' --silent --insecure --url https://bitbucket.agile.bns/rest/build-status/1.0/commits/0e62897b942e4ec65ac5f3f4a057950a0df80e5f
[Pipeline] echo
19:10:43  Error:The JSON input text should neither be null nor empty.

Did you forget the `def` keyword? getValueOfFile seems to be setting a field named value (to a value of type String) which could lead to memory leaks or other issues.


package com.scotiabank.ib.advisor.monitor.service.controller;

import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorResponse;
import com.scotiabank.ib.advisor.monitor.service.services.AdvisorService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/advisors")
public class AdvisorController {

    private final AdvisorService advisorService;

    public AdvisorController(AdvisorService advisorService) {
        this.advisorService = advisorService;
    }

    @PostMapping("/status")
    public ResponseEntity<AdvisorResponse> getAdvisorStatus(@RequestBody AdvisorRequest request) {
        AdvisorResponse response = advisorService.getAdvisorStatuses(request);
        return ResponseEntity.ok(response);
    }

}

------------------------------
package com.scotiabank.ib.advisor.monitor.service.dto;

import java.util.List;

public class AdvisorRequest {
    private List<AdvisorId> advisors;

    public List<AdvisorId> getAdvisors() {
        return advisors;
    }

    public void setAdvisors(List<AdvisorId> advisors) {
        this.advisors = advisors;
    }

    public static class AdvisorId {
        private String id;

        public AdvisorId() {}

        public AdvisorId(String id) {
            this.id = id;
        }

        public String getId() {
            return id;
        }

        public void setId(String id) {
            this.id = id;
        }
    }
}

----------------------------------
package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.dto.*;
import org.springframework.stereotype.Service;

import java.util.Arrays;
import java.util.List;
import java.util.Random;
import java.util.stream.Collectors;

@Service
public class AdvisorService {

    private final Random random = new Random();

    public AdvisorResponse getAdvisorStatuses(AdvisorRequest request) {
        List<AdvisorResponse.AdvisorStatus> advisorList = request.getAdvisors()
                .stream()
                .map(a -> new AdvisorResponse.AdvisorStatus(
                        a.getId(),
                        mockStatus()   // genera "online" o "offline"
                ))
                .collect(Collectors.toList());

        return new AdvisorResponse(advisorList);
    }

    public AdvisorResponse getAllAdvisors(){
        List<AdvisorResponse.AdvisorStatus> advisorList = Arrays.asList(
                new AdvisorResponse.AdvisorStatus("s123456", mockStatus()),
                new AdvisorResponse.AdvisorStatus("s123457", mockStatus()),
                new AdvisorResponse.AdvisorStatus("s123458", mockStatus())
        );
        return new AdvisorResponse(advisorList);
        }

    private String mockStatus() {
        return random.nextBoolean() ? "online" : "offline";
    }
}
------------------------------
package com.scotiabank.ib.advisor.monitor.service.dto;

import java.util.List;

public class AdvisorResponse {
    private List<AdvisorStatus> advisors;

    public AdvisorResponse(List<AdvisorStatus> advisors) {
        this.advisors = advisors;
    }

    public List<AdvisorStatus> getAdvisors() {
        return advisors;
    }

    public void setAdvisors(List<AdvisorStatus> advisors) {
        this.advisors = advisors;
    }

    public static class AdvisorStatus {
        private String id;
        private String status;

        public AdvisorStatus(String id, String status) {
            this.id = id;
            this.status = status;
        }

        public String getId() {
            return id;
        }

        public String getStatus() {
            return status;
        }
    }
}

