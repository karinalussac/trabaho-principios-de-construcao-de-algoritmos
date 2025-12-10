vimport pandas as pd
import os

arq = "alunos.csv"

cols = ["Matricula", "Nome", "Rua", "Numero", "Bairro", "Cidade", "UF", "Telefone", "Email"]

def carregar():
    if os.path.exists(arq):
        try:
            df = pd.read_csv(arq)
            return df
        except:
            print("deu erro lendo, vou criar novo")
            return pd.DataFrame(columns=cols)
    else:
        return pd.DataFrame(columns=cols)

def salvar(df):
    df.to_csv(arq, index=False)

def gerarMat(df):
    try:
        m = df["Matricula"].max()
        if pd.isna(m):
            return 1
        return int(m) + 1
    except:
        return 1

def mostrar(al):
    print("\n---- ALUNO ----")
    for x in al:
        print(x, ":", al[x])

def inserir(df):
    print("\nCADASTRO NOVO")

    mat = gerarMat(df)

    nome = input("Nome: ")
    rua = input("Rua: ")
    num = input("Numero: ")
    bairro = input("Bairro: ")
    cidade = input("Cidade: ")
    uf = input("UF: ")
    tel = input("Telefone: ")
    email = input("Email: ")

    novo_linha = {
        "Matricula": mat,
        "Nome": nome,
        "Rua": rua,
        "Numero": num,
        "Bairro": bairro,
        "Cidade": cidade,
        "UF": uf,
        "Telefone": tel,
        "Email": email
    }

    
    df = pd.concat([df, pd.DataFrame([novo_linha])], ignore_index=True)
    salvar(df)

    print("Aluno cadastrado:", nome, "mat:", mat)
    return df

def editar_ou_remover(df, idx):
    aluno = df.loc[idx]
    mostrar(aluno.to_dict())

    print("\n1 - Editar")
    print("2 - Remover")
    print("0 - Voltar")
    op = input("opc: ")

    if op == "1":
        print("\nEDITAR CAMPOS")
        for i in range(1, len(cols)):
            print(i, "-", cols[i], "(atual:", aluno[cols[i]], ")")
        print("0 - cancelar")

        try:
            esc = int(input("qual editar: "))
        except:
            print("errado")
            return df

        if esc == 0:
            return df

        campo = cols[esc]
        novo = input("novo valor: ")

        df.loc[idx, campo] = novo
        salvar(df)
        print("editado")
        return df

    elif op == "2":
        conf = input("tem certeza (s/n)? ")
        if conf.lower() == "s":
            mat = aluno["Matricula"]
            df = df[df["Matricula"] != mat].reset_index(drop=True)
            salvar(df)
            print("removido")
        return df
    
    else:
        return df

def buscar(df):
    if len(df) == 0:
        print("nao tem alunos ainda")
        return df

    print("\nBUSCA")
    print("1 - por matricula")
    print("2 - por nome")
    esc = input("opc: ")

    if esc == "1":
        m = input("mat: ")
        try:
            m = int(m)
        except:
            print("mat invalida")
            return df
        f = df[df["Matricula"] == m]

    elif esc == "2":
        n = input("nome: ").lower()
        f = df[df["Nome"].str.lower().str.contains(n, na=False)]

    else:
        print("erro")
        return df

    if len(f) == 0:
        print("nada encontrado")
        return df

    if len(f) > 1:
        print("varios achados:")
        for i, r in f.iterrows():
            print(r["Matricula"], "-", r["Nome"])
        esc2 = input("mat p/ abrir: ")
        try:
            esc2 = int(esc2)
        except:
            print("mat ruim")
            return df
        f = df[df["Matricula"] == esc2]
        if len(f) == 0:
            print("nao achei")
            return df

    idx = f.index[0]
    return editar_ou_remover(df, idx)

def main():
    df = carregar()

    while True:
        print("\n--- MENU ---")
        print("1 - novo aluno")
        print("2 - buscar aluno")
        print("3 - sair")

        op = input("opc: ")

        if op == "1":
            df = inserir(df)
        elif op == "2":
            df = buscar(df)
        elif op == "3":
            print("tchau")
            break
        else:
            print("opc invalida")

main()
