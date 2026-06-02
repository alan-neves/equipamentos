
- Acessar https://github.com/fflch/equipamentos

- Fazer o Fork para a sua conta

- Clonar para a sua máquina (Botão verde na lateral direita)
git clone LINK

- cd equipamentos

- Adicionar o upstream 
git remote add upstream https://github.com/fflch/equipamentos.git

- Copiar o env example para o env 
cp .env.example .env

- Editar o .env
	¬ Alterar o banco de dados para equipamentos e o nome a senha do root da sua máquina
	¬ Inserir o token da senha unica
	¬ Inserir a senha do replicado

- Suba os containers
docker compose up -d

- Instale as dependências
docker compose exec equipamentos composer install

- Gerar a key de verificação
docker compose exec equipamentos php artisan key:generate


- Adicionar o tema da USP 
docker compose exec equipamentos php artisan vendor:publish --provider="Uspdev\UspTheme\ServiceProvider" --tag=assets --force

- Criar as tabelas 
docker compose exec equipamentos php artisan migrate

- Acesse o sistema
http://127.0.0.1:8000

## Testes com Dusk

- Copiar o env de testes
    cp .env.dusk.local.example .env.dusk.local

- Criar o banco de testes
    docker compose exec mariadb mariadb -u root -pequipamentos -e "CREATE DATABASE IF NOT EXISTS equipamentos_dusk;"
	
	docker compose exec mariadb mariadb -u root -pequipamentos -e "GRANT ALL PRIVILEGES ON equipamentos_dusk.* TO 'equipamentos'@'%'; FLUSH PRIVILEGES;"

- Rodar as migrations de teste
    docker compose exec equipamentos php artisan migrate --env=dusk.local

- Rodar os testes
    docker compose exec equipamentos php artisan dusk --env=dusk.local

- Para visualizar o browser durante os testes acesse
    http://127.0.0.1:7900  (senha: secret)





