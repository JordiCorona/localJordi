// La tarjeta digital ya está creada
if (containtDigitalCard.account_id_masked === undefined) {
  // No viene enmascarada -> obtener el número limpio desde KeyMaster
  const basicPersonalRequestLibDtoForKeyMaster: BasicPersonalRequestLibDto = {
    user: basicPersonal.transaction_data[BasicPersonalEnum.USER_KEY.toString()],
    client_id: basicPersonal.transaction_data[BasicPersonalEnum.CLIENT_ID.toString()],
    account_id: containtDigitalCard.account_id,
    x_auth_token: extraHeaders['x-auth-token']
  };

  const cardClean = await this.keyMasterBcsService.getDesSurrogate(basicPersonalRequestLibDtoForKeyMaster);

  digitalCard = {
    card_number: maskCardNumber(cardClean),             // ✅ Enmascarado manual
    card_number_key: cardClean
  } as DigitalCardDto;
} else {
  digitalCard = {
    card_number: containtDigitalCard.account_id_masked,
    card_number_key: containtDigitalCard.account_id
  } as DigitalCardDto;
}

// Se guarda en personalData
await this.savePersonalData(productSummaryCat, digitalCard, requestId);



function maskCardNumber(cardNumber: string): string {
  if (!cardNumber || cardNumber.length < 8) {
    return '************'; // Fallback si es muy corto
  }

  // Ejemplo: 1234567890123456 -> 1234********3456
  return `${cardNumber.slice(0, 4)}********${cardNumber.slice(-4)}`;
}
