# SISTEMA DE CADASTRO DE PROCEDIMENTOS MÉDICOS
Elaboramos um código simples usando python para propor um cadastro de procedimentos para uma clínica
# Níveis de especialização médica
NIVEIS_ESPECIALIZACAO = ("Clínico Geral", "Pediatra", "Cardiologista", "Neurologista")
print("Níveis de Especialização Médica Definidos:", NIVEIS_ESPECIALIZACAO)

Observação: nesse exemplo usamos uma tupla para garantir que os valores não possam ser alterados
# Banco de Dados de Procedimentos Médicos (Uma lista de dicionários para armazenar os procedimentos)
procedimentos_db = [
    {"codigo": "CON0015", "nome": "Consulta Básica", "tempo_minutos": 30},
    {"codigo": "EXA0020", "nome": "Exame Cardiológico", "tempo_minutos": 15},
    {"codigo": "EXA0022", "nome": "Ressonância Magnética", "tempo_minutos": 40},
    {"codigo": "CIR0023", "nome": "Cirurgia Pediátrica", "tempo_minutos": 180}, # 3 horas = 180 minutos
    {"codigo": "EXA0102", "nome": "Eco Cardiograma", "tempo_minutos": 20},
]
print("\n--- Banco de Dados de Procedimentos Inseridos ---")
for proc in procedimentos_db:
    print(proc)
    
#Estrutura do Banco de Dados de Médicos e Médico Exemplo(Uma lista de dicionários para o banco de dados de médicos)

medicos_db = []

# Exemplo de dados de um médico a ser armazenado

medico_exemplo = {
    "matricula": "12345",
    "cpf": "111.222.333-44",
    "nome": "Dr. João Silva",
    "nivel_formacao": "Cardiologista", # Deve estar em NIVEIS_ESPECIALIZACAO
    "procedimentos_realiza": ["EXA0020", "EXA0102"] # Códigos dos procedimentos que realiza
}

# Armazena o médico de exemplo no banco de dados

medicos_db.append(medico_exemplo)
print("\n--- Médico de Exemplo Adicionado ao Banco de Médicos ---")
for medico in medicos_db:
    print(medico)

# Função para Adicionar Médico ao Banco de Dados 

def adicionar_medico(novo_medico, banco_medicos):
  
    Adiciona um novo médico ao banco de dados de médicos.
    Args:
        novo_medico (dict): Dicionário contendo os dados do médico.
        banco_medicos (list): Lista que representa o banco de dados de médicos.
    
    # Validação simples do nível de formação
    if novo_medico["nivel_formacao"] not in NIVEIS_ESPECIALIZACAO:
        print(f"Erro: Nível de formação '{novo_medico['nivel_formacao']}' inválido para {novo_medico['nome']}. Não adicionado.")
        return

    # Evita adicionar médicos com a mesma matrícula (matrícula como chave primária implícita)
    for medico in banco_medicos:
        if medico["matricula"] == novo_medico["matricula"]:
            print(f"Erro: Já existe um médico com a matrícula {novo_medico['matricula']}. Não adicionado.")
            return

    banco_medicos.append(novo_medico)
    print(f"\nMédico {novo_medico['nome']} (Matrícula: {novo_medico['matricula']}) adicionado com sucesso!")

# Funções Auxiliares para Manipulação do Banco de Médicos

# Adições para "adicionar, remover e alterar"

def remover_medico(matricula, banco_medicos):
    
    Remove um médico do banco de dados pela matrícula.
    Args:
        matricula (str): Matrícula do médico a ser removido.
        banco_medicos (list): Lista que representa o banco de dados de médicos.
    Returns:
        bool: True se o médico foi removido, False caso contrário.
  
    medico_encontrado = False
    for i, medico in enumerate(banco_medicos):
        if medico["matricula"] == matricula:
            banco_medicos.pop(i)
            medico_encontrado = True
            print(f"\nMédico com matrícula {matricula} removido com sucesso!")
            break
    if not medico_encontrado:
        print(f"\nMédico com matrícula {matricula} não encontrado para remoção.")
    return medico_encontrado

def alterar_dados_medico(matricula, novos_dados, banco_medicos):
   
    Altera os dados de um médico no banco de dados.
    Args:
        matricula (str): Matrícula do médico a ser alterado.
        novos_dados (dict): Dicionário com os novos dados a serem atualizados.
        banco_medicos (list): Lista que representa o banco de dados de médicos.
    Returns:
        bool: True se os dados foram alterados, False caso contrário.
  
    medico_encontrado = False
    for medico in banco_medicos:
        if medico["matricula"] == matricula:
            # Validação para nível de formação ao alterar
            if "nivel_formacao" in novos_dados and novos_dados["nivel_formacao"] not in NIVEIS_ESPECIALIZACAO:
                print(f"Erro: Nível de formação '{novos_dados['nivel_formacao']}' é inválido. Alteração cancelada para nível de formação.")
                del novos_dados["nivel_formacao"] # Remove a chave inválida para não causar erro
            medico.update(novos_dados)
            medico_encontrado = True
            print(f"\nDados do médico com matrícula {matricula} alterados com sucesso!")
            break
    if not medico_encontrado:
        print(f"\nMédico com matrícula {matricula} não encontrado para alteração.")
    return medico_encontrado

# Exemplo de uso da função adicionar_medico:
novo_medico_2 = {
    "matricula": "98765",
    "cpf": "555.666.777-88",
    "nome": "Dra. Ana Costa",
    "nivel_formacao": "Pediatra",
    "procedimentos_realiza": ["CON0015", "CIR0023"]
}
adicionar_medico(novo_medico_2, medicos_db)

