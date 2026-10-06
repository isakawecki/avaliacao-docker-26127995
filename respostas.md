# Respostas · Avaliação Prática de Docker · Cooperativa AgroVale (Turma A)

Nome: Isabella
Matrícula: 26127995
Usuário do GitHub: isakawecki
Usuário do Docker Hub: isabella028491



Responda com as suas palavras e com o que aconteceu na SUA máquina. Resposta curta e certa vale mais
do que texto longo copiado. Resposta que contradiz o seu próprio Dockerfile ou compose vale zero.

## Parte 1 · Dockerfile do portal

1. Qual imagem base você usou e qual o tamanho final da imagem do portal (saída de `docker images`)?

Foi usada a imagem base oficial nginx:1.27-alpine. O tamanho final da imagem é de 
73.6MB

2. Em qual pasta do container o Nginx procura os arquivos do site? Mostre o comando que você usou para
   conferir que o `index.html` está lá dentro.

Ele procura os arquivos do site na pasta /usr/share/nginx/html/.Para conferir usei o comando: docker exec teste-portal ls -l /usr/share/nginx/html/

## Parte 2 · Docker Hub

3. Nome completo da imagem publicada e link público do repositório no Docker Hub.
O nome completo da imagem: isabella028491/agrovale-portal:1.0-26127995

Link público do repositório:https://hub.docker.com/r/isabella028491/agrovale-portal
4. Por que o `docker login` foi feito com um token de acesso e não com a senha da conta?

Por questão de segurança. Usar o token evita digitar e expor a senha real da conta no terminal. Se o token vazar ou eu não precisar mais dele, basta excluir ele, sem precisar trocar a senha principal.

## Parte 3 · Página de manutenção

5. Preencha uma linha por defeito encontrado. Defeito inexistente listado aqui desconta pontos.

| # | Instrução | O que estava errado | O que você viu acontecer | Como corrigiu |
|---|---|---|---|---|
| 1 | |Não existia a instrução COPY para transferir a pasta site/ para dentro da imagem. | O contêiner não possuía o arquivo index.html da manutenção ("Voltamos em breve") em seu interior.| Alterado para WORKDIR /usr/share/nginx/html|
| 2 |WORKDIR /usr/share/nginx |O diretório base estava apontando para a pasta pai e não para a pasta onde o Nginx busca os arquivos do site. |O contêiner subiu mas mostrou a página padrão de boas-vindas do Nginx ou erro ao não encontrar o conteúdo correto. |Alterado para WORKDIR /usr/share/nginx/html |
| 3 | |Faltava o comando CMD para executar o Nginx em primeiro plano (foreground). |O contêiner iniciava e encerrava imediatamente (status Exited no docker ps -a). | Adicionada a instrução CMD ["nginx", "-g", "daemon off;"]|

6. Qual a diferença entre `-p 7042:80` e `-p 80:7042` no `docker run`? Qual dos dois números é a porta do container?

-p 7042:80: Abre a porta 7042 no meu computador e redireciona para a porta 80 dentro do container, que é onde o Nginx escuta por padrão.

-p 80:7042: Faz o contrário, redirecionando da porta 80 do computador para a porta 7042 do container. Como nada está rodando na 7042 lá dentro, o site não carrega.

O número que representa a porta do container é o segundo (no caso de -p 7042:80, é a porta 80).
## Parte 4 · docker-compose.yml

7. No serviço `blog`, por que `WORDPRESS_DB_HOST` recebe `db` e não `localhost`?
Porque dentro do Docker os containers conversam pelo nome do serviço. O nome db é resolvido pelo DNS interno da rede do Docker. Se colocar localhost, o WordPress vai procurar o banco dentro do próprio container dele e vai dar erro.

8. Por que o serviço `db` não publica a porta 3306? Se precisar consultar o banco, como faz sem publicar
   a porta? Mostre o comando.

Não precisa publicar a porta 3306 porque o WordPress e o banco já conversam na rede interna do Docker, o que é mais seguro.
Comando para acessar o banco sem expor a porta:
docker exec -it agrovale-db-1 mariadb -u root -p

## Parte 5 · Persistência

9. Quais comandos você usou para derrubar e subir a stack? Qual comando teria apagado o post que você criou,
   e por quê?
Para derrubar e subir mantendo o post:
docker compose down
docker compose up -d

O comando que apagaria o post é o docker compose down -v. A flag -v remove os volumes (db_data e blog_data), que é onde as informações do banco e do post ficam salvas.

10. Código de conclusão impresso pelo verificador:

 Código de conclusão: AGROVALE-26127995-9321BC18
