pipeline {
    agent any

    environment {
        // Snowflake configuration
        SNOWSQL_ACCOUNT = 'mg05545.eu-west-1'
        SNOWSQL_USER = 'MRAJAMANI'
        SNOWSQL_WAREHOUSE = 'POC_ITIM_PERIASAMY'
        SNOWSQL_DATABASE = 'POC_CICD_PY'
        SNOWSQL_SCHEMA = 'PUBLIC'
        // Path to SnowSQL executable
        SNOWSQL_PATH = '"C:\\Program Files\\SnowSQL\\"'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository containing SQL scripts
                git branch: 'main', url: 'https://github.com/manjunathbabur/CICD_SNOW_TEST.git'
            }
        }

        stage('Verify Environment') {
            steps {
                // Debugging steps to check environment and paths
                bat "echo SNOWSQL PATH: %SNOWSQL_PATH%"
                bat "echo Current PATH: %PATH%"
                bat "dir"
            }
        }

        stage('Run SnowSQL Scripts') {
            steps {
                // Execute SnowSQL to run the SQL script
                bat """
                %SNOWSQL_PATH% ^
                  snowsql ^
                -a %SNOWSQL_ACCOUNT% ^
                -u %SNOWSQL_USER% ^
                -w %SNOWSQL_WAREHOUSE% ^
                -d %SNOWSQL_DATABASE% ^
                -s %SNOWSQL_SCHEMA% ^
                -o log_dir="C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\DEV_SNOWSQL\\logs" ^
                -f scripts\\create_table.sql
                """
            }
        }

        stage('Validation') {
            steps {
                // Validation step to query Snowflake
                bat """
                %SNOWSQL_PATH% ^
                snowsql ^
                -a %SNOWSQL_ACCOUNT% ^
                -u %SNOWSQL_USER% ^
                -w %SNOWSQL_WAREHOUSE% ^
                -d %SNOWSQL_DATABASE% ^
                -s %SNOWSQL_SCHEMA% ^
                -q "SELECT COUNT(*) FROM MY_TABLE;"
                """
            }
        }
    }

    post {
        success {
            echo 'Deployment and validation succeeded!'
        }
        failure {
            echo 'Deployment failed! Check the logs for details.'
        }
    }
}
