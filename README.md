Poir que me da error esta clase?

package com.scotiabank.ib.advisor.monitor.service.services;

import com.scotiabank.ib.advisor.monitor.service.model.AdvisorRequest;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponse;
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

Te comparto mis clases generadas con openapi
package com.scotiabank.ib.advisor.monitor.service.model;

import java.net.URI;
import java.util.Objects;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonCreator;
import org.openapitools.jackson.nullable.JsonNullable;
import java.time.OffsetDateTime;
import jakarta.validation.Valid;
import jakarta.validation.constraints.*;
import io.swagger.v3.oas.annotations.media.Schema;


import java.util.*;
import jakarta.annotation.Generated;

/**
 * Advisor
 */

@Generated(value = "org.openapitools.codegen.languages.SpringCodegen", date = "2025-09-10T19:33:54.389991100-06:00[America/Mexico_City]")
public class Advisor {

  private String id;

  public Advisor id(String id) {
    this.id = id;
    return this;
  }

  /**
   * Get id
   * @return id
  */
  
  @Schema(name = "id", example = "s123456", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("id")
  public String getId() {
    return id;
  }

  public void setId(String id) {
    this.id = id;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    Advisor advisor = (Advisor) o;
    return Objects.equals(this.id, advisor.id);
  }

  @Override
  public int hashCode() {
    return Objects.hash(id);
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("class Advisor {\n");
    sb.append("    id: ").append(toIndentedString(id)).append("\n");
    sb.append("}");
    return sb.toString();
  }

  /**
   * Convert the given object to string with each line indented by 4 spaces
   * (except the first line).
   */
  private String toIndentedString(Object o) {
    if (o == null) {
      return "null";
    }
    return o.toString().replace("\n", "\n    ");
  }
}

package com.scotiabank.ib.advisor.monitor.service.model;

import java.net.URI;
import java.util.Objects;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonCreator;
import com.scotiabank.ib.advisor.monitor.service.model.Advisor;
import java.util.ArrayList;
import java.util.List;
import org.openapitools.jackson.nullable.JsonNullable;
import java.time.OffsetDateTime;
import jakarta.validation.Valid;
import jakarta.validation.constraints.*;
import io.swagger.v3.oas.annotations.media.Schema;


import java.util.*;
import jakarta.annotation.Generated;

/**
 * AdvisorRequest
 */

@Generated(value = "org.openapitools.codegen.languages.SpringCodegen", date = "2025-09-10T19:33:54.389991100-06:00[America/Mexico_City]")
public class AdvisorRequest {

  @Valid
  private List<@Valid Advisor> advisors;

  public AdvisorRequest advisors(List<@Valid Advisor> advisors) {
    this.advisors = advisors;
    return this;
  }

  public AdvisorRequest addAdvisorsItem(Advisor advisorsItem) {
    if (this.advisors == null) {
      this.advisors = new ArrayList<>();
    }
    this.advisors.add(advisorsItem);
    return this;
  }

  /**
   * Get advisors
   * @return advisors
  */
  @Valid 
  @Schema(name = "advisors", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("advisors")
  public List<@Valid Advisor> getAdvisors() {
    return advisors;
  }

  public void setAdvisors(List<@Valid Advisor> advisors) {
    this.advisors = advisors;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    AdvisorRequest advisorRequest = (AdvisorRequest) o;
    return Objects.equals(this.advisors, advisorRequest.advisors);
  }

  @Override
  public int hashCode() {
    return Objects.hash(advisors);
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("class AdvisorRequest {\n");
    sb.append("    advisors: ").append(toIndentedString(advisors)).append("\n");
    sb.append("}");
    return sb.toString();
  }

  /**
   * Convert the given object to string with each line indented by 4 spaces
   * (except the first line).
   */
  private String toIndentedString(Object o) {
    if (o == null) {
      return "null";
    }
    return o.toString().replace("\n", "\n    ");
  }
}

package com.scotiabank.ib.advisor.monitor.service.model;

import java.net.URI;
import java.util.Objects;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonCreator;
import com.scotiabank.ib.advisor.monitor.service.model.AdvisorResponseAdvisorsInner;
import java.util.ArrayList;
import java.util.List;
import org.openapitools.jackson.nullable.JsonNullable;
import java.time.OffsetDateTime;
import jakarta.validation.Valid;
import jakarta.validation.constraints.*;
import io.swagger.v3.oas.annotations.media.Schema;


import java.util.*;
import jakarta.annotation.Generated;

/**
 * AdvisorResponse
 */

@Generated(value = "org.openapitools.codegen.languages.SpringCodegen", date = "2025-09-10T19:33:54.389991100-06:00[America/Mexico_City]")
public class AdvisorResponse {

