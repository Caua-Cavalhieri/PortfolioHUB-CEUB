1 Configuração da Gestão de Usuários no GitHub (Papéis e Permissões)
Para o projeto PortfolioHUB, precisamos gerenciar dois tipos de "usuários":

A Equipe de Desenvolvimento: As pessoas que constroem o repositório (PortfolioHUB-CEUB).

Os Usuários da Plataforma: As pessoas que farão login no seu site (PortfolioHUB) usando suas contas GitHub.

A configuração de gestão é diferente para cada um.

A. Para a Equipe de Desenvolvimento (Dentro do Repositório)
Isto define quem pode alterar o código-fonte do seu projeto. Você gerencia isso na aba Settings > Collaborators and teams do seu repositório.

Owner (Proprietário): (Geralmente você, que criou o repositório). Tem controle total, incluindo faturamento e exclusão do repositório.

Admin (Administrador): Controle total do repositório, exceto ações destrutivas (como excluir). Pode gerenciar colaboradores e configurar branch protection.

Recomendação: Atribua este papel a si mesmo e, talvez, ao professor.

Maintain (Mantenedor): Pode escrever no repositório, gerenciar Pull Requests e Releases.

Recomendação: Papel ideal para os desenvolvedores líderes da equipe.

Write (Escrita): Pode fazer push direto para branches e aprovar Pull Requests.

Recomendação: Papel padrão para todos os desenvolvedores da equipe.

Read (Leitura): Pode ver o código e abrir Issues, mas não pode fazer push.

Recomendação: Para stakeholders ou membros de consulta.

B. Para os Usuários da Plataforma (Integração com o PortfolioHUB via OAuth)
Esta é a parte mais importante da integração. Você não gerencia esses usuários adicionando-os como "Colaboradores". Em vez disso, você gerencia as permissões que sua aplicação solicita a eles.

Isso é feito através da configuração do seu GitHub OAuth App.

Vá para: Seu Perfil > Settings > Developer settings > OAuth Apps.

Crie um "New OAuth App" (o PortfolioHUB).

Defina os "Scopes" (Permissões): Os scopes definem o que sua aplicação pode fazer em nome do usuário. Para garantir a conformidade (Princípio do Privilégio Mínimo), você deve solicitar o mínimo possível.

read:user: (Altamente recomendado). Permite que sua aplicação leia o perfil público do usuário (nome, avatar, e-mail). Necessário para identificar quem está logado.

public_repo: (Provavelmente necessário). Permite que sua aplicação leia os repositórios públicos do usuário para exibi-los no portfólio.

NÃO SOLICITE: repo (acesso total a repositórios, incluindo privados) ou admin:* (privilégios de administrador), a menos que seja absolutamente essencial para sua funcionalidade e você possa justificar o risco de segurança.

2. Melhores Práticas e Políticas de Segurança (Para Conformidade)
Aqui está uma lista de políticas de segurança essenciais que você deve implementar para garantir a robustez e conformidade do projeto, conforme solicitado pelo desafio.

🛡️ Política 1: Proteção da Branch Principal (main)
Objetivo: Impedir que código quebrado ou não revisado chegue à branch principal (produção).

Como Implementar (Ação):

Vá para Settings > Branches no seu repositório.

Clique em "Add branch protection rule" para a branch main.

Habilite as seguintes regras:

✅ Require a pull request before merging: (Exigir um Pull Request). Ninguém pode dar push direto para a main.

✅ Require approvals (1): (Exigir aprovações). O Pull Request deve ser revisado e aprovado por pelo menos 1 outro membro da equipe antes de ser mesclado.

(Opcional, recomendado) Require status checks to pass before merging: Se você tiver testes automatizados (GitHub Actions), force-os a passar antes do merge.

🔐 Política 2: Gerenciamento Seguro de Segredos (Chaves de API)
Objetivo: Garantir que suas chaves de API, Client ID e Client Secret do OAuth nunca sejam expostos no código-fonte.

Como Implementar (Ação):

Localmente: Use um arquivo .env (que está listado no .gitignore) para carregar segredos como variáveis de ambiente em sua máquina de desenvolvimento.

No GitHub: NUNCA commite chaves de API.

Para o Backend (Produção): Use o sistema de gerenciamento de segredos do seu provedor de hospedagem (ex: Vercel Environment Variables, Heroku Config Vars, AWS Secrets Manager).

Para GitHub Actions (CI/CD): Use GitHub Secrets (Settings > Secrets and variables > Actions).

📦 Política 3: Monitoramento de Dependências Vulneráveis
Objetivo: Automatizar a detecção e correção de vulnerabilidades conhecidas em pacotes de software que seu projeto utiliza (ex: npm, pip, maven).

Como Implementar (Ação):

Vá para Settings > Code security and analysis.

Habilite o Dependabot alerts e o Dependabot security updates.

Resultado: O Dependabot irá escanear suas dependências e, se encontrar uma vulnerabilidade, ele abrirá um Pull Request automaticamente para atualizar o pacote para uma versão segura.