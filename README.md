# Ataque de Força Bruta com BurpSuite

Este documento descreve, em nível conceitual, como funciona o processo de teste de força bruta em um formulário de autenticação utilizando o BurpSuite. O objetivo é demonstrar a análise de respostas e o comportamento do servidor durante um teste de segurança realizado em ambiente autorizado.

---

## 1. Definindo o Alvo

O primeiro passo consiste em interceptar a requisição de login através do **BurpSuite Proxy**.  
A requisição capturada é enviada para o módulo **Intruder**, que é responsável por automatizar múltiplas tentativas de autenticação para fins de teste.

---

## 2. Selecionando o Ponto de Ataque

No Intruder, seleciona-se o campo que será alvo do teste.  
Como o objetivo é avaliar a robustez da senha, o **campo de senha** é marcado como posição de ataque.

Como apenas um parâmetro será variado (a senha), utiliza-se o modo de ataque **Sniper**, adequado para testes em um único campo.

---

## 3. Configurando o Payload

No menu **Payloads**, define-se a fonte das tentativas que serão utilizadas no teste:

- Tipo: *Simple List*
- Wordlist: subconjunto de 3.500 senhas extraído da wordlist **rockyou.txt**

Cada valor da lista será inserido no campo de senha, substituindo a posição indicada na requisição original.

---

## 4. Executando o Teste e Interpretando as Repostas

Com o ataque em execução, o BurpSuite apresenta as respostas retornadas pelo servidor em cada tentativa.  
O principal parâmetro avaliado é o cabeçalho **Location**, que indica para onde o servidor redireciona o usuário após a tentativa de login.

Os comportamentos observados foram:

- **Tentativas inválidas:**  
  O campo *Location* permanece apontando para `login.php`, indicando que o usuário continua na página de autenticação.  
  Isso significa que a senha informada está incorreta.

- **Tentativa bem-sucedida:**  
  Para uma das senhas, o valor de *Location* passou a apontar para `index.php`, a página inicial da aplicação.  
  Isso indica sucesso no login.

Nesse teste, a senha **“password”** foi a única que resultou em redirecionamento para `index.php`, identificando-a como a senha válida dentro do ambiente autorizado.

