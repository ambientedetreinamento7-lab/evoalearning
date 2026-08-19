# EVOA — Contexto do Projeto

Monorepo Next.js com duas frentes na mesma aplicação:

- **Site institucional/comercial** (público, sem login): landing, soluções,
  metodologia EVOADI®, planos, cases, blog. Otimizado para conversão e SEO.
- **LXP gamificada** (`/plataforma/*`): construída do zero, com arquitetura
  definida por este documento e pelo subagente `backend-supabase-architect`.
  Nenhum código ou schema de plataforma anterior é reaproveitado — construção
  inteiramente nova, seguindo os padrões abaixo desde a primeira linha. Hoje
  serve como vitrine do produto (`/plataforma/demo`, sem login), no futuro é o
  produto real vendido a clientes (`/plataforma/app`, autenticado). Tratar
  desde o início como produto SaaS de referência — não como protótipo.

Banco de dados e auth: Supabase único, separação entre público e privado feita por
RLS + `organization_id`, não por projetos separados.

## Padrão de qualidade do produto (LXP)

A LXP será vendida como produto e precisa ser referência de mercado em:

- **UI/UX**: interações fluidas, feedback imediato em toda ação (progresso,
  pontos, badges), estados de loading/erro/vazio bem desenhados — nunca tela
  quebrada ou spinner infinito sem explicação.
- **Acessibilidade**: WCAG 2.1 AA como piso mínimo — contraste, navegação por
  teclado, `aria-label` em componentes interativos (ranking, badges, barra de
  progresso), textos alternativos em ícones informativos.
- **Performance**: Core Web Vitals no verde tanto no site público quanto dentro da
  plataforma autenticada. Loading states nunca bloqueiam a percepção de
  responsividade.
- **Funcionalidades**: gamificação (pontos, badges, ranking, trilhas, desafios) é
  o diferencial competitivo — priorizar consistência e clareza de regras sobre
  quantidade de features.
- **Segurança**: ver contrato completo em `.claude/agents/backend-supabase-architect.md`.
  Não é camada adicional, é parte do padrão de qualidade — dado de usuário mal
  protegido invalida qualquer diferencial de UX.

Toda decisão de arquitetura ou UI que envolva esses pontos deve ser explicitamente
justificada, não assumida por padrão de biblioteca.

## Design System

Fonte única de verdade dos tokens de marca — nenhum outro agente cria cor,
espaçamento ou tipografia fora daqui.

- Cor primária: `#57BEEB` / institucional: `#1B1D33` / apoio: `#7DD1F5`
- Gradiente principal: `linear-gradient(90deg, #57BEEB 0%, #4FAFD9 50%, #4288BF 100%)`
- Gradiente institucional: `linear-gradient(135deg, #1B1D33 0%, #252846 100%)`
- Tipografia: Poppins ou Inter — H1 700/56px, H2 700/40px, H3 600/28px, corpo
  400/16px, texto pequeno 400/14px
- Espaçamento em sistema de 8pt: 8/16/24/32/48/64/96px
- Border radius: 8/12/16px — cards com `box-shadow: 0 8px 24px rgba(0,0,0,.05)`
- Botão primário: fundo `#57BEEB`, texto branco, radius 8px, sombra
  `0 4px 12px rgba(87,190,235,.30)`
- Container: largura máxima 1200px, centralizado

## Estrutura de rotas

```
/                       → landing pública
/solucoes, /planos etc  → páginas institucionais (SSG/ISR)
/blog, /cases           → conteúdo de prova de eficácia
/plataforma/demo        → LXP em modo showcase (sem login, dado fictício)
/plataforma/app         → LXP real (autenticado, quando existir)
```

## Subagentes disponíveis

- `backend-supabase-architect` — schema, RLS, migrations, auth, segurança de dados
- (adicionar conforme forem criados: design-system-guardian, frontend-builder,
  content-strategist, gamification-engine, devops, qa-accessibility)

Consultar o subagente relevante antes de decisões estruturais — não improvisar
schema ou lógica de autenticação fora do contrato definido.
