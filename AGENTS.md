1. Regra principal

Antes de modificar qualquer código:

leia e entenda o código relacionado ao problema;
investigue a causa raiz;
verifique implementações anteriores relacionadas;
procure regressões recentes quando aplicável;
preserve funcionalidades já existentes;
prefira alterações pequenas, localizadas e reversíveis;
nunca faça uma reescrita ampla quando uma correção pontual resolver.

Não considere uma tarefa concluída apenas porque o código compilou. Sempre valide o comportamento afetado.

2. Proteção da branch main
NUNCA altere main diretamente.
NUNCA faça commit diretamente em main.
NUNCA faça force push.
NUNCA reescreva histórico Git.
Crie uma branch específica para cada tarefa.
Entregue as alterações através de commit e, quando solicitado ou apropriado, Pull Request.
Não faça merge automático no main.
Aguarde aprovação antes de qualquer merge.

3. Não fazer deploy automaticamente

Não faça deploy em produção sem autorização explícita.

Isso inclui:

Cloudflare Workers;
Cloudflare Pages;
Supabase;
VPS/Hostinger;
serviços systemd;
banco de dados de produção;
migrations;
cron jobs;
GitHub Actions com efeito de produção.

É permitido executar build, testes e ambientes locais/de preview.

4. Não destruir infraestrutura ou dados

NUNCA:

apague tabelas;
apague dados;
execute DROP, TRUNCATE ou operações destrutivas;
recrie banco de produção;
resete banco;
remova buckets;
remova secrets;
altere credenciais;
altere DNS;
desative serviços;
destrua Workers;
remova filas;
faça alterações irreversíveis;

sem autorização explícita.

Para mudanças de banco de dados, prefira sempre operações compatíveis, incrementais e reversíveis.

5. Secrets e segurança

Nunca:

coloque secrets no código;
coloque tokens em commits;
exponha API keys;
exponha cookies;
exponha senhas;
exponha credenciais Supabase, Cloudflare, Hostinger ou outros provedores;
copie .env para arquivos versionados.

Use variáveis de ambiente e os mecanismos de secrets já existentes.

Se encontrar um secret versionado, não o reproduza na resposta.

6. Preserve a arquitetura existente

Antes de criar um sistema novo, procure se já existe uma implementação equivalente.

Reutilize:

serviços;
helpers;
APIs;
componentes;
tipos;
hooks;
funções;
filas;
tabelas;
Workers;
pipelines;
mecanismos de cache;

existentes.

Evite duplicação de lógica.

Não crie uma segunda solução paralela para um problema que já possui infraestrutura no projeto.

7. Não alterar partes não relacionadas

Cada tarefa deve possuir o menor escopo possível.

Não faça:

refactors estéticos não solicitados;
renomeações em massa;
formatação de centenas de arquivos;
mudança de dependências sem necessidade;
alteração de configuração global sem necessidade;
troca de biblioteca apenas por preferência pessoal.

Se encontrar outro problema durante a tarefa, relate-o separadamente.

8. Performance é requisito funcional

O Guaxi Ofertas possui forte foco em performance.

Nenhuma alteração pode causar regressão desnecessária em:

LCP;
FCP;
CLS;
TBT;
INP;
TTFB;
tamanho do JavaScript;
número de requests;
tamanho das imagens;
carregamento inicial;
cache;
lazy loading;
responsividade.

Evite adicionar JavaScript ao caminho crítico quando não for necessário.

Prefira:

carregamento lazy;
execução após interação;
execução em idle quando apropriado;
cache;
assets otimizados;
imagens responsivas;
componentes leves.

9. Imagens

Não altere indiscriminadamente o pipeline de imagens.

Ao trabalhar com imagens:

preserve srcset e variantes responsivas quando existentes;
não envie imagens gigantes para elementos pequenos;
preserve proporção;
evite upscale desnecessário;
use dimensões apropriadas;
preserve boa nitidez de logos;
não aplique compressão agressiva em logos;
verifique visualmente se possível;
não substitua logos oficiais por aproximações.

Mudanças no otimizador de imagens devem ser testadas em diferentes tipos de imagem antes de serem consideradas concluídas.

10. Interface e identidade visual

Preserve a identidade visual do Guaxi Ofertas.

Não redesenhe componentes que não fazem parte da tarefa.

Preserve:

responsividade;
hierarquia visual;
espaçamentos;
componentes existentes;
experiência mobile;
experiência desktop.

Não copie visualmente concorrentes.

Referências externas podem servir para entender uma funcionalidade, mas a implementação final deve manter identidade própria do Guaxi.

11. Mobile

Toda alteração de frontend deve considerar pelo menos:

desktop;
mobile.

Não considere uma mudança concluída se ela funciona apenas em uma largura de tela.

Evite:

overflow horizontal;
elementos cortados;
layouts quebrados;
botões inacessíveis;
textos sobrepostos.

12. Cloudflare

Não altere configurações globais do Cloudflare sem necessidade.

Tenha cuidado especial com:

cache;
Workers;
redirects;
headers;
compression;
image optimization;
routing;
bindings;
environment variables.

Uma otimização não deve quebrar comportamento já existente.

Mudanças de cache precisam considerar invalidação e conteúdo personalizado.

13. Supabase

O Supabase deve ser tratado prioritariamente como armazenamento persistente/fonte de verdade, não como local para gravações temporárias excessivas.

Evite introduzir:

polling agressivo;
heartbeat excessivo;
updates redundantes;
alta frequência de escrita;
queries N+1;
scans completos desnecessários;
triggers pesados;
índices redundantes.

Sempre considere custo de:

Disk IO;
WAL;
writes;
locks;
tamanho de tabelas;
frequência de queries.

