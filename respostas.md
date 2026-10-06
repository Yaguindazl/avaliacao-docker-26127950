# Respostas · Avaliação Prática de Docker · Cooperativa AgroVale (Turma A)

Nome: Yago Dias dos Santos
Matrícula: 26127950
Usuário do GitHub: Yaguindazl
Usuário do Docker Hub: yagodias14

## Parte 1 · Dockerfile do portal

**1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?**

Usei a imagem `nginx:1.27-alpine`. Coloquei uma versão fixa para não usar o `latest`.
No `docker images`, a imagem do portal ficou com 73.6MB.

**2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para conferir que o `index.html` está lá dentro.**

O Nginx procura os arquivos em `/usr/share/nginx/html`.

## Parte 2 · Docker Hub

**3. Nome completo da imagem publicada e link público do repositório no Docker Hub.**

A imagem é `yagodias14/agrovale-portal:1.0-26127950`.
O link é https://hub.docker.com/r/yagodias14/agrovale-portal

**4. Por que o `docker login` foi feito com um token de acesso e não com a senha da conta?**

Eu fiz o login no Docker Hub usando um token de acesso em vez da senha. O token é mais seguro porque eu posso definir as permissões e apagar ele depois, caso seja necessário.

Assim, se o token vazar, eu consigo revogar o acesso sem comprometer a senha da minha conta. Já se eu usasse a senha e ela vazasse, minha conta poderia ficar comprometida.


## Parte 3 · Página de manutenção

**5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.**

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|-----------|---------------------|--------------------------|---------------|
| 1 | Faltava o `COPY` depois do `WORKDIR` | A página "Voltamos em breve" não era copiada para dentro da imagem | O container ficou ligado e o log não mostrou erro, mas o navegador mostrou "Welcome to nginx!". Dentro do container só tinha os arquivos padrão do Nginx | Coloquei `COPY site/ /usr/share/nginx/html/`. Usei o caminho completo porque o `WORKDIR` fica uma pasta acima |

**6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?**

No `-p 7042:80`, o 7042 é a porta do meu computador e o 80 é a porta do container.
No `-p 80:7042` é o contrário.
A porta do container é sempre o número da direita.

## Parte 4 · docker-compose.yml

**7. No serviço `blog`, por que `WORDPRESS_DB_HOST` recebe `db` e não `localhost`?**

Dentro do Docker Compose, os serviços se encontram pelo nome. O banco se chama `db`.
Se eu usasse `localhost`, o blog ia procurar o banco dentro dele mesmo, e lá não tem banco.

**8. Por que o serviço `db` não publica a porta 3306? Se precisar consultar o banco sem publicar a porta, como faz? Mostre o comando.**

Só o blog precisa falar com o banco, e os dois já estão na mesma rede.
Sem publicar a porta, ninguém de fora consegue chegar no banco.
Para consultar sem abrir a porta, entro dentro do container com:
`docker compose exec db mariadb -u root -p`
Ele pede a senha do root.

## Parte 5 · Persistência

**9. Quais comandos você usou para derrubar e subir a stack? Qual comando teria apagado o post que você criou, e por quê?**

Para derrubar usei `docker compose down` e para subir de novo usei `docker compose up -d`.
O comando que ia apagar o post é o `docker compose down -v`.
O `-v` apaga os volumes, e o post estava guardado no volume do banco.

**10. Código de conclusão impresso pelo verificador:**

```
AGROVALE-26127950-A1473881
```