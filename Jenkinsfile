node('master') {
    def mvnHome

    stage('Preparation') {
        // سحب الكود من مستودعك الشخصي
        git 'https://github.com/zyphor0/dulla88.git'

        // تحديد أداة Maven (يجب أن تكون مُعدّة مسبقًا في Jenkins كـ "MAVEN")
        mvnHome = tool 'MAVEN'
    }

    stage('Build') {
        // تنفيذ عملية البناء باستخدام Maven
        if (isUnix()) {
            sh "${mvnHome}/bin/mvn -Dmaven.test.failure.ignore clean package"
        } else {
            bat("${mvnHome}\\bin\\mvn -Dmaven.test.failure.ignore clean package")
        }
    }

    stage('Deploy') {
        // نشر ملف الـ WAR إلى Tomcat
        sh '''
            echo "محتوى مجلد target:"
            ls -l $WORKSPACE/target

            echo "إيقاف SELinux مؤقتًا (يُفضل فقط للتجربة)"
            sudo setenforce 0

            echo "حذف ملفات WAR القديمة"
            sudo rm -rf /usr/share/tomcat/webapps/*.war

            echo "نسخ WAR الجديد إلى Tomcat"
            sudo cp $WORKSPACE/target/*.war /usr/share/tomcat/webapps/
        '''
    }
}
