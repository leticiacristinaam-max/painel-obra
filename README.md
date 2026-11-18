# 📊 Painel do Cronograma — Primavera P6

Painel web de acompanhamento de obra que **se atualiza sozinho a partir do Primavera P6**. O planejador lança o andamento no P6, aperta **F9** e, minutos depois, a gestão vê o status atualizado, no computador ou no celular, com acesso protegido por senha.

> 🔒 **Os dados deste repositório são criptografados.** Os arquivos `app.enc` e `dados.enc` só podem ser lidos com a senha de acesso, e nenhuma informação do projeto fica em texto aberto aqui.

---

## O que o painel mostra

| Bloco | O que responde |
|---|---|
| **Indicadores** | Término previsto × linha de base, fim do asfalto e folga, avanço físico real × planejado, atividades atrasadas, caminho crítico e sobrealocação |
| **O que precisa de atenção** | Alertas automáticos por gravidade: caminho crítico atrasado, atividades que já deveriam ter começado ou terminado, folga perdida, necessidade de nivelamento, data date desatualizada e edições sem F9 |
| **Estrada do asfalto** | Toneladas de CBUQ executadas × saldo do contrato, com a projeção de dias de usina |
| **Curva S** | Avanço físico acumulado: planejado (linha de base) × projetado, com o realizado na data date |
| **Carga dos recursos** | Mapa semanal da ocupação de cada equipamento e equipe em relação ao limite diário |
| **Tabelas** | Atrasadas (com o motivo), caminho crítico, atividades que perderam folga, sobrealocação dia a dia e asfalto por trecho |

---

## Como funciona

```
Primavera P6 (banco local)
        │  F9 / salvar
        ▼
Script em Python (no computador do planejador, somente leitura)
  • lê atividades, ligações, linha de base, calendário e recursos
  • calcula variações, folgas, Curva S, carga dos recursos e alertas
  • gera o painel local e criptografa os dados
        │
        ▼
GitHub Pages (este repositório)
  • index.html → tela de acesso
  • app.enc / dados.enc → painel e dados criptografados
        │
        ▼
Navegador da gestão: digita a senha → o painel é decifrado no próprio aparelho
```

- **Atualização automática:** um processo em segundo plano percebe as mudanças no banco do P6 e publica uma nova versão. As publicações são limitadas a uma a cada 10 minutos, e só acontecem quando os números mudam.
- **Página sempre atual:** quem estiver com o painel aberto recebe os dados novos sem recarregar a página.
- **Sem servidor e sem banco na nuvem:** tudo é estático. O P6 nunca é alterado pelo script, que só lê o banco.

---

## Segurança

- O painel e os dados são criptografados **antes** de sair do computador do planejador.
- A chave é derivada da senha com **PBKDF2-SHA256 (200 mil iterações)**. O conteúdo é cifrado com fluxo HMAC-SHA256 em modo contador, e a integridade é conferida com HMAC-SHA256.
- A decifragem acontece **no navegador de quem acessa** (Web Crypto API). A senha nunca é enviada a nenhum servidor.
- Sem a senha, o site mostra apenas a tela de acesso.
- O site pede aos buscadores para não ser indexado (`robots.txt` e `noindex`).

---

## Arquivos

| Arquivo | Função |
|---|---|
| `index.html` | Tela de acesso e decifragem no navegador |
| `app.enc` | Interface do painel (criptografada) |
| `dados.enc` | Dados do cronograma (criptografados) |
| `logo_engebio.png`, `foto_obra.jpg` | Imagens da tela de acesso |
| `robots.txt`, `.nojekyll` | Configuração do site |

> Os arquivos deste repositório são **gerados automaticamente**. Alterações manuais são sobrescritas na próxima publicação.

---

## Tecnologias

- **Primavera P6 Professional:** planejamento (EAP, lógica, calendário, recursos, linhas de base, nivelamento)
- **Python** (só biblioteca padrão): leitura do banco do P6, cálculos e publicação
- **HTML, CSS e JavaScript puros:** painel, gráficos e animações, sem frameworks
- **Web Crypto API:** decifragem no navegador
- **GitHub Pages:** hospedagem estática

---

## Acesso

O acesso é restrito. Para solicitar a senha, fale com o responsável pelo planejamento da obra.
