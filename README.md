Me puedes ayudar a ajustar mis clases de mis test unitarios con las clases autogeneradas por favor
package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.dto.*;
import org.junit.jupiter.api.Test;

import java.util.Collections;

import static org.assertj.core.api.Assertions.assertThat;

class AdvisorServiceTest {

    private final AdvisorService advisorService = new AdvisorService();

    @Test
    void shouldReturnStatusForSingleAdvisor() {
        AdvisorRequest.AdvisorId advisorId = new AdvisorRequest.AdvisorId("s123456");
        AdvisorRequest request = new AdvisorRequest();
        request.setAdvisors(Collections.singletonList(advisorId));

        AdvisorResponse response = advisorService.getAdvisorStatuses(request);

        assertThat(response.getAdvisors()).hasSize(1);
        assertThat(response.getAdvisors().get(0).getId()).isEqualTo("s123456");
        assertThat(response.getAdvisors().get(0).getStatus()).isIn("online", "offline");
    }

}

package com.scotiabank.ib.advisor.monitor.service.controller;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.dto.AdvisorResponse;
import com.scotiabank.ib.advisor.monitor.service.services.AdvisorService;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.MediaType;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.boot.test.autoconfigure.web.servlet.AutoConfigureMockMvc;
import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;


import java.util.Collections;


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
        AdvisorRequest request = new AdvisorRequest();
        request.setAdvisors(Collections.singletonList(new AdvisorRequest.AdvisorId("s123456")));

        AdvisorResponse mockResponse = new AdvisorResponse(
                Collections.singletonList(new AdvisorResponse.AdvisorStatus("s123456", "online"))
        );

        Mockito.when(advisorService.getAdvisorStatuses(Mockito.any())).thenReturn(mockResponse);

        mockMvc.perform(post("/advisors/status")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(request)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.advisors[0].id").value("s123456"))
                .andExpect(jsonPath("$.advisors[0].status").value("online"));
    }
}