Não faça mudanças de schema destrutivas.

14. VPS

Não altere serviços existentes da VPS sem compreender sua finalidade.

Não:

mate processos arbitrariamente;
remova serviços systemd;
sobrescreva arquivos de configuração;
altere permissões globalmente;
exponha portas;
altere firewall;

sem necessidade e autorização explícita.

Preserve serviços que já estejam funcionando.

15. Dependências

Não adicione uma biblioteca se o problema puder ser resolvido de forma simples com recursos já disponíveis.

Antes de adicionar dependência:

verifique se já existe biblioteca equivalente no projeto;
considere impacto no bundle;
considere manutenção;
considere segurança;
considere compatibilidade.

Não faça atualização geral de dependências durante uma tarefa não relacionada.

16. Testes obrigatórios

Antes de concluir qualquer alteração, execute todos os testes relevantes disponíveis.

Quando aplicável:

typecheck;
lint;
testes unitários;
testes de integração;
build;
smoke test;
validação funcional da área modificada.

Se algum teste não puder ser executado, explique exatamente por quê.

Nunca diga que algo foi testado se o teste não foi realmente executado.

17. Não mascarar erros

Não "corrija" testes simplesmente:

removendo testes;
desabilitando testes;
adicionando skip;
ignorando TypeScript;
adicionando any indiscriminadamente;
desabilitando ESLint;
suprimindo exceptions;
ocultando erros no console.

Corrija a causa do problema.

18. Tratamento de regressões

Quando o usuário informar que algo "funcionava antes", "regrediu", "parou de funcionar" ou semelhante:

primeiro investigue o histórico Git.

Compare:

implementação atual;
commits anteriores;
código que funcionava;
alterações introduzidas posteriormente.

Sempre que possível, identifique o commit ou alteração responsável pela regressão antes de implementar outra solução.

NÃO reconstrua do zero algo que já havia sido resolvido anteriormente sem primeiro consultar o histórico.

19. Preserve avanços anteriores

Uma correção nova não pode simplesmente substituir uma implementação mais avançada por uma solução simplificada antiga.

Ao corrigir regressões:

restaure o comportamento perdido;
preserve melhorias que continuam válidas;
faça merge conceitual das duas necessidades;
evite rollback amplo quando somente parte da alteração causou o problema.

20. Investigue antes de editar

Para problemas complexos, siga esta ordem:

OBSERVAR → LOCALIZAR → ENTENDER → COMPARAR → FORMULAR HIPÓTESE → ALTERAR → TESTAR → VALIDAR

Não comece pela edição.

21. Erros externos

Diferencie claramente:

bug de código;
problema de configuração;
indisponibilidade de serviço externo;
rate limit;
bloqueio anti-bot;
problema de autenticação;
problema de rede;
problema de dados.

Não altere arquitetura para contornar um erro externo antes de confirmar sua causa.

22. Monetização e links de afiliado

Não remova, substitua ou contorne mecanismos de monetização existentes sem autorização.

Alterações em links devem preservar:

URL final correta;
identificação da loja;
parâmetros necessários;
sistema de afiliados;
fallback já existente.

Evite monetização duplicada.

23. Scraping e coletores

Não crie coletores paralelos se já houver pipeline existente.

Para problemas de scraping:

primeiro determine se o problema ocorre em:

descoberta;
resolução da URL;
parsing;
autenticação;
anti-bot;
monetização;
persistência;
publicação.

Corrija a camada responsável.

24. Filas, retries e jobs

Evite loops infinitos e retries sem limite.

Todo mecanismo de retry deve distinguir:

erro transitório;
erro permanente;
sucesso;
resultado terminal.

Não confirme/ACK um job antes de atingir estado terminal.

Evite gravações repetidas de estado sem alteração.

25. Logs

Logs devem ajudar diagnóstico.

Não introduza logs extremamente verbosos em loops de alta frequência.

Nunca coloque secrets, cookies ou dados sensíveis nos logs.

26. Compatibilidade

Preserve contratos existentes de:

API;
banco;
frontend;
Workers;
serviços da VPS;
filas;
extensões;
consumidores externos.

Uma alteração interna não deve quebrar consumidores existentes sem necessidade.

27. Antes de finalizar

Ao concluir uma tarefa, apresente:

Causa raiz

Explique o que realmente estava causando o problema.

Alterações

Liste apenas as mudanças relevantes realizadas.

Arquivos alterados

Informe quais arquivos foram modificados.

Validação

Informe exatamente os comandos/testes executados e seus resultados.

Riscos

Informe qualquer risco ou comportamento que ainda precise ser observado.

Não realizado

Informe explicitamente qualquer parte da solicitação que não tenha sido possível validar.

28. Comunicação

Responda ao usuário preferencialmente em português do Brasil.

Seja objetivo.

Não esconda falhas.

Não diga "resolvido", "corrigido", "funcionando" ou equivalente antes de realmente validar.

Diferencie claramente:

código alterado;
commit criado;
PR criado;
CI aprovado;
merge realizado;
deploy realizado;
produção validada.

Essas etapas NÃO são equivalentes.

29. Regra crítica de conclusão

Código escrito não significa problema resolvido.

Uma tarefa somente pode ser declarada concluída quando a etapa que era possível validar naquele ambiente tiver sido efetivamente validada.

Se houver CI, aguarde seu resultado quando isso fizer parte da tarefa.

Se houver falha, investigue-a.

Não entregue uma alteração sabidamente com CI vermelho como solução final.

30. Prioridade

Em caso de conflito, siga esta prioridade:

segurança e integridade dos dados;
não quebrar produção;
preservar funcionalidades existentes;
corrigir a causa raiz;
evitar regressões;
performance;
simplicidade;
estética do código.
