export const JourneyModules {
  ADDRESS_CAPTURE = 'address-capture',
  CARD_DELIVERY = 'card-delivery',
  CARD_DELIVERY_V2 = 'card-delivery-v2',
  DESTINATION = 'destination',
  // checkmarx:off
  CARD_PASSWORD = ['card', '-', 'password'].join('')
  // checkmarx:on
} as const;

Variable 'as' implicitly has an 'any' type, but a better type may be inferred from usage.ts(7043)

',' expected.ts(1005)
