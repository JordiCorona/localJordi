Me puedes ayudar por favor.
Tengo un error en el proceso de envio de notificaciones para la creacion de la tarjeta digital.
revisando el error es por que un dato de mi tarjeta viene null

el codigo fuente es este, puedes revisar que realiza
 //Creación servicio pendiente
      const basicPersonalRequestLibDto: BasicPersonalRequestLibDto = {
        user: userKey,
        client_id: cif,
        account_id: card.account_id,
        x_auth_token: extraHeaders['x-auth-token']
      }
      const containtDigitalCard=cards.accounts.find(element=> element.product_id.includes(PersonalDataEnum.DIGITAL_D));
      let responseDigitalCard;
      let digitalCard;
      if(containtDigitalCard === undefined){
        //Se crea tarjeta de Crédito digital
        
        this.logger.log("url create journey config Server>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "+JSON.stringify(this.configService.get("DIGITAL-CARD.URL_CREATE")));
        this.logger.log("url create journey process >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "+JSON.stringify(process.env.URL_CREATE));
        responseDigitalCard=  await this.createBcsService.createDigitalCard(basicPersonalRequestLibDto);
        this.logger.log("responseDigitalCard >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>   "+JSON.stringify(responseDigitalCard));
        if(responseDigitalCard.notifications!=null){

          if(responseDigitalCard.notifications[0].code==="I_CEN_BS_001"){
            digitalCard=responseDigitalCard.data as DigitalCardDto;
            this.loggerESLM.log(
            `Successful card digital activating for requestId: ${requestId}`, 
            BUSINESS_TAG,
            );
          }else{
            this.loggerESLM.log(
            `Error activating digital card for requestId: ${requestId}`, 
            BUSINESS_TAG_FAILED,
            );
            throw new DigitalCardException(`Error activating digital card`, CodeError.TDG_001);
          }
        }
      }else{
        //La tarjeta digital ya esta creada.
        if(containtDigitalCard.account_id_masked===undefined){
          digitalCard={
            card_number: "",
            card_number_key: containtDigitalCard.account_id
          } as DigitalCardDto
        }else{
          digitalCard={
            card_number: containtDigitalCard.account_id_masked,
            card_number_key: containtDigitalCard.account_id
          } as DigitalCardDto
        }
        
      }

      // Se guarda en personsalData
      await this.savePersonalData(productSummaryCat,digitalCard,requestId);

      //Envio de notificaciones
      await this.registerEventTracing(requestId);
      this.notificationsServiceDelegate.sendNotifications(requestId, personalDataResponse);

      let responseDelivery = await this.delivery(requestId);

      this.loggerESLM.log(
      `Digital card created successfully for requestId: ${requestId}`, 
      BUSINESS_TAG,
      );

      return { next_step: responseDelivery };
    }catch (error) {
      this.loggerESLM.log(
      `Error creating digital card for requestId: ${requestId}`, 
      BUSINESS_TAG_FAILED,
      );
      throw new DigitalCardException(`Error processing digital card ${error}`, CodeError.TDG_001);
    }
  }




      }else{
        //La tarjeta digital ya esta creada.
        if(containtDigitalCard.account_id_masked===undefined){
          digitalCard={
            card_number: "",
            card_number_key: containtDigitalCard.account_id
          } as DigitalCardDto
        }else{
          digitalCard={
            card_number: containtDigitalCard.account_id_masked,
            card_number_key: containtDigitalCard.account_id
          } as DigitalCardDto
        }
        
      }

      // KEYMASTER Desubrogar tarjeta para enmascarar -> bcs ya no regresa tarjeta en mascarada
      const basicPersonalRequestLibDtoForKeyMaster: BasicPersonalRequestLibDto = {
      user: basicPersonal.transaction_data[BasicPersonalEnum.USER_KEY.toString()],
      client_id: basicPersonal.transaction_data[BasicPersonalEnum.CLIENT_ID.toString()],
      account_id: digitalCard['card_number_key'],
      x_auth_token:  extraHeaders['x-auth-token']
    }
      let cardClean= await this.keyMasterBcsService.getDesSurrogate(basicPersonalRequestLibDtoForKeyMaster);
      digitalCard['card_number_key']=cardClean;
      
      // Se guarda en personsalData
      await this.savePersonalData(productSummaryCat,digitalCard,requestId);
