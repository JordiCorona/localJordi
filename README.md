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
        private int count;

        public UnreadMessage(String advisorId, int count) {
            this.advisorId = advisorId;
            this.count = count;
        }

        public String getAdvisorId() {
            return advisorId;
        }

        public int getCount() {
            return count;
        }
    }
}


package com.scotiabank.ib.advisor.monitor.service.service;

import com.scotiabank.ib.advisor.monitor.service.dto.UnreadMessagesResponse;
import org.springframework.stereotype.Service;

import java.util.Arrays;

@Service
public class UnreadMessagesService {

    public UnreadMessagesResponse getUnreadMessages(String clientId) {
        // 🔹 Mock: devolvemos datos fijos
        return new UnreadMessagesResponse(
                clientId,
                Arrays.asList(
                        new UnreadMessagesResponse.UnreadMessage("s123401", 0),
                        new UnreadMessagesResponse.UnreadMessage("s123457", 1),
                        new UnreadMessagesResponse.UnreadMessage("s123458", 0)
                )
        );
    }
}


package com.scotiabank.ib.advisor.monitor.service.controller;

import com.scotiabank.ib.advisor.monitor.service.dto.UnreadMessagesResponse;
import com.scotiabank.ib.advisor.monitor.service.service.UnreadMessagesService;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/clients")
public class UnreadMessagesController {

    private final UnreadMessagesService unreadMessagesService;

    public UnreadMessagesController(UnreadMessagesService unreadMessagesService) {
        this.unreadMessagesService = unreadMessagesService;
    }

    @GetMapping("/{clientId}/messages/unread")
    public UnreadMessagesResponse getUnreadMessages(@PathVariable String clientId) {
        return unreadMessagesService.getUnreadMessages(clientId);
    }
}
