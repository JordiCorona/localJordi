package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.model.UnreadMessagesResponse;
import com.scotiabank.ib.advisor.monitor.service.model.UnreadMessagesResponseUnreadMessagesInner;
import org.junit.jupiter.api.Test;

import java.util.List;

import static org.junit.jupiter.api.Assertions.*;

class UnreadMessagesServiceTest {

    private final UnreadMessagesService service = new UnreadMessagesService();

    @Test
    void testGetUnreadMessages() {
        String clientId = "Jordi";
        UnreadMessagesResponse response = service.getUnreadMessages(clientId);

        assertNotNull(response);
        assertEquals(clientId, response.getClientId());
        assertNotNull(response.getUnreadMessages());
        assertEquals(3, response.getUnreadMessages().size());

        List<UnreadMessagesResponseUnreadMessagesInner> unreadMessages = response.getUnreadMessages();

        assertEquals("s123401", unreadMessages.get(0).getAdvisorId());
        assertEquals("s123457", unreadMessages.get(1).getAdvisorId());
        assertEquals("s123458", unreadMessages.get(2).getAdvisorId());
    }
}



package com.scotiabank.ib.advisor.monitor.service.controller;

import com.scotiabank.ib.advisor.monitor.service.model.UnreadMessagesResponse;
import com.scotiabank.ib.advisor.monitor.service.model.UnreadMessagesResponseUnreadMessagesInner;
import com.scotiabank.ib.advisor.monitor.service.services.UnreadMessagesService;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;
import org.springframework.http.ResponseEntity;

import java.util.Collections;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

class UnreadMessagesControllerTest {

    @Test
    void testGetUnreadMessagesFromController() {
        // Arrange
        String clientId = "Jordi";
        UnreadMessagesResponse mockResponse = new UnreadMessagesResponse()
                .clientId(clientId)
                .unreadMessages(Collections.singletonList(
                        new UnreadMessagesResponseUnreadMessagesInner()
                                .advisorId("s123401")
                                .messages(true) // o .count(1) según contrato
                ));

        UnreadMessagesService mockService = Mockito.mock(UnreadMessagesService.class);
        when(mockService.getUnreadMessages(clientId)).thenReturn(mockResponse);

        UnreadMessagesController controller = new UnreadMessagesController(mockService);

        // Act
        ResponseEntity<UnreadMessagesResponse> response = controller.getUnreadMessages(clientId);

        // Assert
        assertNotNull(response);
        assertEquals(200, response.getStatusCodeValue());
        assertEquals(clientId, response.getBody().getClientId());
        assertEquals(1, response.getBody().getUnreadMessages().size());
        assertEquals("s123401", response.getBody().getUnreadMessages().get(0).getAdvisorId());
        assertTrue(response.getBody().getUnreadMessages().get(0).getMessages());
    }
}
