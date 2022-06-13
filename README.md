# Troubleshoot : Multicarteira FIDEM - HTML de contrato enviado divergente do cadastrado
**N° Ticket analisado:** 10325

## **Descrição:** 
O processo de emissão de contratos HTML da fitbank, não estava funcionando após a configuração 
do whitelabel para a estrutura de contrato personalizado da Multicarteira FIDEM, que funcionou por um 
período curto e após não continuou a funcionar

Note, que inicialmente, a emissão ocorria novamente, como descrito na imagem:

![image](https://user-images.githubusercontent.com/63611415/173401110-e8f7f32e-5b8c-4fda-822b-aa30311d7317.png)

Porém, o mesmo processo parou de funcionar depois de um tempo, onde não foi mais possível
visualizar o contrato que havia sido criado/assinado anteriormente, como mostrado abaixo:

![image](https://user-images.githubusercontent.com/63611415/173401412-d1094b2f-1c32-45b5-b10e-e424afff5bdc.png)

## **Causa:** 
O processo de emissão dos contratos da fitbank estavam sendo prejudicados devido a uma
antiga estrutura de micro serviços desatualizados que estava presente na API, impedindo o processo
de geração e configuração dos contratos com HTML personalizados.

## **Solução:** 
A estrutura de micro serviços que estava ocasionando o erro foi desativada, e substituída
por um novo processo de geração de contratos com HTML configurado, permitindo a emissão
correta dos mesmos pela API, o cliente que relatou o problema foi notificado sobre a correção.
