import csv

class Imovel:
    def __init__(self, quartos, vagas):
        self.quartos = quartos
        self.vagas = vagas

    def calcular_valor(self):
        pass


class Apartamento(Imovel):
    def __init__(self, quartos, vagas, tem_crianca):
        super().__init__(quartos, vagas)
        self.tem_crianca = tem_crianca

    def calcular_valor(self):
        valor = 700
        if self.quartos == 2:
            valor += 200
        if self.vagas:
            valor += 300
        if not self.tem_crianca:
            valor *= 0.95
        return valor


class Casa(Imovel):
    def calcular_valor(self):
        valor = 900
        if self.quartos == 2:
            valor += 250
        if self.vagas:
            valor += 300
        return valor


class Estudio(Imovel):
    def __init__(self, vagas):
        super().__init__(0, vagas)

    def calcular_valor(self):
        valor = 1200
        if self.vagas > 0:
            valor += 250
            if self.vagas > 2:
                valor += (self.vagas - 2) * 60
        return valor


class Orcamento:
    CONTRATO = 2000

    def __init__(self, imovel):
        self.imovel = imovel

    def gerar(self):
        aluguel = self.imovel.calcular_valor()
        parcelas_contrato = self.CONTRATO / 5
        return aluguel, parcelas_contrato


def gerar_csv(valor):
    with open("parcelas.csv", "w", newline="") as arquivo:
        writer = csv.writer(arquivo)
        writer.writerow(["Mês", "Valor"])
        for i in range(1, 13):
            writer.writerow([i, f"{valor:.2f}"])


# PROGRAMA PRINCIPAL
print("1 - Apartamento")
print("2 - Casa")
print("3 - Estúdio")
tipo = int(input("Escolha o tipo de imóvel: "))

if tipo == 1:
    quartos = int(input("Quantidade de quartos: "))
    vagas = input("Possui garagem? (s/n): ").lower() == 's'
    crianca = input("Possui crianças? (s/n): ").lower() == 's'
    imovel = Apartamento(quartos, vagas, crianca)

elif tipo == 2:
    quartos = int(input("Quantidade de quartos: "))
    vagas = input("Possui garagem? (s/n): ").lower() == 's'
    imovel = Casa(quartos, vagas)

else:
    vagas = int(input("Quantidade de vagas: "))
    imovel = Estudio(vagas)

orcamento = Orcamento(imovel)
valor_aluguel, contrato = orcamento.gerar()

print(f"\nValor do aluguel mensal: R$ {valor_aluguel:.2f}")
print(f"Contrato: 5x de R$ {contrato:.2f}")

gerar_csv(valor_aluguel)
print("Arquivo parcelas.csv gerado com sucesso!")
