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
        SNOWSQL_PATH = '"C:\\Program Files\\SnowSQL\\snowsql.exe"'
        // Full path to the SQL file
        SQL_FILE_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\DEV_SNOWSQL\\scripts\\create_table.sql'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository containing SQL scripts
                git branch: 'main', url: 'https://github.com/manjunathbabur/CICD_SNOW_TEST.git'
            }
        }

        stage('Run SnowSQL Script') {
            steps {
                // Execute SnowSQL to run the create_table.sql script
                bat """
                %SNOWSQL_PATH% ^
                -a %SNOWSQL_ACCOUNT% ^
                -u %SNOWSQL_USER% ^
                -w %SNOWSQL_WAREHOUSE% ^
                -d %SNOWSQL_DATABASE% ^
                -s %SNOWSQL_SCHEMA% ^
                -f %SQL_FILE_PATH%
                """
            }
        }
    }

    post {
        success {
            echo 'Table creation succeeded!'
        }
        failure {
            echo 'Table creation failed! Check the logs for details.'
        }
    }
}
