# api-nest-notas-01-introducao
Notas de aula sobre conceitos introdutórios de api com nest js

## Parte 1: Conceitos Básicos de API REST

### URLs em APIs REST

Uma URL em uma API REST normalmente é composta por:
- **Domínio**: https://api.exemplo.com
- **Caminho**: /usuarios/123
- **Query**: ?status=ativo&ordenar=nome

**Exemplo completo**: https://api.exemplo.com/usuarios/123?status=ativo&ordenar=nome

#### Componentes da URL:

**1. Domínio**
O domínio identifica o servidor da API. É a parte que especifica onde a API está hospedada.
- Exemplo: `https://api.exemplo.com`
- Pode incluir subdomínios específicos para APIs: `https://api.minhaempresa.com`

**2. Caminho da URL**
O caminho determina o recurso específico que você deseja acessar na API.
- Exemplo: `/usuarios/123` - acessa o usuário com ID 123
- Exemplo: `/produtos` - acessa a lista de produtos
- Exemplo: `/categorias/5/produtos` - acessa produtos da categoria 5

**3. Query com parâmetros**
A query permite filtrar ou ordenar os resultados usando parâmetros após o símbolo `?`.
- Exemplo: `?status=ativo` - filtra apenas itens com status ativo
- Exemplo: `?ordenar=nome` - ordena os resultados por nome
- Exemplo: `?status=ativo&ordenar=nome&limite=10` - múltiplos parâmetros separados por `&`

O domínio identifica o servidor da API, o caminho determina o recurso específico, e a query permite filtrar ou ordenar os resultados usando parâmetros.
