pipeline {
    agent {
        node {
            label 'android-root-image-builder'
        }
    }

    stages {
        stage('Download kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/1_fetch_kernel.sh android_kernel_planet_mt6873 rooted-stock-android
                    '''
                }
        }

        stage('Build kernel') {
            steps {
                    sh '''
                        bash /opt/common/scripts/2_build_kernel.sh k6873v1_64_defconfig
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

        stage('Root the Android Boot Image') {
            steps {
                sh '''
                    bash /opt/magisk-tools/patch.sh /out/boot.img
                    cp /out/root-boot.img "${WORKSPACE}/astro_root_boot_a.img"
                    '''
            }
        }

        stage('Publish boot image on S3') {
            steps {
               archiveArtifacts artifacts: 'astro_root_boot_a.img', onlyIfSuccessful: true
            }
        }
  }
}