# Exemplo de adicionar um médico com nível de formação inválido (será recusado)
novo_medico_invalido = {
    "matricula": "10101",
    "cpf": "000.000.000-00",
    "nome": "Dr. Invalido",
    "nivel_formacao": "Oftalmologista",
    "procedimentos_realiza": ["CON0015"]
}
adicionar_medico(novo_medico_invalido, medicos_db)

print("\n--- Banco de Dados de Médicos Atualizado ---")
for medico in medicos_db:
    print(medico)

# Função para Calcular Salário-Base do Médico
VALOR_POR_30_MINUTOS = 34.15
VALOR_POR_MINUTO = VALOR_POR_30_MINUTOS / 30

def calcular_salario_base(matricula, banco_medicos, banco_procedimentos):
   
    Calcula o salário-base mensal de um médico com base nos procedimentos que ele realiza. [cite: 8]
    Args:
        matricula (str): Matrícula do médico.
        banco_medicos (list): Lista que representa o banco de dados de médicos.
        banco_procedimentos (list): Lista que representa o banco de dados de procedimentos.
    Returns:
        tuple: (salario_base, total_minutos_trabalhados) ou (None, None) se o médico não for encontrado.
    """
    total_minutos_trabalhados = 0
    medico_encontrado = None

    for medico in banco_medicos:
        if medico["matricula"] == matricula:
            medico_encontrado = medico
            break

    if medico_encontrado:
        for cod_proc in medico_encontrado["procedimentos_realiza"]:
            found_proc = False
            for proc in banco_procedimentos:
                if proc["codigo"] == cod_proc:
                    total_minutos_trabalhados += proc["tempo_minutos"]
                    found_proc = True
                    break
            if not found_proc:
                print(f"Atenção: Procedimento com código '{cod_proc}' não encontrado no banco de procedimentos para o médico {medico_encontrado['nome']}.")
        salario_base = total_minutos_trabalhados * VALOR_POR_MINUTO
        return salario_base, total_minutos_trabalhados
    else:
        return None, None

Função para Imprimir Dados do Médico

def imprimir_dados_medico(matricula, banco_medicos, banco_procedimentos):
  
    Imprime os dados completos de um médico no formato especificado.
    Args:
        matricula (str): Matrícula do médico a ser impresso.
        banco_medicos (list): Lista que representa o banco de dados de médicos.
        banco_procedimentos (list): Lista que representa o banco de dados de procedimentos.
    """
    medico_encontrado = None
    for medico in banco_medicos:
        if medico["matricula"] == matricula:
            medico_encontrado = medico
            break

    if medico_encontrado:
        print(f"\n--- Dados do Médico: Matrícula {matricula} ---")
        print(f"Matrícula: {medico_encontrado['matricula']}")
        print(f"Nome: {medico_encontrado['nome']}")
        print(f"CPF: {medico_encontrado['cpf']}")
        print(f"Especialização: {medico_encontrado['nivel_formacao']}")
        print("Procedimentos realizados:")
        
        procedimentos_do_medico = []
        for cod_proc in medico_encontrado["procedimentos_realiza"]:
            for proc in banco_procedimentos:
                if proc["codigo"] == cod_proc:
                    procedimentos_do_medico.append(proc)
                    break
        
        if procedimentos_do_medico:
            for proc in procedimentos_do_medico:
                print(f"  {proc['codigo']} - {proc['nome']} - {proc['tempo_minutos']} minutos")
        else:
            print("  Nenhum procedimento cadastrado para este médico ou códigos inválidos.")

        salario_base, total_minutos_trabalhados = calcular_salario_base(matricula, banco_medicos, banco_procedimentos)
        
        if salario_base is not None:
            print(f"Horas trabalhadas no mês: {total_minutos_trabalhados / 60:.2f}") # Convertendo minutos para horas
            print(f"Salário-base: R$ {salario_base:.2f}")
        else:
            print("Não foi possível calcular o salário-base para este médico.")
    else:
        print(f"\nMédico com matrícula {matricula} não encontrado no banco de dados.")

# EXECUÇÃO DE EXEMPLOS E TESTES

print("\n--- Testes de Funcionalidades ---")

# Testando a alteração de dados de um médico
alterar_dados_medico("12345", {"nome": "Dr. João Carlos Silva"}, medicos_db)
imprimir_dados_medico("12345", medicos_db, procedimentos_db)

# Testando a remoção de um médico

remover_medico("98765", medicos_db)
imprimir_dados_medico("98765", medicos_db, procedimentos_db) # Tentando imprimir médico removido

# Adicionando o "Seu Zé" conforme o exemplo da prova para testar impressão

medico_seu_ze = {
    "matricula": "11111",
    "cpf": "777.888.999-00",
    "nome": "Seu Zé",
    "nivel_formacao": "Clínico Geral", # Adicionei um nível para consistência
    "procedimentos_realiza": ["EXA0020"]
}
adicionar_medico(medico_seu_ze, medicos_db)
imprimir_dados_medico("11111", medicos_db, procedimentos_db)

# Testando com um médico que não existe

imprimir_dados_medico("00000", medicos_db, procedimentos_db)

# Testando alteração de nível de formação inválido

alterar_dados_medico("12345", {"nivel_formacao": "Oftalmologista"}, medicos_db)
imprimir_dados_medico("12345", medicos_db, procedimentos_db) # Verifica se a alteração não ocorreu

print("\n--- Fim do Sistema ---")

Projeto orientado pela Professora Havana Alves da Disciplina Programação 2 
