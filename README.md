// La tarjeta digital ya está creada
if (containtDigitalCard.account_id_masked === undefined) {
  // No viene enmascarado -> lo desenmascaramos con KeyMaster
  const basicPersonalRequestLibDtoForKeyMaster: BasicPersonalRequestLibDto = {
    user: basicPersonal.transaction_data[BasicPersonalEnum.USER_KEY.toString()],
    client_id: basicPersonal.transaction_data[BasicPersonalEnum.CLIENT_ID.toString()],
    account_id: containtDigitalCard.account_id,
    x_auth_token: extraHeaders['x-auth-token']
  };

  const cardClean = await this.keyMasterBcsService.getDesSurrogate(basicPersonalRequestLibDtoForKeyMaster);

  digitalCard = {
    card_number: cardClean,
    card_number_key: cardClean
  } as DigitalCardDto;
} else {
  digitalCard = {
    card_number: containtDigitalCard.account_id_masked,
    card_number_key: containtDigitalCard.account_id
  } as DigitalCardDto;
}

// Validación defensiva antes de continuar
if (!digitalCard.card_number || digitalCard.card_number.trim() === "") {
  this.logger.error(`digitalCard.card_number is null or empty for requestId: ${requestId}`);
  throw new DigitalCardException("digital-card-number is missing or empty", CodeError.TDG_002);
}

// Guardar en personalData
await this.savePersonalData(productSummaryCat, digitalCard, requestId);
