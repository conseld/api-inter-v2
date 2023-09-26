# INCLUIR BOLETO COBRANÇA-INTER

## Incluir Boleto de Cobrança pix

Método utilizado para incluir uma cobrança com código de barras e QRCode.<br>
A inclusão é assíncrona, então você receberá um Código identificador único da cobranca, que deverá ser utilizado para atualizar a cobrança em seu sistema de forma ativa, através de consulta, ou de forma passiva, via Webhook.

## Escopo

Escopo requerido: boleto-cobranca.write<br>
Rate limit: 120 chamadas por minuto

```php
    require '../../../vendor/autoload.php';

    use Divulgueregional\ApiInterV2\InterBanking;

    $config = [
        'certificate' => '../cert/Inter_API_Certificado.crt',//local do certificado crt
        'certificateKey' => '../cert/Inter_API_Chave.key',//local do certificado key
    ];

    $dadosBoleto = [
        "seuNumero" => "85401",//obrigatório
        "valorNominal" => 50,//obrigatório
        "valorAbatimento" => 0,
        "dataEmissao" => "2022-04-28",
        "dataVencimento" => "2022-07-15",//obrigatório
        "numDiasAgenda" => 30,//obrigatório
        "cnpjCPFBeneficiario" => '99999999999999',//CNPJ de quem está emitindo o boleto
        "pagador" => [
            "cpfCnpj" => "99999999999",//obrigatório
            "nome" => "Nome do Pagador",//obrigatório
            "email" => "",
            "telefone" => "",
            "cep" => "95097000",//obrigatório
            "numero" => "000",
            "complemento" => "",
            "bairro" => "Rio Branco",
            "cidade" => "Caxias do Sul",//obrigatório
            "uf" => "RS",//obrigatório
            "endereco" => "Endereço do pagador",//obrigatório
            "ddd" => "",
            "tipoPessoa" => "FISICA"//obrigatório
        ],
        "mensagem" => [
            "linha1" => "Teste de evio 1",
            "linha2" => "Teste de evio 2",
            "linha3" => "",
            "linha4" => "",
            "linha5" => ""
        ],
        "desconto1" => [
            "codigoDesconto" => "NAOTEMDESCONTO",
            "taxa" => 0,
            "valor" => 0,
            "data" => ""
        ],
        "desconto2" => [
            "codigoDesconto" => "NAOTEMDESCONTO",
            "taxa" => 0,
            "valor" => 0,
            "data" => ""
        ],
        "desconto3" => [
            "codigoDesconto" => "NAOTEMDESCONTO",
            "taxa" => 0,
            "valor" => 0,
            "data" => ""
        ],
        "multa" => [
            "codigoMulta" => "NAOTEMMULTA",
            "data" => "",
            "taxa" => 0,
            "valor" => 0
        ],
        "mora" => [
            "codigoMora" => "ISENTO",
            "data" => "",
            "taxa" => 0,
            "valor" => 0
        ]
    ];

    $token = '';//seu token
    try {
        $bankingInter = new InterBanking($config);
        $bankingInter->setToken($token);

        echo "<pre>";
        $response = $bankingInter->incluirBoletoCobrancaPix($dadosBoleto);
        // echo $incluirBoletoCobranca->nossoNumero;
        print_r($response);
    } catch (\Exception $e) {
        echo $e->getMessage();
    }
```

## ATENÇÃO

Todos os parâmetros como os dados do pagador, devem ser validados por você para não der problema na geração do boleto. A cobrança é cadastrada no banco central e aparecerá no DDA dos sacados. Os dadosBoleto você deverá informar com os seus dados, revise cada parâmetro e coloque a informação corretamente.