  @Valid
  private List<@Valid AdvisorResponseAdvisorsInner> advisors;

  public AdvisorResponse advisors(List<@Valid AdvisorResponseAdvisorsInner> advisors) {
    this.advisors = advisors;
    return this;
  }

  public AdvisorResponse addAdvisorsItem(AdvisorResponseAdvisorsInner advisorsItem) {
    if (this.advisors == null) {
      this.advisors = new ArrayList<>();
    }
    this.advisors.add(advisorsItem);
    return this;
  }

  /**
   * Get advisors
   * @return advisors
  */
  @Valid 
  @Schema(name = "advisors", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("advisors")
  public List<@Valid AdvisorResponseAdvisorsInner> getAdvisors() {
    return advisors;
  }

  public void setAdvisors(List<@Valid AdvisorResponseAdvisorsInner> advisors) {
    this.advisors = advisors;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    AdvisorResponse advisorResponse = (AdvisorResponse) o;
    return Objects.equals(this.advisors, advisorResponse.advisors);
  }

  @Override
  public int hashCode() {
    return Objects.hash(advisors);
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("class AdvisorResponse {\n");
    sb.append("    advisors: ").append(toIndentedString(advisors)).append("\n");
    sb.append("}");
    return sb.toString();
  }

  /**
   * Convert the given object to string with each line indented by 4 spaces
   * (except the first line).
   */
  private String toIndentedString(Object o) {
    if (o == null) {
      return "null";
    }
    return o.toString().replace("\n", "\n    ");
  }
}

package com.scotiabank.ib.advisor.monitor.service.model;

import java.net.URI;
import java.util.Objects;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonTypeName;
import com.fasterxml.jackson.annotation.JsonValue;
import org.openapitools.jackson.nullable.JsonNullable;
import java.time.OffsetDateTime;
import jakarta.validation.Valid;
import jakarta.validation.constraints.*;
import io.swagger.v3.oas.annotations.media.Schema;


import java.util.*;
import jakarta.annotation.Generated;

/**
 * AdvisorResponseAdvisorsInner
 */

@JsonTypeName("AdvisorResponse_advisors_inner")
@Generated(value = "org.openapitools.codegen.languages.SpringCodegen", date = "2025-09-10T19:33:54.389991100-06:00[America/Mexico_City]")
public class AdvisorResponseAdvisorsInner {

  private String id;

  /**
   * Estado de conexión del asesor
   */
  public enum StatusEnum {
    ONLINE("online"),
    
    OFFLINE("offline");

    private String value;

    StatusEnum(String value) {
      this.value = value;
    }

    @JsonValue
    public String getValue() {
      return value;
    }

    @Override
    public String toString() {
      return String.valueOf(value);
    }

    @JsonCreator
    public static StatusEnum fromValue(String value) {
      for (StatusEnum b : StatusEnum.values()) {
        if (b.value.equals(value)) {
          return b;
        }
      }
      throw new IllegalArgumentException("Unexpected value '" + value + "'");
    }
  }

  private StatusEnum status;

  public AdvisorResponseAdvisorsInner id(String id) {
    this.id = id;
    return this;
  }

