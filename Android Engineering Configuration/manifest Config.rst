.. _topics-Manifest Configuration

================
AndroidManifest Configuration
================


Permission Configuration
=========================

- Configuring network access

.. code-block:: c

	<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
	<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
	<uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
	<uses-permission android:name="android.permission.INTERNET"/>


Application Configuration
=========================

- Note: If targetSdkVersion 28, you need to add this configuration, otherwise you can ignore the AndroidManifest configuration.

- In the application add android:networkSecurityConfig="@xml/network_security_config"

.. code-block:: c

   <application 
	xmlns:tools="http://schemas.android.com/tools"
	android:allowBackup="false"
	android:icon="@mipmap/ic_launcher"
	android:label="@string/app_name"
	android:supportsRtl="true"
	android:theme="@style/AppTheme"
	tools:replace="android:allowBackup"
	android:usesCleartextTraffic="true"
	android:networkSecurityConfig="@xml/network_security_config">
    </application>


