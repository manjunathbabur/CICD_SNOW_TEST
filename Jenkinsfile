pipeline {
    agent any

    environment {
        // Snowflake configuration
        SNOWSQL_ACCOUNT = 'mg05545.eu-west-1'
        SNOWSQL_USER = 'MRAJAMANI'
        SNOWSQL_WAREHOUSE = 'POC_ITIM_PERIASAMY'
        SNOWSQL_DATABASE = 'POC_CICD_PY'
        SNOWSQL_SCHEMA = 'PUBLIC'
        SNOWSQL_PATH = '"C:\\Program Files\\SnowSQL\\snowsql.exe"'
        SQL_FILE_PATH = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\DEV_SNOWSQL\\scripts\\create_table.sql'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the repository containing the SQL scripts
                git branch: 'main', url: 'https://github.com/manjunathbabur/CICD_SNOW_TEST.git'
            }
        }

        stage('Verify Environment') {
            steps {
                // Debugging steps to verify environment variables and paths
                bat "echo SnowSQL Path: %SNOWSQL_PATH%"
                bat "echo Current PATH: %PATH%"
                bat "dir C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\DEV_SNOWSQL\\scripts"
            }
        }

        stage('Run SnowSQL Script') {
            steps {
                // Execute SnowSQL command to create the table
                bat """
                %SNOWSQL_PATH% ^
                -a %SNOWSQL_ACCOUNT% ^
                -u %SNOWSQL_USER% ^
                -w %SNOWSQL_WAREHOUSE% ^
                -d %SNOWSQL_DATABASE% ^
                -s %SNOWSQL_SCHEMA% ^
                -f %SQL_FILE_PATH% ^
                -o log_level=DEBUG
                """
            }
        }
    }

    post {
        success {
            echo 'Snowflake table creation succeeded!'
        }
        failure {
            echo 'Snowflake table creation failed! Check the logs for details.'
        }
    }
}
-
