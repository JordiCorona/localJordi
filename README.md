Gracias, me puedes ayudar conesto ahora:
En mi desarrollo de mi servicio implemente estas clases mockeadas para simular el caso.

package com.scotiabank.ib.advisor.monitor.service.controller;

import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorResponse;
import com.scotiabank.ib.advisor.monitor.service.services.AdvisorService;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/advisors")
public class AdvisorMonitorServiceController {

    private final AdvisorService advisorService;

    public AdvisorMonitorServiceController(AdvisorService advisorService) {
        this.advisorService = advisorService;
    }

    @PostMapping("/status")
    public ResponseEntity<AdvisorResponse> getAdvisorStatus(@RequestBody AdvisorRequest request) {
        AdvisorResponse response = advisorService.getAdvisorStatuses(request);
        return ResponseEntity.ok(response);
    }
}

package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorResponse;
import org.springframework.stereotype.Service;
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

    private String mockStatus() {
        return random.nextBoolean() ? "online" : "offline";
    }
}

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

El tema es que mis clase DTO son generadas manualmente y me solcitan que estos datos los tome de las clases autogeneradas del open api, como realizo este ajuste? 

