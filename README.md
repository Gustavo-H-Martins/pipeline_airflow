```
    # Pelo prompt
    cd /caminho/para/meu_projeto

    # Cria venv com python 3.9
    pipenv install --python=3.9 Flask==2.2.3 apache-airflow==2.5.3
    
    # Define home para airflow em uma raiz do projeto (especificado no arquivo .env)
    echo "AIRFLOW_HOME=${PWD}/airflow" >> .env
    echo "AIRFLOW__CORE__DAGS_FOLDER=${PWD}/outras_dags" >> .env
    
    # Insere o venv criado e carrega o conteúdo do arquivo .env
    pipenv shell
    
    # Alterar o banco do airflow
    airflow config set core sql_alchemy_conn 'postgresql+psycopg2://user:user@localhost:5432/airflow'
    
    # alterar o executor
    airflow config set core executor 'LocalExecutor'

    # não carregar exemplos
    airflow config set core load_examples 'False'
    
    # o mesmo processo acima : 
    ## Para visualizar as variáveis
        # PowerShell
            Get-Content airflow.cfg | Select-String -Pattern 'executor|sql_alchemy_conn|load_examples'

        # Linux
            cat airflow.cfg | grep -E 'executor|sql_alchemy_conn|load_examples'
    ## Para alterar as variáveis
        # PowerShell
            (Get-Content airflow.cfg) | Foreach-Object { $_ -replace 'executor =.*', 'executor = LocalExecutor' } | Set-Content airflow.cfg
            (Get-Content airflow.cfg) | Foreach-Object { $_ -replace 'sql_alchemy_conn =.*', 'sql_alchemy_conn = postgresql+psycopg2://user:user@172.17.195.136:5432/airflow' } | Set-Content airflow.cfg
            (Get-Content airflow.cfg) | Foreach-Object { $_ -replace 'load_examples =.*', 'load_examples = False' } | Set-Content airflow.cfg

        # Linux
            sed -i 's/^executor =.*/executor = LocalExecutor/' airflow.cfg
            sed -i 's/^sql_alchemy_conn =.*/sql_alchemy_conn = postgresql+psycopg2:\/\/user:user@172.17.195.136:5432\/airflow/' airflow.cfg
            sed -i 's/^load_examples =.*/load_examples = False/' airflow.cfg
    
    # Inicializa o airflow
    airflow db init
    mkdir -p ${pwd}/airflow/dags

    # executar o airflow
    airflow webserver -p 8080
```