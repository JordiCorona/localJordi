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

    @Test
    void shouldReturnAllAdvisors() {
        AdvisorResponse response = advisorService.getAllAdvisors();
        assertThat(response.getAdvisors()).hasSize(3);
    }
}






@WebMvcTest(AdvisorController.class)
@AutoConfigureMockMvc(addFilters = false)
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

        Mockito.when(advisorService.getAdvisorStatuses(Mockito.any())).thenReturn(mockResponse);

        mockMvc.perform(post("/advisors/status")
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(objectMapper.writeValueAsString(request)))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.advisors[0].id").value("s123456"))
                .andExpect(jsonPath("$.advisors[0].status").value("online"));
    }
}
