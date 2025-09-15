Puedes ayudarme a analizar esta clase api.yml

openapi: 3.0.2
info:
  title: Advisor Monitor Service
  description: >
    Este microservicio permite monitorear el estado de los asesores registrados en el sistema. 
    Permite consultar el estado de conexión (online/offline) de uno o varios asesores mediante sus IDs, 
    lo que facilita el monitoreo en tiempo real y la integración con otros sistemas de reporting.
  version: 1.0.0

servers:
  - url: http://localhost:8080
    description: Servidor local de desarrollo

security:
  - oauth2:
      - advisor.read

paths:
  /advisors/status:
    post:
      tags:
        - "Advisors"
      summary: Consultar el estado de los asesores
      description: >
        Este endpoint recibe una lista de IDs de asesores y devuelve el estado de cada uno (online/offline). 
        Se utiliza principalmente para monitoreo y reporting de la disponibilidad de asesores. 
        Los datos son mockeados actualmente pero se espera integrarlos con el sistema de estado real.
      operationId: getAdvisorStatusByIds
      security:
        - oauth2:
            - advisor.read
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AdvisorRequest'
            examples:
              ApiRequestAdvisors:
                $ref: '#/components/examples/ApiRequestAdvisors'
      responses:
        '200':
          description: Respuesta exitosa con el estado de los asesores
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/AdvisorResponse'
              examples:
                ApiResponsePostAdvisors:
                  $ref: '#/components/examples/ApiResponsePostAdvisors'
        '400':
          description: Solicitud incorrecta (Bad Request)
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                ApiResponseError400Example:
                  $ref: '#/components/examples/ApiResponseError400Example'
        '401':
          description: No autorizado
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                ApiResponseError401Example:
                  $ref: '#/components/examples/ApiResponseError401Example'
        '500':
          description: Error interno del servidor
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ErrorResponse'
              examples:
                ApiResponseError500Example:
                  $ref: '#/components/examples/ApiResponseError500Example'

components:
  securitySchemes:
    oauth2:
      type: oauth2
      flows:
        clientCredentials:
          tokenUrl: https://auth.example.com/oauth/token
          scopes:
            advisor.read: "Permite leer el estado de los asesores"
            advisor.write: "Permite modificar o actualizar datos de asesores"

  schemas:
    Advisor:
      type: object
      properties:
        id:
          type: string
          example: "s123456"

    AdvisorRequest:
      type: object
      properties:
        advisors:
          type: array
          items:
            $ref: '#/components/schemas/Advisor'

    AdvisorResponse:
      type: object
      properties:
        advisors:
          type: array
          items:
            allOf:
              - $ref: '#/components/schemas/Advisor'
              - type: object
                properties:
                  status:
                    type: string
                    enum: ["ONLINE", "OFFLINE"]
                    description: Estado de conexión del asesor
                    example: "online"

    ErrorResponse:
      type: object
      properties:
        timestamp:
          type: string
          format: date-time
        message:
          type: string
        details:
          type: array
          items:
            type: string

  examples:
    ApiRequestAdvisors:
      value:
        advisors:
          - id: "s123456"
          - id: "s123457"
          - id: "s123458"

    ApiResponsePostAdvisors:
      value:
        advisors:
          - id: "s123456"
            status: "online"
          - id: "s123457"
            status: "offline"
          - id: "s123458"
            status: "online"

    ApiResponseError400Example:
      value:
        timestamp: "2025-01-21T14:29:00Z"
        message: "Bad Request"
        details: ["Solicitud inválida"]

    ApiResponseError401Example:
      value:
        timestamp: "2025-01-21T14:29:00Z"
        message: "Unauthorized"
        details: ["Token no válido o expirado"]

    ApiResponseError500Example:
      value:
        timestamp: "2025-01-21T14:29:00Z"
        message: "Internal Server Error"
        details: ["Error interno en el servidor"]


El servicio que estoy construyendo son las siguientes:
package com.scotiabank.ib.advisor.monitor.service.controller;

import com.scotiabank.ib.advisor.monitor.service.model.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponse;
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

import com.scotiabank.ib.advisor.monitor.service.model.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponse;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponseAdvisorsInner;
import org.springframework.stereotype.Service;

import java.util.List;
import java.util.Random;
import java.util.stream.Collectors;

@Service
public class AdvisorService {

    private final Random random = new Random();

    public AdvisorResponse getAdvisorStatuses(AdvisorRequest request) {
        List<AdvisorResponseAdvisorsInner> advisorList = request.getAdvisors()
                .stream()
                .map(a -> new AdvisorResponseAdvisorsInner()
                        .id(a.getId())
                        .status(mockStatus()))
                .collect(Collectors.toList());

        return new AdvisorResponse().advisors(advisorList);
    }

    private AdvisorResponseAdvisorsInner.StatusEnum mockStatus() {
        return random.nextBoolean()
                ? AdvisorResponseAdvisorsInner.StatusEnum.ONLINE
                : AdvisorResponseAdvisorsInner.StatusEnum.OFFLINE;
    }
}

Me puedes ayudar a que las clases que te comparti consuman las clases autogeneradas del open api 

Las clases son:
Api
AdvisorApi
ApiUtil

Model:
Advisor
AdvisorRequest
AdvisorResponse
AdvisorResponseAdvisorInner
AdvisorResponseAdvisorInnerAllOf
ErrorResponse

Requiero que mi controlador y mi service consuman estas clases, tener en cuenta el scop de seguridad.
