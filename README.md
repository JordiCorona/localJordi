Te comparto las clases del unreadMessages, me puedes ayudar a consumir clases autogeneradas.
De esta manera se estan consumiendo dto manuales, requiero que consumasn el open api


package com.scotiabank.ib.advisor.monitor.service.controller;

import com.scotiabank.ib.advisor.monitor.service.dto.UnreadMessagesResponse;
import com.scotiabank.ib.advisor.monitor.service.api.MessagesApi;
import com.scotiabank.ib.advisor.monitor.service.services.UnreadMessagesService;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/clients")
public class UnreadMessagesController implements MessagesApi {

    private final UnreadMessagesService unreadMessagesService;

    public UnreadMessagesController(UnreadMessagesService unreadMessagesService) {
        this.unreadMessagesService = unreadMessagesService;
    }

    @GetMapping("/unread/messages/{clientId}")
    public UnreadMessagesResponse getUnreadMessages(@PathVariable String clientId) {
        return unreadMessagesService.getUnreadMessages(clientId);
    }
}


package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.dto.UnreadMessagesResponse;
import org.springframework.stereotype.Service;

import java.util.Arrays;

@Service
public class UnreadMessagesService {

    public UnreadMessagesResponse getUnreadMessages(String clientId) {
        return new UnreadMessagesResponse(
                clientId,
                Arrays.asList(
                        new UnreadMessagesResponse.UnreadMessage("s123401", false),
                        new UnreadMessagesResponse.UnreadMessage("s123457", true),
                        new UnreadMessagesResponse.UnreadMessage("s123458", false)
                )
        );
    }
}

package com.scotiabank.ib.advisor.monitor.service.dto;

import java.util.List;

public class UnreadMessagesResponse {
    private String clientId;
    private List<UnreadMessage> unreadMessages;

    public UnreadMessagesResponse(String clientId, List<UnreadMessage> unreadMessages) {
        this.clientId = clientId;
        this.unreadMessages = unreadMessages;
    }

    public String getClientId() {
        return clientId;
    }

    public List<UnreadMessage> getUnreadMessages() {
        return unreadMessages;
    }

    public static class UnreadMessage {
        private String advisorId;
        private boolean messages;

        public UnreadMessage(String advisorId, boolean messages) {
            this.advisorId = advisorId;
            this.messages = messages;
        }

        public String getAdvisorId() {
            return advisorId;
        }

        public boolean getMessages() {
            return messages;
        }
    }
}
