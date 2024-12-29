pipeline {
    agent any

    environment {
        SNOWSQL_USER = 'MRAJAMANI'
        SNOWSQL_PASSWORD = credentials('SNOWFLAKE_PASSWORD')
        SNOWSQL_ACCOUNT = 'mg05545.eu-west-1'
        SNOWSQL_WAREHOUSE = 'POC_ITIM_PERIASAMY'
        SNOWSQL_DATABASE = 'POC_CICD_PY'
        SNOWSQL_SCHEMA = 'PUBLIC'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/manjunathbabur/CICD_SNOW_TEST.git'
            }
        }

        stage('Deploy SQL Scripts') {
            steps {
                script {
                    def sqlFiles = findFiles(glob: 'scripts/*.sql')
                    for (file in sqlFiles) {
                        sh """
                        "C:/Program Files/SnowSQL" \
                         snowsql \
                        -a $SNOWSQL_ACCOUNT \
                        -u $SNOWSQL_USER \
                        -w $SNOWSQL_WAREHOUSE \
                        -d $SNOWSQL_DATABASE \
                        -s $SNOWSQL_SCHEMA \
                        -f ${file.path}
                        """
                    }
                }
            }
        }

        stage('Validation') {
            steps {
                sh """
                snowsql \
                -a $SNOWSQL_ACCOUNT \
                -u $SNOWSQL_USER \
                -w $SNOWSQL_WAREHOUSE \
                -d $SNOWSQL_DATABASE \
                -s $SNOWSQL_SCHEMA \
                -q "SELECT COUNT(*) AS RECORD_COUNT FROM MY_DATABASE.MY_SCHEMA.MY_TABLE;"
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment and validation succeeded!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
