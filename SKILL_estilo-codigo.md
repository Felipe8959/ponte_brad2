---
name: estilo-codigo
description: >
  Estilo de código pessoal do usuário para TODO código gerado neste workspace
  (notebooks, módulos em src/, SQL, testes). Use esta skill SEMPRE que for gerar,
  editar ou refatorar qualquer código — Python, PySpark, SQL, YAML — mesmo que o
  pedido não mencione estilo. Vale junto com a skill classificador-ouvidoria.
---

# Estilo de código

Regras de como o usuário quer que o código seja escrito. Em conflito com estilo
"padrão de mercado", estas regras vencem (exceto se quebrarem funcionamento).

## Comentários

- breves, em minúsculo, sem ponto final, sem formalidade
- só onde agregam: explicar um "porquê" não óbvio, marcar um bloco, avisar um cuidado
- nunca comentar o óbvio, nunca docstring longa cerimoniosa
- docstring, quando necessária, é 1 linha em minúsculo

```python
# bom
# monta a shortlist juntando os dois indices e removendo duplicadas
def montar_shortlist(labels, exemplares, k):
    ...

# ruim
# Esta função é responsável por realizar a montagem da shortlist de candidatos,
# combinando os resultados provenientes dos dois índices vetoriais. 📊
def montar_shortlist(labels, exemplares, k):
    """
    Função que realiza a montagem da shortlist.

    Parameters
    ----------
    ...
    """
```

## Emojis e tom

- **proibido emoji** em qualquer lugar: código, comentários, prints, logs,
  mensagens de erro, nomes de célula, markdown de notebook
- prints e logs curtos, diretos, em minúsculo quando fizer sentido

```python
# bom
print(f"recall@{k}: {recall:.3f}")
print("indice atualizado")

# ruim
print(f"✅ Recall@K calculado com sucesso! 🎯 Valor: {recall}")
print("🚀 Índice atualizado!!!")
```

## Simplicidade

- código simples e direto; nada de over-engineering
- preferir função solta a classe; classe só quando há estado real a carregar
- sem abstração especulativa ("pode ser útil no futuro" = não escreve agora)
- sem try/except genérico engolindo erro; deixar quebrar ou tratar o caso específico
- evitar one-liners espertos ilegíveis; legível > compacto
- type hints só onde ajudam de verdade (assinaturas públicas); sem exagero
- nomes de variáveis/funções em português, snake_case, curtos e claros
  (`montar_prompt`, `df_rotulos`, `k_final`)

```python
# bom
def carregar_rotulos(env="dev"):
    # so o loader conhece nome fisico, ver docs/02
    return carregar("rotulos_historicos", env)

# ruim
class RotulosHistoricosLoaderFactoryImpl:
    def __init__(self, environment_configuration_manager): ...
```

## Notebooks

- células curtas, uma responsabilidade por célula
- célula markdown de título só quando separa seção grande; texto mínimo
- lógica reutilizável vai para `src/`; notebook orquestra e mostra resultado
- sem célula de "setup gigante" com 50 imports; importar o que usa
- resultado de validação/eval: mostrar com display/print simples, sem widget enfeitado

## SQL

- palavras-chave em minusculo (select, from, where), resto também minúsculo
- comentários com `--`, mesmas regras dos comentários python
- CTEs nomeadas em português curto (`com_rotulo`, `apenas_ativas`)

## Checagem final antes de entregar código

1. tem emoji em algum lugar? remover
2. tem comentário formal/maiúsculo/óbvio? encurtar ou apagar
3. tem classe/abstração desnecessária? trocar por função
4. tem docstring cerimoniosa? reduzir a 1 linha ou apagar
5. os prints estão curtos e diretos? ajustar