  /**
   * Get id
   * @return id
  */
  
  @Schema(name = "id", example = "s123456", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("id")
  public String getId() {
    return id;
  }

  public void setId(String id) {
    this.id = id;
  }

  public AdvisorResponseAdvisorsInner status(StatusEnum status) {
    this.status = status;
    return this;
  }

  /**
   * Estado de conexión del asesor
   * @return status
  */
  
  @Schema(name = "status", example = "online", description = "Estado de conexión del asesor", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("status")
  public StatusEnum getStatus() {
    return status;
  }

  public void setStatus(StatusEnum status) {
    this.status = status;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    AdvisorResponseAdvisorsInner advisorResponseAdvisorsInner = (AdvisorResponseAdvisorsInner) o;
    return Objects.equals(this.id, advisorResponseAdvisorsInner.id) &&
        Objects.equals(this.status, advisorResponseAdvisorsInner.status);
  }

  @Override
  public int hashCode() {
    return Objects.hash(id, status);
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("class AdvisorResponseAdvisorsInner {\n");
    sb.append("    id: ").append(toIndentedString(id)).append("\n");
    sb.append("    status: ").append(toIndentedString(status)).append("\n");
    sb.append("}");
    return sb.toString();
  }

  /**
   * Convert the given object to string with each line indented by 4 spaces
   * (except the first line).
   */
  private String toIndentedString(Object o) {
    if (o == null) {
      return "null";
    }
    return o.toString().replace("\n", "\n    ");
  }
}

package com.scotiabank.ib.advisor.monitor.service.model;

import java.net.URI;
import java.util.Objects;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonCreator;
import com.fasterxml.jackson.annotation.JsonTypeName;
import com.fasterxml.jackson.annotation.JsonValue;
import org.openapitools.jackson.nullable.JsonNullable;
import java.time.OffsetDateTime;
import jakarta.validation.Valid;
import jakarta.validation.constraints.*;
import io.swagger.v3.oas.annotations.media.Schema;


import java.util.*;
import jakarta.annotation.Generated;

/**
 * AdvisorResponseAdvisorsInnerAllOf
 */

@JsonTypeName("AdvisorResponse_advisors_inner_allOf")
@Generated(value = "org.openapitools.codegen.languages.SpringCodegen", date = "2025-09-10T19:33:54.389991100-06:00[America/Mexico_City]")
public class AdvisorResponseAdvisorsInnerAllOf {

  /**
   * Estado de conexión del asesor
   */
  public enum StatusEnum {
    ONLINE("online"),
    
    OFFLINE("offline");

    private String value;

    StatusEnum(String value) {
      this.value = value;
    }

    @JsonValue
    public String getValue() {
      return value;
    }

    @Override
    public String toString() {
      return String.valueOf(value);
    }

    @JsonCreator
    public static StatusEnum fromValue(String value) {
      for (StatusEnum b : StatusEnum.values()) {
        if (b.value.equals(value)) {
          return b;
        }
      }
      throw new IllegalArgumentException("Unexpected value '" + value + "'");
    }
  }

  private StatusEnum status;

  public AdvisorResponseAdvisorsInnerAllOf status(StatusEnum status) {
    this.status = status;
    return this;
  }

  /**
   * Estado de conexión del asesor
   * @return status
  */
  
  @Schema(name = "status", example = "online", description = "Estado de conexión del asesor", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("status")
  public StatusEnum getStatus() {
    return status;
  }

  public void setStatus(StatusEnum status) {
    this.status = status;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    AdvisorResponseAdvisorsInnerAllOf advisorResponseAdvisorsInnerAllOf = (AdvisorResponseAdvisorsInnerAllOf) o;
    return Objects.equals(this.status, advisorResponseAdvisorsInnerAllOf.status);
  }

  @Override
  public int hashCode() {
    return Objects.hash(status);
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("class AdvisorResponseAdvisorsInnerAllOf {\n");
    sb.append("    status: ").append(toIndentedString(status)).append("\n");
    sb.append("}");
    return sb.toString();
  }

  /**
   * Convert the given object to string with each line indented by 4 spaces
   * (except the first line).
   */
  private String toIndentedString(Object o) {
    if (o == null) {
      return "null";
    }
    return o.toString().replace("\n", "\n    ");
  }
}

package com.scotiabank.ib.advisor.monitor.service.model;

import java.net.URI;
import java.util.Objects;
import com.fasterxml.jackson.annotation.JsonProperty;
import com.fasterxml.jackson.annotation.JsonCreator;
import java.time.OffsetDateTime;
import java.util.ArrayList;
import java.util.List;
import org.springframework.format.annotation.DateTimeFormat;
import org.openapitools.jackson.nullable.JsonNullable;
import java.time.OffsetDateTime;
import jakarta.validation.Valid;
import jakarta.validation.constraints.*;
import io.swagger.v3.oas.annotations.media.Schema;


import java.util.*;
import jakarta.annotation.Generated;

/**
 * ErrorResponse
 */

@Generated(value = "org.openapitools.codegen.languages.SpringCodegen", date = "2025-09-10T19:33:54.389991100-06:00[America/Mexico_City]")
public class ErrorResponse {

  @DateTimeFormat(iso = DateTimeFormat.ISO.DATE_TIME)
  private OffsetDateTime timestamp;

  private String message;

  @Valid
  private List<String> details;

  public ErrorResponse timestamp(OffsetDateTime timestamp) {
    this.timestamp = timestamp;
    return this;
  }

  /**
   * Get timestamp
   * @return timestamp
  */
  @Valid 
  @Schema(name = "timestamp", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("timestamp")
  public OffsetDateTime getTimestamp() {
    return timestamp;
  }

  public void setTimestamp(OffsetDateTime timestamp) {
    this.timestamp = timestamp;
  }

  public ErrorResponse message(String message) {
    this.message = message;
    return this;
  }

  /**
   * Get message
   * @return message
  */
  
  @Schema(name = "message", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("message")
  public String getMessage() {
    return message;
  }

  public void setMessage(String message) {
    this.message = message;
  }

  public ErrorResponse details(List<String> details) {
    this.details = details;
    return this;
  }

  public ErrorResponse addDetailsItem(String detailsItem) {
    if (this.details == null) {
      this.details = new ArrayList<>();
    }
    this.details.add(detailsItem);
    return this;
  }

  /**
   * Get details
   * @return details
  */
  
  @Schema(name = "details", requiredMode = Schema.RequiredMode.NOT_REQUIRED)
  @JsonProperty("details")
  public List<String> getDetails() {
    return details;
  }

  public void setDetails(List<String> details) {
    this.details = details;
  }

  @Override
  public boolean equals(Object o) {
    if (this == o) {
      return true;
    }
    if (o == null || getClass() != o.getClass()) {
      return false;
    }
    ErrorResponse errorResponse = (ErrorResponse) o;
    return Objects.equals(this.timestamp, errorResponse.timestamp) &&
        Objects.equals(this.message, errorResponse.message) &&
        Objects.equals(this.details, errorResponse.details);
  }

  @Override
  public int hashCode() {
    return Objects.hash(timestamp, message, details);
  }

  @Override
  public String toString() {
    StringBuilder sb = new StringBuilder();
    sb.append("class ErrorResponse {\n");
    sb.append("    timestamp: ").append(toIndentedString(timestamp)).append("\n");
    sb.append("    message: ").append(toIndentedString(message)).append("\n");
    sb.append("    details: ").append(toIndentedString(details)).append("\n");
    sb.append("}");
    return sb.toString();
  }

  /**
   * Convert the given object to string with each line indented by 4 spaces
   * (except the first line).
   */
  private String toIndentedString(Object o) {
    if (o == null) {
      return "null";
    }
    return o.toString().replace("\n", "\n    ");
  }
}

este es mi controller



