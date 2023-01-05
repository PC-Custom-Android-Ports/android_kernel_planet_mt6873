pipeline {
    environment {
        SHORT_COMMIT = "${GIT_COMMIT[0..7]}"
    }
    
    agent {
        node {
            label 'android-boot-image-builder'
        }
    }

    stages {
        stage('Download kernel') {
            steps {
                    sh 'bash /opt/common/scripts/1_fetch_kernel.sh android_kernel_planet_mt6873 keyboard-patch-android'
                }
        }

        stage('Build kernel') {
            steps {
                    sh 'bash /opt/common/scripts/2_build_kernel.sh k6873v1_64_defconfig'
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
                sh 'cp /out/boot.img "${WORKSPACE}/astro_boot_${SHORT_COMMIT}_a-keyboard-patch.img"'
            }
        }

        stage('Publish boot image on S3') {
            steps {
               archiveArtifacts artifacts: 'astro_boot_${SHORT_COMMIT}_a-keyboard-patch.img', onlyIfSuccessful: true
            }
        }
  }
}
