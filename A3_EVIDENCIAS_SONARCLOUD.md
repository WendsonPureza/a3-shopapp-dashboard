# Evidencias do SonarCloud - A3

## Identificacao

- Sistema: Shop App
- Repositorio: https://github.com/erinkelsey/shopapp-nodejs
- Licenca declarada no package.json: ISC
- Commit analisado: `52b3cfd8816297580af7faf47e6a7e6df8876f16`
- Data da analise: 24/11/2026
- Ferramenta: SonarCloud, scanner npm `@sonar/scan` 5.1.0
- Organizacao: `wendsonpureza`
- Projeto: `shopapp-nodejs-a3`
- Dashboard: https://sonarcloud.io/dashboard?id=shopapp-nodejs-a3

## Indicadores

| Indicador | Resultado |
|---|---:|
| Linhas de codigo | 1274 |
| Bugs | 2 |
| Vulnerabilidades | 8 |
| Code smells | 1 |
| Security hotspots | 0 |
| Duplicacao | 2.0% |
| Complexidade | 145 |
| Cobertura | 0.0% |
| Rating de confiabilidade | 4.0 |
| Rating de seguranca | 5.0 |
| Rating de manutencao | 1.0 |

Nao ha dados de cobertura porque o projeto nao possui script de teste configurado e nenhuma execucao com relatorio foi enviada ao SonarCloud.

## Achados principais

| Linha | Categoria | Regra | Evidencia |
|---|---|---|---|
| app.js:82 | Vulnerabilidade | javascript:S5693 | Corpo enviado pelo middleware multer pode nao ter limite de tamanho definido |
| app.js:19 | Vulnerabilidade | javascript:S5689 | Versao da framework Express pode ser divulgada implicitamente |
| admin.js:164 | Vulnerabilidade | jssecurity:S5147 | Entrada potencial em consulta de produto |
| admin.js:196 | Vulnerabilidade | jssecurity:S5147 | Entrada potencial em consulta de exclusao |
| auth.js:44 | Vulnerabilidade | jssecurity:S5147 | Entrada potencial em consulta de usuario durante login |
| auth.js:235 | Vulnerabilidade | jssecurity:S5147 | Entrada potencial em consulta de redefinicao de senha |
| shop.js:261 | Vulnerabilidade | jssecurity:S5147 | Entrada potencial em consulta de pedidos |
| controllers/auth.js:10 | Vulnerabilidade | javascript:S5332 | Uso de HTTP em configuracao relacionada a transporte |
| routes/auth.js:73 | Code smell | javascript:S6671 | Motivo de rejeicao enviado como texto em vez de Error |
| public/css/cart.css:3 | Bug | css:S4656 | Propriedade margin duplicada |
| public/css/cart.css:23 | Bug | css:S4657 | margin-right sobrescrita por margin shorthand |

## Interpretacao

Os cinco achados de injecao NoSQL apontados pelo SonarCloud merecem validacao manual. Eles aparecem em consultas Mongoose que recebem dados de requisicao ou identificadores. Os casos de teste devem incluir objetos malformados e tipos inesperados. Nao foi realizado teste da aplicacao, apenas a analise estatica.

O achado de tamanho de body merece configuracao de limite e teste com payload acima do limite. O achado de HTTP depende da configuracao real de HOST_URL e do ambiente de producao; nao deve ser concluido como vulnerabilidade ativa sem essa confirmacao.

Os bugs CSS afetam o layout do carrinho e podem causar visualizacao incorreta, especialmente em largura responsiva.

## Rastreabilidade

| Achado | Risco | Caso relacionado |
|---|---|---|
| S5147 em auth.js | Obtencao indevida de sessao ou redefinicao por entrada malformada | CT-AUTH-02, CT-AUTH-03 |
| S5147 em admin.js | Alteracao ou exclusao de recurso fora do proprietario | CT-ADM-03, CT-ADM-04 |
| S5147 em shop.js | Consulta de pedidos de outro usuario | CT-PED-02 |
| S5693 em app.js | Upload ou corpo excessivo causa indisponibilidade | CT-UPL-02 |
| S5332 em controllers/auth.js | Transmissao insegura em ambiente inadequado | CT-AUTH-04 |
| S4656/S4657 em cart.css | Layout inconsistente no carrinho | CT-CARR-03 |
