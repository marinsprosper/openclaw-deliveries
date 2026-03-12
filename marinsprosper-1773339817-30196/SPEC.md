Abaixo está um panorama completo, em formato de especificação funcional e técnica, para entregar a um programador e orientar a construção da plataforma Marinsprosper.
Especificação da plataforma Marinsprosper
1. Visão geral do produto
A Marinsprosper é uma plataforma de intermediação P2P inspirada no modelo operacional da Airtm, mas com adaptações próprias. O objetivo é conectar clientes e caixeiros para operações de compra e venda de USDT, com foco em segurança operacional, rastreabilidade, mediação de conflitos e reputação.
A proposta central da plataforma é:
o cliente sempre transfere primeiro quando estiver protegido pelas regras da plataforma;
o caixeiro mantém uma caução/garantia para poder aceitar ordens;
a plataforma não funciona como banco tradicional;
a plataforma precisa ter registro completo da operação, chat por ordem, sistema de reputação, cancelamento com aceite da contraparte e mediação administrativa.
O nome da marca é Marinsprosper.
O posicionamento deve passar imagem de fintech/banco digital: simples, segura, limpa e profissional.
Slogan sugerido: “Câmbio P2P, simples e seguro.”
2. Objetivo do MVP
O MVP deve permitir testar o fluxo completo de ponta a ponta, com:
cadastro e login;
criação de ordens de compra e venda;
aceite por caixeiro;
controle de limite por caução;
troca de mensagens e envio de comprovantes/TXID;
conclusão da ordem;
solicitação de cancelamento;
aceite ou recusa do cancelamento;
abertura de disputa;
resolução pelo admin;
avaliação entre as partes;
painel administrativo completo para gestão operacional.
O MVP precisa parecer um aplicativo, mesmo sendo web.
Cada área deve abrir como uma página própria, com navegação fluida, como se fosse app.
3. Perfis de usuário
A plataforma deve ter três perfis principais.
3.1 Cliente
É quem cria a solicitação de compra ou venda de USDT.
Permissões do cliente:
criar ordem;
visualizar suas ordens;
abrir o detalhe de cada ordem;
conversar no chat da ordem;
enviar comprovante/TXID;
solicitar cancelamento;
aceitar ou recusar cancelamento pedido pela outra parte;
abrir disputa;
avaliar o caixeiro ao final;
visualizar seu histórico.
3.2 Caixeiro
É quem aceita a ordem e executa a contraparte da transação.
Permissões do caixeiro:
visualizar ordens disponíveis;
aceitar ordem;
visualizar suas ordens aceitas;
conversar no chat da ordem;
confirmar recebimento;
concluir ordem;
solicitar cancelamento;
aceitar ou recusar cancelamento;
abrir disputa;
avaliar o cliente;
visualizar sua reputação;
visualizar seu saldo de caução e limite operacional.
3.3 Administrador
É quem controla a plataforma.
Permissões do admin:
aprovar ou reprovar clientes;
aprovar ou reprovar caixeiros;
editar percentuais de taxas;
controlar blacklists;
visualizar todas as ordens;
visualizar todos os chats;
visualizar comprovantes;
mediar disputas;
resolver disputas;
ajustar caução/limites;
bloquear usuários;
ver relatórios operacionais;
ver logs de auditoria.
4. Modelo operacional da plataforma
4.1 Lógica principal
A plataforma opera no modelo P2P com proteção baseada em regras e garantia do caixeiro.
Regras principais:
O cliente cria a ordem.
O caixeiro aceita a ordem.
A plataforma verifica se o caixeiro tem caução suficiente.
O cliente transfere primeiro.
O cliente envia comprovante/TXID.
O caixeiro confirma e executa a contraparte.
A ordem é concluída.
Ao final, ambos podem avaliar, salvo regras de cancelamento.
4.2 Regra da caução do caixeiro
O caixeiro precisa manter um valor depositado como garantia de boas práticas.
Exemplo fornecido:
se o caixeiro tem 100 USD de caução;
ele só pode pegar uma ordem de até 50 USD, caso a regra de limite seja 50%.
Isso significa que o sistema deve ter um parâmetro configurável:
fator de exposição da caução, por exemplo 0.5.
Fórmula: limite_disponivel = caucao_total * fator_exposicao - exposicao_ativa
A exposição ativa deve considerar ordens em aberto, como:
aceita;
aguardando confirmação;
cancelamento pendente;
disputa.
O sistema deve impedir que um caixeiro aceite ordens acima do seu limite.
4.3 Taxas e comissões
O usuário definiu a lógica abaixo como referência:
cliente paga taxa fixa ao caixeiro: por exemplo 3%;
cliente/contraparte recebe referência líquida de 2,5%;
a plataforma retém 0,5% como comissão;
a comissão da plataforma deve ser editável pelo admin.
O sistema precisa permitir configurar no painel:
percentual cobrado pela plataforma;
fator de caução;
possivelmente percentual padrão do caixeiro.
Essas taxas devem ser parametrizadas, nunca hardcoded.
5. Fluxos de operação
5.1 Fluxo de compra de USDT
Caso de uso: cliente compra USDT com PIX, Wise ou outro método.
Fluxo:
Cliente cria ordem de compra.
Informa valor, rede e método.
Ordem entra na fila disponível.
Caixeiro aceita.
Cliente recebe os dados para pagar.
Cliente paga primeiro.
Cliente envia comprovante no chat.
Caixeiro confirma recebimento.
Caixeiro envia USDT.
Ordem é concluída.
Ambas as partes avaliam.
5.2 Fluxo de venda de USDT
Caso de uso: cliente vende USDT e vai receber moeda fiduciária.
Fluxo:
Cliente cria ordem de venda.
Caixeiro aceita.
Cliente envia USDT primeiro.
Cliente informa TXID/comprovante.
Caixeiro confirma recebimento na rede.
Caixeiro envia PIX ou transferência.
Ordem é concluída.
Ambas as partes avaliam.
5.3 Regra de “quem envia primeiro”
Pelo que foi definido, a lógica da plataforma deve tratar como regra do modelo:
a parte protegida pela estrutura operacional envia primeiro;
no seu desenho prático, o cliente envia primeiro;
o caixeiro só executa depois da confirmação.
Para simplificar no MVP, isso pode ser representado como:
o sistema exige uma ação do cliente chamada “Marcar como enviado/pago”;
depois o caixeiro pode concluir.
6. Cancelamento e mediação
6.1 Cancelamento
O cancelamento não é livre.
Regra obrigatória:
uma parte solicita cancelamento;
a outra parte precisa aceitar;
se aceitar, a ordem encerra;
se recusar, a ordem vai para mediação/disputa.
Regras adicionais:
quem solicitou o cancelamento não pode avaliar a contraparte;
se já houve envio de comprovante ou TXID, o cancelamento não pode simplesmente encerrar a ordem sem rastreio.
6.2 Disputa
Se houver recusa do cancelamento ou divergência operacional:
uma das partes abre disputa;
o admin analisa: 
chat;
comprovantes;
TXIDs;
timestamps;
histórico da ordem;
o admin resolve.
A resolução pode ser:
ordem concluída;
ordem cancelada;
penalização do caixeiro;
uso da caução em caso de dano comprovado;
bloqueio ou blacklist.
7. Sistema de reputação
A reputação deve seguir o padrão de aplicativos comuns, com nota de 1 a 5 estrelas.
Regras:
cliente avalia caixeiro;
caixeiro avalia cliente;
média acumulada deve ser exibida;
contador de avaliações deve ser mantido;
quem solicitou cancelamento não pode avaliar;
o sistema deve impedir avaliação duplicada para a mesma ordem.
Campos necessários:
média;
quantidade de avaliações;
histórico de avaliações.
8. Chat por transação
Cada ordem precisa ter um chat próprio.
Funcionalidades obrigatórias:
mensagens em tempo real ou quase tempo real;
identificação de remetente;
data e hora;
envio de texto;
envio de comprovante/TXID;
visualização do chat pelo admin.
Mesmo no MVP simples, isso pode começar com:
texto;
campo de comprovante como texto;
e depois evoluir para upload real de imagem/pdf.
9. Estrutura visual e navegação
A plataforma deve parecer um aplicativo.
Requisito de navegação:
Cada seção deve abrir como uma página própria, dentro do próprio sistema, sem aparência de site estático.
Páginas mínimas:
/login
/register
/client
/cashier
/admin
/order/[id]
A navegação deve ser do tipo SPA/app:
clicou na aba;
troca de tela;
interface muda como app;
sem sensação de recarregamento bruto.
10. Páginas do sistema
10.1 Login
Campos:
email
senha
Botões:
entrar
criar conta
Mensagens:
usuário pendente
usuário bloqueado
usuário não encontrado
10.2 Cadastro
Campos:
email
senha
tipo de conta: 
cliente
caixeiro
Comportamento:
usuário criado fica como pending;
admin precisa aprovar.
10.3 Dashboard do cliente
Elementos:
criar ordem;
lista de ordens;
status;
histórico;
links para abrir cada ordem.
10.4 Dashboard do caixeiro
Elementos:
saldo de caução;
limite operacional;
exposição atual;
ordens disponíveis;
ordens aceitas;
botão aceitar;
link para detalhes da ordem.
10.5 Dashboard do admin
Elementos:
painel de taxas;
painel de aprovação;
blacklist;
disputas;
auditoria;
relatórios.
10.6 Página da ordem
Elementos:
ID da ordem;
valor;
lado (compra/venda);
método;
rede;
status;
cliente;
caixeiro;
comprovante/TXID;
ações da ordem;
chat;
cancelamento;
disputa;
avaliação.
11. Estados da ordem
A ordem deve ter estados claros.
Sugestão de estados:
created
accepted
paid_by_first_party
completed
cancel_requested
cancel_refused
dispute
resolved
Cada estado deve destravar ou bloquear certas ações.
12. Regras operacionais importantes
12.1 Aprovação
Tanto cliente quanto caixeiro podem precisar de aprovação manual no MVP.
12.2 Blacklist
Usuários podem ser bloqueados e incluídos em blacklist.
12.3 Admin vê tudo
O admin deve conseguir ver:
todas as ordens;
todos os chats;
todas as provas;
todos os logs.
12.4 Auditoria
Cada ação importante deve gerar log:
criação de ordem;
aceite;
envio de comprovante;
conclusão;
pedido de cancelamento;
recusa;
abertura de disputa;
resolução;
avaliação;
aprovação;
blacklist;
ajuste de taxas.
13. Modelo de dados sugerido
13.1 Tabela/coleção users
Campos:
id
email
role
status
ratingAvg
ratingCount
createdAt
13.2 Tabela/coleção orders
Campos:
id
createdAt
updatedAt
side
amountUsd
network
paymentMethod
clientId
cashierId
status
cancelRequestedBy
cancelReason
disputeOpenedBy
disputeNote
adminResolution
proofText
13.3 Tabela/coleção chatMessages
Campos:
id
orderId
at
senderId
text
13.4 Tabela/coleção ratings
Campos:
id
orderId
fromUserId
toUserId
stars
at
13.5 Estrutura de caução
Campos:
cashierId
collateralUsd
13.6 Configurações globais
Campos:
clientPaysCashierPct
clientReceivesPct
platformPct
collateralFactor
13.7 Blacklist
Campos:
userId
reason
createdAt
13.8 Audit log
Campos:
at
action
meta
14. Regras de autorização
Cliente:
só vê as próprias ordens;
só envia mensagem em ordens em que participa;
só avalia ordens em que participou.
Caixeiro:
só vê ordens disponíveis e suas ordens;
só envia mensagem em ordens em que participa;
só conclui ordens aceitas por ele.
Admin:
vê tudo;
altera tudo operacionalmente;
resolve disputas;
aprova usuários;
define parâmetros.
15. Prototipação e testes
O protótipo precisa permitir testar tudo sem depender de infraestrutura externa.
Modo de teste offline
Ideal para o programador:
localStorage ou banco fake;
usuários seed: 
admin@marinsprosper.local
cashier@marinsprosper.local
client@marinsprosper.local
Testes obrigatórios
cliente cria ordem;
caixeiro aceita;
cliente envia comprovante;
caixeiro conclui;
partes avaliam;
cancelar com aceite;
cancelar com recusa;
abrir disputa;
admin resolver;
admin bloquear usuário;
admin alterar taxa;
admin aprovar usuário;
testar limite por caução.
16. Requisitos não funcionais
visual limpo, estilo fintech/banco digital;
UX simples;
responsivo;
preparado para futuro app mobile;
arquitetura que permita migrar de mock/offline para backend real;
logs claros;
separação por páginas/rotas;
nomes de arquivos e estrutura organizados.
17. Integração futura
Mesmo que o MVP seja offline, o projeto deve ser pensado para depois conectar com backend real, como:
Supabase;
FlutterFlow;
Firebase;
API própria.
Idealmente:
separar camada de dados da UI;
funções de negócio isoladas;
páginas já prontas para trocar a fonte de dados depois.
18. Observações legais e operacionais
Como você mencionou Paraguai e intenção de fazer da forma correta, o programador precisa saber que futuramente o sistema pode precisar suportar:
KYC/níveis de verificação;
AML;
retenção de dados;
trilha de auditoria;
documentos legais;
aceite de termos e privacidade;
suporte e SLA;
registro de marca e identidade visual profissional.
Isso não precisa travar o MVP, mas a estrutura deve nascer preparada.
Resumo executivo para o programador
Construir uma plataforma chamada Marinsprosper, estilo fintech/app, com páginas separadas e navegação fluida, contendo três perfis: cliente, caixeiro e admin. O sistema deve permitir ordens P2P de compra e venda de USDT, com regra de que o cliente envia primeiro, aceitação pelo caixeiro condicionada à caução, chat por ordem, comprovante/TXID, cancelamento com aceite da contraparte, disputa com mediação do admin, avaliação 1–5 estrelas, painel admin com taxas configuráveis, aprovação, blacklist e auditoria completa.
