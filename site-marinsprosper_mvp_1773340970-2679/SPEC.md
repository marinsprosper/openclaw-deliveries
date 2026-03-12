Marinsprosper — Plataforma P2P (MVP offline)
Este site é um protótipo SPA (tipo app) com:
- Login/Register + aprovação (pending) por admin
- Perfis: cliente / caixeiro / admin
- Ordens: created -> accepted -> paid_by_first_party -> completed
- Cancelamento: cancel_requested (precisa aceite) -> resolved / dispute
- Chat por ordem + prova/TXID
- Reputação (rating 1–5) com regra simples (1 avaliação por ordem)
- Caução/limite do caixeiro: limite = caução * fator - exposição ativa
- Auditoria: log das ações principais
Tudo funciona em localStorage (sem backend), mas já separado para plugar Supabase/Firebase/API depois.
