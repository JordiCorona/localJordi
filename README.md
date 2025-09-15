package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.model.UnreadMessagesResponse;
import com.scotiabank.ib.advisor.monitor.service.model.UnreadMessagesResponseUnreadMessagesInner;
import org.springframework.stereotype.Service;

import java.util.Arrays;

@Service
public class UnreadMessagesService {

    public UnreadMessagesResponse getUnreadMessages(String clientId) {
        return new UnreadMessagesResponse()
                .clientId(clientId)
                .unreadMessages(Arrays.asList(
                        new UnreadMessagesResponseUnreadMessagesInner()
                                .advisorId("s123401")
                                .count(0),   // según tu contrato puede ser count o boolean
                        new UnreadMessagesResponseUnreadMessagesInner()
                                .advisorId("s123457")
                                .count(1),
                        new UnreadMessagesResponseUnreadMessagesInner()
                                .advisorId("s123458")
                                .count(2)
                ));
    }
}
