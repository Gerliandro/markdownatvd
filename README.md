# **Troubleshoot:** Multicarteira FIDEM - HTML de contrato enviado divergente do cadastrado
# **N° Ticket Azure:** 10325

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

# **Troubleshoot:** Multicarteira Paypi – Taxa de mensalidade que a conta virtual paga pra BU está indo para a conta primaria
# **N° Ticket Azure:** 10702

## **Descrição:**
Ocorreu um problema referente ao pagamento de taxa de mensalidade definida na Business Unit
da Paypi, onde o valor liquidado pelas contas virtuais para a Business Unit, estavam sendo direcionadas
diretamente para a conta primaria referente a BU e não a conta virtual vinculada a mesma.

Podemos observe este problema na seguinte imagem, onde as taxas configuradas no Business Unit da Paypi
estão sendo debitadas diretamente na conta primaria:

![image](https://user-images.githubusercontent.com/63611415/173402241-fca4f585-6e30-4734-94ad-257ab1e93a8f.png)

## **Causa:**
O problema ocorria por que o CNPJ vinculado a BU era o mesmo do Titular, desse modo o sistema estava realizando a operação de credito na conta do titular, onde o credito era atribuido somente a conta primaria

## **Solução:**
Foi informado ao cliente sobre a causa do problema , e o mesmo pode corrigir o ocorrido , ficando ciente que o sistema automaticamente credita na conta do titular quando o CNPJ da BU e do titular são os mesmos.

# **Troubleshoot:** GetBoletoByDate dando exceção mas os dados estão sendo passado corretamente
# **N° Ticket Azure:** 10824

## **Descrição:**
Neste caso, o método GetBoletoByDate lançou uma exceção de falta de parâmetros mesmo ao
utilizar os campos do método corretamente.

Podemos notar a visualização do erro ao analisar os arquivos Json de entrada e saída do método

![image](https://user-images.githubusercontent.com/63611415/173403885-9af7ae7e-ac04-4697-9ce0-c9b87937521c.png)

## **Causa:**
O método em questão começou a apresentar este problema após uma atualização de User , que ao
chamar o método GetBoletoByDate permitia a passagem de parâmetros com nomenclatura incorreta.

## **Solução:**
A solução proposta foi remover a validação dos parâmetros obrigatórios do Receiver , de modo que
mesmo que a grafia dos parâmetros não fosse mais previamente validada, o método voltaria a funcionar
normalmente, apresentando os boletos dentro da data especificada

# **Troubleshoot:** - Cálculo de juros e multa divergente
# **N° Ticket Azure:** 13402

## **Descrição:**
Foi reportado pelo cliente um erro na geração da taxa de juros de boletos que possuem a
possibilidade de serem liquidados no sábado, onde o juro calculado foi divergente do que se esperava, onde o
cálculo não incluiu o próximo dia ultimo após o sábado.

Desse modo, podemos observar na imagem abaixo a taxa gerada e os dias respectivos a geração da mesma: 

![image](https://user-images.githubusercontent.com/63611415/173404178-eeff2234-7609-4dc5-8c19-febd71f47e56.png)

## **Causa:**
O problema ocorreu por que quando uma consulta de boleto é realizada no sábado e o mesmo é
inserido na fila de pagamentos nesse mesmo dia, quando o boleto é enviado para o processamento de
pagamentos, o sistema retornava “valor divergente” já que o cálculo da taxa era feito somente até o último dia
útil, e no sábado o ExtraValue recebia os valores calculados somente até esse dia em questão , desse modo
quando o processamento de pagamento era efetuado no próximo dia útil , o valor extrapolava o esperado e
por isso era retornado “valor divergente”.

## **Solução:** 
Barrar a inclusão de pagamentos vencidos após o horário de corte, de modo a garantir que não
haverá divergências de valores novamente.

# **Troubleshoot:** - - Banco de dados trazendo informação errada
# **N° Ticket Azure:** 16710

## **Descrição:** 
Na tentativa de envio do arquivo de transferência bancaria, o sistema rejeita o mesmo devido o
Banco de Dados estar passando as informações erradas de transferência para o arquivo, mudando o tipo do
mesmo de TED para TEV. Onde podemos notar o ocorrido na imagem abaixo

![image](https://user-images.githubusercontent.com/63611415/173404429-7c75fb04-a3ab-4a39-ba75-be51908ad560.png)

## **Causa:** 
As informações que estavam registradas no Banco de Dados responsável por preencher as
informações bancarias do arquivo, estavam gravadas com um valor diferente do convencional para a
realização da transferência desejada entre os bancos Votorantim e Nubank, onde o valor gravado
permitia a transferência para outras demais instituições. Podemos observar a causa a partir das
imagens

Enumeração com os tipos de conta destino (Onde o valor determinará o processamento correto
para a transferência entre a conta origem e a conta destino)

![image](https://user-images.githubusercontent.com/63611415/173404601-7a9d0c4e-40f4-4fda-91db-bbb6baaa093c.png)

Dados gravados no banco de dados: 

![image](https://user-images.githubusercontent.com/63611415/173404670-2a969e7c-5915-4c75-adb0-e9845abfe106.png)

## **Solução:** 
Validar se existe algum tratamento prévio nos dados do banco de dados ao
requisitar/consultar tais dados, afim de garantir se os dados estão sendo tratados devidamente e
caso não estejam, trata-los devidamente para que em seguida possam ser lidos da maneira correta
pelo sistema
