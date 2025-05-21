# MUX-Multiplexador 2-1

## 1. O que é um MUX 2-para-1?

Um **Multiplexador 2-para-1** é um circuito combinacional que seleciona **um** de dois sinais de entrada e o encaminha para a saída, de acordo com o valor de um sinal de controle.

* **Entradas**:

  * `D0`, `D1` → dados (bits) a serem selecionados
  * `Sel`    → linha de seleção (0 ou 1)
* **Saída**:

  * `Y`      → resultado, igual a `D0` se `Sel=0`, ou `D1` se `Sel=1`

---

## 2. Tabela-verdade

| Sel |  D1 |  D0 |  Y  |
| :-: | :-: | :-: | :-: |
|  0  |  0  |  0  |  0  |
|  0  |  0  |  1  |  1  |
|  0  |  1  |  0  |  0  |
|  0  |  1  |  1  |  1  |
|  1  |  0  |  0  |  0  |
|  1  |  0  |  1  |  0  |
|  1  |  1  |  0  |  1  |
|  1  |  1  |  1  |  1  |

> Note que as colunas `D1` e `D0` aparecem juntas apenas para ilustrar todos os casos, mas a saída **Y** só leva em conta o valor de **um** deles, dependendo de **Sel**.

---

## 3. Expressão lógica

Podemos expressar `Y` como:

```
Y = (¬Sel & D0)  |  (Sel & D1)
```

Em que:

* `¬Sel` = NOT(Sel)
* `&`    = AND
* `|`    = OR

---

## 4. Implementação com portas básicas

![Diagrama MUX 2:1](https://upload.wikimedia.org/wikipedia/commons/thumb/1/1f/Multiplexer_2-1.svg/600px-Multiplexer_2-1.svg.png)

1. **Inversor** gera `¬Sel`.
2. **E1** recebe `¬Sel` e `D0`.
3. **E2** recebe `Sel` e `D1`.
4. **OU** final combina as saídas de E1 e E2 para formar `Y`.

---

## 5. Código Verilog (baseado no repositório)

No seu repositório há implementações em **Verilog** e **VHDL**. Abaixo um exemplo de Verilog, adaptado de [src/mux2to1.v](https://github.com/miguelsrrobo/MUX-Multiplexador) (estrutura típica):

```verilog
module mux2to1 (
    input  wire D0,    // entrada 0
    input  wire D1,    // entrada 1
    input  wire Sel,   // seletor
    output wire Y      // saída
);
    // implementação direta
    assign Y = (Sel) ? D1 : D0;
    // ou usando portas lógicas:
    // assign Y = (~Sel & D0) | (Sel & D1);
endmodule
```

No repositório, você encontra também:

* **VHDL** (em `src/mux2to1.vhd`): estrutura `with Sel select ...`
* **Testbench** (em `sim/`): estimula todas as combinações de Sel/D0/D1 e verifica a saída.

---

## 6. Como usar / simular

1. **Compilar** o módulo no seu ambiente (Quartus, Vivado, Icarus…).
2. **Criar** um testbench que gere pulso para `Sel` e entradas D0, D1.
3. **Observar** a forma de onda: quando `Sel=0` aparente `Y=D0`, e quando `Sel=1`, `Y=D1`.

---

### Dica de estudo

* Tente implementar um **MUX 4-para-1** reutilizando dois MUX 2-para-1 em cascata.
* Compare a síntese em LUTs de FPGA do MUX direto 4:1 versus a versão em cascata.
