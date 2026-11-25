# Plano e casos de teste - Shop App

## Plano resumido

- Objetivo: avaliar autenticacao, cadastro e edicao de produtos, carrinho, pedidos e robustez de entradas.
- Estrategia: testes funcionais de caixa-preta, validacao de API, interfaces e regressao.
- Tecnicas: particionamento de equivalencia, valor limite e caso de uso.
- Ambiente planeado: Node.js 22, MongoDB, navegador moderno e credenciais externas somente de teste.
- Observacao: os casos nao foram executados neste documento.

## Casos

| ID | Funcionalidade | Objetivo | Dados | Passos resumidos | Resultado esperado | Aprovação/reprovação | Técnica | Prioridade |
|---|---|---|---|---|---|---|---|---|
| CT-AUTH-01 | Cadastro | Aceitar dados validos | email novo, senha alfanumerica com 5 caracteres ou mais | abrir cadastro; preencher; confirmar | conta criada e redirecionamento para login | aprovado se nao existir conta duplicada | Equivalencia positiva | Alta |
| CT-AUTH-02 | Login | Rejeitar credenciais invalidas | email valido, senha incorreta | abrir login; enviar | acesso nao concedido e sessao nao criada | aprovado se usuario continuar anonimo | Equivalencia negativa | Alta |
| CT-AUTH-03 | Redefinicao | Rejeitar token expirado ou malformado | token expirado e objeto malformado | abrir URL de redefinicao; enviar nova senha | redefinicao bloqueada sem alteracao | aprovado se usuario nao trocar senha | Valor limite/entrada negativa | Alta |
| CT-AUTH-04 | Login/reset | Verificar apenas HTTPS em ambiente publico | URL de ambiente e configuracao HOST_URL | configurar HTTPS e fazer login/reset | dados trafegados por canal seguro | aprovado se ambiente publico redirecionar ou bloquear HTTP | Caixa-preta/seguranca | Alta |
| CT-ADM-01 | Produto | Cadastrar produto valido | titulo com 3 caracteres, descricao com 5-400 caracteres, preco numerico | enviar formulario com imagem valida | produto salvo e listado | aprovado se todas as validacoes passarem | Valor limite positivo | Alta |
| CT-ADM-02 | Produto | Rejeitar limites invalidos | titulo com 2 caracteres; descricao com 4 ou 401; preco nao numerico | enviar cada variante | produto nao salvo e mensagem objetiva | aprovado se nenhum registro for criado | Valor limite/negativo | Alta |
| CT-ADM-03 | Edicao | Impedir usuario editar produto de outro usuario | produto cujo userId pertence a outro usuario | enviar edicao com ID valido | edicao negada e dados originais preservados | aprovado se nao houver alteracao | Caso de uso/seguranca | Alta |
| CT-ADM-04 | Exclusao | Impedir injecao em ID de produto | ID como objeto, array ou string malformada | enviar requisicao de exclusao | requisicao rejeitada e produto preservado | aprovado se nenhuma conta ou produto afetado | Entrada malformada | Alta |
| CT-CARR-01 | Carrinho | Adicionar produto uma vez | usuario autenticado e produto existente | adicionar o mesmo produto duas vezes | quantidade do item aumenta para 2 | aprovado se quantidade e total estiverem corretos | Regras de negocio | Alta |
| CT-CARR-02 | Carrinho | Remover item | item presente no carrinho | remover item | carrinho atualizado e total recalculado | aprovado se item e total forem removidos | Caso de uso | Alta |
| CT-CARR-03 | Carrinho | Verificar layout responsivo | viewport pequeno e lista com varios itens | abrir carrinho em 320px e 1440px | itens sem sobreposicao e margens consistentes | aprovado se CSS nao cortar nem sobrescrever conteudo | Exploratorio/UI | Media |
| CT-PED-01 | Pedido | Criar pedido somente apos checkout valido | carrinho preenchido e pagamento aprovado | concluir checkout de teste | pedido criado uma unica vez e carrinho limpo | aprovado se houver um pedido correspondente | Integracao | Alta |
| CT-PED-02 | Pedido | Consultar somente pedidos proprios | usuario A e pedido do usuario B | solicitar URL da fatura com ID do pedido B | acesso negado ao pedido alheio | aprovado se nao retornar dados nem PDF | Seguranca/caixa-preta | Alta |
| CT-UPL-01 | Upload | Aceitar formatos permitidos | arquivos PNG, JPG e JPEG | enviar em novo produto | imagem aceita | aprovado se arquivo for armazenado | Equivalencia positiva | Alta |
| CT-UPL-02 | Upload | Rejeitar formato e tamanho invalidos | arquivo GIF e arquivo acima do limite definido | enviar em novo produto | arquivo rejeitado e produto nao criado | aprovado se nao houver armazenamento indevido | Valor limite/negativo | Alta |
