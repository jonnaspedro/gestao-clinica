# 🏥 Sistema de Cadastro de Procedimentos Médicos

Este projeto foi desenvolvido em **Python** para simular um sistema de cadastro de procedimentos e médicos em uma clínica.  
O sistema inclui funcionalidades de inserção, alteração, remoção e consulta de médicos e procedimentos, além do cálculo do salário-base dos médicos com base no tempo dos procedimentos realizados.

Projeto orientado pela **Professora Havana Alves** – Disciplina: **Programação 2**.

---

## 📌 Funcionalidades

- Cadastro de **procedimentos médicos** (código, nome, tempo em minutos).  
- Cadastro de **médicos** com matrícula, CPF, nome, nível de formação e procedimentos realizados.  
- **Validação** de níveis de formação (tupla imutável).  
- Funções para:
  - ➕ **Adicionar médico**
  - ✏️ **Alterar dados do médico**
  - ❌ **Remover médico**
  - 📊 **Calcular salário-base**
  - 📄 **Imprimir dados completos de um médico**

---

## 🛠️ Estrutura do Código

- `NIVEIS_ESPECIALIZACAO` → Níveis válidos de formação médica.  
- `procedimentos_db` → Lista de dicionários com os procedimentos disponíveis.  
- `medicos_db` → Lista de dicionários representando os médicos cadastrados.  

---

## 🧪 Exemplos de Uso

### Adicionando Médico
```python
novo_medico = {
    "matricula": "98765",
    "cpf": "555.666.777-88",
    "nome": "Dra. Ana Costa",
    "nivel_formacao": "Pediatra",
    "procedimentos_realiza": ["CON0015", "CIR0023"]
}
adicionar_medico(novo_medico, medicos_db)
