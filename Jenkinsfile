pipeline {
    agent {
        node {
            label 'android-boot-image-builder'
        }
    }

    stages {
        stage('Download kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/1_fetch_kernel.sh keyboard-patch-android android_kernel_planet_mt6873
                    '''
                }
        }

        stage('Build kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/2_build_kernel.sh 
                    '''
                }
        }

        stage('Repackage kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/3_copy_kernel.sh && \
                        bash /opt/common/scripts/4_fix_permissions.sh
                    '''
                }
        }

        stage('Copy boot image to master node') {
            steps {
                sh 'cp /out/boot.img "${WORKSPACE}/astro_boot_a-keyboard-patch.img"'
            }
        }

        stage('Publish boot image on S3') {
            steps {
               archiveArtifacts artifacts: 'astro_boot_a-keyboard-patch.img', onlyIfSuccessful: true
            }
        }
  }
}
