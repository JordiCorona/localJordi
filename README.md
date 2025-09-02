import com.fasterxml.jackson.databind.ObjectMapper;
import com.scotiabank.ib.advisor.monitor.service.controller.AdvisorController;
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

import java.util.Collections;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.post;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.*;

@WebMvcTest(AdvisorController.class)
@AutoConfigureMockMvc(addFilters = false) // <- Desactiva seguridad en tests
class AdvisorControllerTest {

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

        Mockito.when(advisorService.getAdvisorStatuses(Mockito.any(AdvisorRequest.class)))
                .thenReturn(mockResponse);

        mockMvc.perform(post("/advisors/status")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(request)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.advisors[0].id").value("s123456"))
                .andExpect(jsonPath("$.advisors[0].status").value("online"));
    }
}



spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: http://dummy
          jwk-set-uri: http://dummy
server:
  port: 0
logging:
  level:
    root: WARN
    com.scotiabank.ib.advisor.monitor: DEBUG

