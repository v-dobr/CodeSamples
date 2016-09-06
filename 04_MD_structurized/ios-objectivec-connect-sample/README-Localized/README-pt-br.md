# Exemplo de conexão com o Office 365 para iOS usando o SDK do Microsoft Graph

O Microsoft Graph é um ponto de extremidade unificado para acessar dados, relações e ideias que vêm do Microsoft Cloud. Este exemplo mostra como realizar a conexão e a autenticação no Microsoft Graph e, em seguida, chamar APIs de mala direta e usuário por meio do [SDK do Microsoft Graph para iOS](https://github.com/microsoftgraph/msgraph-sdk-ios).

> Observação: Experimente a página [Portal de Registro do Aplicativo Microsoft Graph](https://graph.microsoft.io/en-us/app-registration) que simplifica o registro para que você possa executar este exemplo com mais rapidez.

## Pré-requisitos
* [Xcode](https://developer.apple.com/xcode/downloads/) da Apple
* Instalação do [CocoaPods](https://guides.cocoapods.org/using/using-cocoapods.html) como um gerente de dependências.
* Uma conta de email comercial ou pessoal da Microsoft como o Office 365, ou outlook.com, hotmail.com, etc. Inscreva-se em uma [Assinatura do Office 365 para Desenvolvedor](https://aka.ms/devprogramsignup) que inclua os recursos necessários para começar a criar aplicativos do Office 365.

     > Observação: Caso já tenha uma assinatura, o link anterior direciona você para uma página com a mensagem *Não é possível adicioná-la à sua conta atual*. Nesse caso, use uma conta de sua assinatura atual do Office 365.    
* Uma ID de cliente do aplicativo registrado no [Portal de Registro do Aplicativo do Microsoft Graph](https://graph.microsoft.io/en-us/app-registration)
* Para realizar solicitações de autenticação, um **MSAuthenticationProvider** deve ser fornecido para autenticar solicitações HTTPS com um token de portador OAuth 2.0 apropriado. Usaremos [msgraph-sdk-ios-nxoauth2-adapter](https://github.com/microsoftgraph/msgraph-sdk-ios-nxoauth2-adapter) para uma implementação de exemplo de MSAuthenticationProvider que pode ser usada para acelerar seu projeto. Confira a seção **Código de Interesse** abaixo para obter mais informações.


## Executar este exemplo em Xcode

1. Clonar este repositório
2. Use o CocoaPods para importar as dependências de autenticação e o SDK do Microsoft Graph:

        pod 'MSGraphSDK'
        pod 'MSGraphSDK-NXOAuth2Adapter'


 Este aplicativo de exemplo já contém um podfile que colocará os pods no projeto. Simplesmente navegue até o projeto do **Terminal** e execute:

        pod install

   Para saber mais, confira o artigo **Usar o CocoaPods** em [Recursos Adicionais](#recursos-adicionais)

3. Abrir **O365-iOS-Microsoft-Graph-SDK.xcworkspace**
4. Abra **AuthenticationConstants.m**. Observe que você pode adicionar o valor de **ID de Cliente** do processo de registro na parte superior do arquivo.

        // You will set your application's clientId
        NSString * const kClientId    = @"ENTER_YOUR_CLIENT_ID";

    > Observação: Você notará que foram configurados os seguintes escopos de permissão para esse projeto: **"https://graph.microsoft.com/Mail.Send", "https://graph.microsoft.com/User.Read", "offline_access"**. As chamadas de serviço usadas neste projeto, que enviam emails para sua conta de email e recuperam algumas informações de perfil (Nome de Exibição, Endereço de Email) exigem essas permissões para que o aplicativo seja executado corretamente.

5. Execute o exemplo. Você será solicitado a conectar/autenticar uma conta de email comercial ou pessoal e, em seguida, poderá enviar um email a essa conta ou a outra conta de email selecionada.


##Código de Interesse

Todo código de autenticação pode ser visualizado no arquivo **AuthenticationProvider.m**. Usamos um exemplo de implementação do MSAuthenticationProvider estendida do [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) para oferecer suporte a aplicativos nativos registrados, atualização automática de tokens de acesso e recurso de logout:

        [[NXOAuth2AuthenticationProvider sharedAuthProvider] loginWithViewController:nil completion:^(NSError *error) {
            if (!error) {
            [MSGraphClient setAuthenticationProvider:[NXOAuth2AuthenticationProvider sharedAuthProvider]];
            self.client = [MSGraphClient client];
             }
        }];


Depois que o provedor de autenticação estiver definido, podemos criar e inicializar um objeto de cliente (MSGraphClient) que será usado para fazer chamadas no ponto de extremidade do serviço do Microsoft Graph (email e usuários). Em **SendMailViewcontroller.m**, podemos montar uma solicitação de email e enviá-la usando o seguinte código:

    MSGraphUserSendMailRequestBuilder *requestBuilder = [[self.client me]sendMailWithMessage:message saveToSentItems:true];    
    MSGraphUserSendMailRequest *mailRequest = [requestBuilder request];   
    [mailRequest executeWithCompletion:^(NSDictionary *response, NSError *error) {      
    }];


Para obter mais informações, incluindo código para chamar outros serviços, como o OneDrive, confira o [SDK do Microsoft Graph para iOS](https://github.com/microsoftgraph/msgraph-sdk-ios)

## Perguntas e comentários

Gostaríamos de saber sua opinião sobre o projeto de conexão com o Office 365 para iOS usando o Microsoft Graph. Você pode nos enviar suas perguntas e sugestões por meio da seção [Issues](https://github.com/microsoftgraph/iOS-objectivec-connect-sample/issues) deste repositório.

As perguntas sobre o desenvolvimento do Office 365 em geral devem ser postadas no [Stack Overflow](http://stackoverflow.com/questions/tagged/Office365+API). Não deixe de marcar as perguntas ou comentários com [Office365] e [MicrosoftGraph].

## Colaboração
Será preciso assinar um [Contributor License Agreement (Contrato de Licença de Colaborador)](https://cla.microsoft.com/) antes de enviar a solicitação pull. Para concluir o CLA (Contributor License Agreement), você deve enviar uma solicitação através do formulário e assinar eletronicamente o CLA quando receber o email com o link para o documento.

Este projeto adotou o [Código de Conduta do Código Aberto da Microsoft](https://opensource.microsoft.com/codeofconduct/). Para saber mais, confira as [Perguntas frequentes do Código de Conduta](https://opensource.microsoft.com/codeofconduct/faq/) ou contate [opencode@microsoft.com](mailto:opencode@microsoft.com) se tiver outras dúvidas ou comentários.

## Recursos adicionais

* [Centro de Desenvolvimento do Office](http://dev.office.com/)
* [Página de visão geral do Microsoft Graph](https://graph.microsoft.io)
* [Usando o CocoaPods](https://guides.cocoapods.org/using/using-cocoapods.html)

## Direitos autorais
Copyright © 2016 Microsoft. Todos os direitos reservados.