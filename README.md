package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.model.Advisor;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponse;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponseAdvisorsInner;
import org.junit.jupiter.api.Test;

import java.util.Collections;

import static org.assertj.core.api.Assertions.assertThat;

class AdvisorServiceTest {

    private final AdvisorService advisorService = new AdvisorService();

    @Test
    void shouldReturnStatusForSingleAdvisor() {
        Advisor advisor = new Advisor().id("s123456");
        AdvisorRequest request = new AdvisorRequest()
                .advisors(Collections.singletonList(advisor));

        AdvisorResponse response = advisorService.getAdvisorStatuses(request);

        assertThat(response.getAdvisors()).hasSize(1);
        assertThat(response.getAdvisors().get(0).getId()).isEqualTo("s123456");
        assertThat(response.getAdvisors().get(0).getStatus())
                .isIn(AdvisorResponseAdvisorsInner.StatusEnum.ONLINE,
                      AdvisorResponseAdvisorsInner.StatusEnum.OFFLINE);
    }
}


package com.scotiabank.ib.advisor.monitor.service.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.scotiabank.ib.advisor.monitor.service.model.Advisor;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponse;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponseAdvisorsInner;
import com.scotiabank.ib.advisor.monitor.service.services.AdvisorService;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;

import java.util.Collections;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(AdvisorMonitorServiceController.class)
@AutoConfigureMockMvc(addFilters = false)
public class AdvisorMonitorServiceControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @Autowired
    private ObjectMapper objectMapper;

    @MockBean
    private AdvisorService advisorService;

    @Test
    void shouldReturnAdvisorStatusWithoutSecurity() throws Exception {
        AdvisorRequest request = new AdvisorRequest()
                .advisors(Collections.singletonList(new Advisor().id("s123456")));

        AdvisorResponse mockResponse = new AdvisorResponse()
                .advisors(Collections.singletonList(
                        new AdvisorResponseAdvisorsInner()
                                .id("s123456")
                                .status(AdvisorResponseAdvisorsInner.StatusEnum.ONLINE)
                ));

        Mockito.when(advisorService.getAdvisorStatuses(Mockito.any())).thenReturn(mockResponse);

        mockMvc.perform(post("/advisors/status")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(request)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.advisors[0].id").value("s123456"))
                .andExpect(jsonPath("$.advisors[0].status").value("ONLINE"));
    }
}
