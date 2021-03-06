Universidade Federal de São Carlos

Programação Orientada a Objetos Avançada

Julia Cinel Chagas - 759314

***

# **Princípio da Responsabilidade Única (SRP)**

Muitas vezes, principalmente por conta de inexperiência, programadores constroem softwares não tão simples ou fáceis de serem manejados. Isso ocorre muitas vezes por falta do entendimento dos Princípios da Programação Orientada a Objetos (SOLID).

Dentre os princípios que compõem o SOLID, há o Princípio da Responsabilidade Única, que basicamente diz que uma classe deve possuir somente um motivo para mudar, ou seja, ela deve ter apenas uma responsabilidade.

A seguir, observa-se um claro exemplo da violação desse princípio, retirado de um trabalho da disciplina de Programação Orientada a Objetos do segundo semestre de 2019, cujo o objetivo era criar um sistema de gerenciamento de um banco.

## Trecho de código que não segue o SRP:

```cpp
class ContaCorrente {
public:
    ContaCorrente(string cpf_cliente);

    ~ContaCorrente();
	list <Lancamento*>  lista_lancamentos;
    int GetNumero();
	bool FazerLancamento(int, double);

    bool FazerLancamento(int, double, tm data);
    list <Lancamento*> getLancamentos();
    bool debitoConta (double, tm);
    void creditoConta (double, tm);
    static int getQuantidadeContas();
    static double getMontanteTotal();
    void imprimeExtrato(tm, tm);
    string toString() const;

    const string &getCpFcliente() const;
    void setCpFcliente(const string &cpFcliente);

    tm getDataAbertura() const;
    void setDataAbertura(tm dataAbertura);

    double getSaldoAtual() const;
    void setSaldoAtual(double saldoAtual);

	double getLimiteChequeEspecial() const;
	void setLimiteChequeEspecial(double limiteChequeEspecial);

```

No trecho de código acima pode-se identificar pelo menos quatro papéis diferentes que a classe ContaCorrente está realizando. 

Em `imprimeExtrato` tem-se um método cuja função é mostrar na tela o extrato da determinada conta, assim como em `lista_lancamentos` também é exibido na tela uma lista dos lançamentos feitos nesta conta.

Já em `debitoConta` e em `creditoConta` observa-se dois métodos que realizam operações no disco, do tipo de escrita, e a `getLancamentos` que faz uma leitura na memória secundária.

Os métodos `getQuantidadeContasCorrentes` e `getMontanteTotalCorrentes` têm a função de contar quantas contas correntes existem e a quantidade de dinheiro total, respectivamente, ou seja, operações que o banco deve realizar sobre essas contas.

Os demais métodos fazem parte da abstração da conta no sistema, portando extrai da `ContaCorrente` suas funcionalidades e é a visão que o programador tem acerca do sistema criado.

## Trecho de código que segue o SRP:

```cpp
class ContaCorrenteImprime {
public:
    void imprimeExtrato(tm, tm);
    list <Lancamento*>  lista_lancamentos;
}

class ContaCorrenteDados {
public:
    bool debitoConta (double, tm);
    void creditoConta (double, tm);
    list <Lancamento*> getLancamentos();
}

class Banco {
public:
    static int getQuantidadeContasCorrentes();
    static double getMontanteTotalCorrentes();
}

class ContaCorrente {
public:
    ContaCorrente(string cpf_cliente);

    ~ContaCorrente();
    int GetNumero();
	bool FazerLancamento(int, double);

    bool FazerLancamento(int, double, tm data);
    string toString() const;

    const string &getCpFcliente() const;
    void setCpFcliente(const string &cpFcliente);

    tm getDataAbertura() const;
    void setDataAbertura(tm dataAbertura);

    double getSaldoAtual() const;
    void setSaldoAtual(double saldoAtual);

	double getLimiteChequeEspecial() const;
	void setLimiteChequeEspecial(double limiteChequeEspecial);
}
```

Para que seja aplicado de maneira correta o **SRP** neste exemplo, modifica-se a classe `ContaCorrente` dividindo-a em quatro diferentes classes, com objetivo de:
 - Evitar uma classe que faz tudo no código (também chamada de *God Class*), pois pode tornar muito difícil realizar mudanças nesse tipo de classe sem afetar outras funcionalidades dentro dela, ou seja, é ideal evitar dependências entre elas;
 - Melhorar a coesão, isto é, uma classe não deve ter responsabilidades que saem do seu escopo;
 - Modularização do código;
 - Eliminar a dificuldade em relação a criação de testes automatizados.

 Com isso, nota-se que a classe foi dividida para que cada uma tenha uma única responsabilidade. 

## **Referências**

[Vídeo aulas Prof. Daniel Lucrédio](https://www.youtube.com/playlist?list=PLaPmgS59eMSFYb42BcmYzVcClCh0t-26L)

[Princípio da Responsabilidade Única](https://medium.com/@angelomribeiro/princ%C3%ADpio-da-responsabilidade-%C3%BAnica-6d633087fa4e)

[Single-Responsability Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle)

[O que é SOLID: O guia completo para você entender os 5 princípios da POO](https://medium.com/desenvolvendo-com-paixao/o-que-%C3%A9-solid-o-guia-completo-para-voc%C3%AA-entender-os-5-princ%C3%ADpios-da-poo-2b937b3fc530)

[SRP Single Responsibility Principle (Princípio da Responsabilidade Única)](http://techblog.desenvolvedores.net/2017/08/09/srp-single-responsibility-principle-principio-da-responsabilidade-unica/)