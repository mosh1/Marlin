node {
	stage 'Checkout'
	checkout scm

	stage 'Load and prepare Ardunio'
	sh '''wget http://downloads-02.arduino.cc/arduino-1.6.4-linux64.tar.xz
tar Jxf arduino-1.6.4-linux64.tar.xz'''

	stage 'Prepare dummy Xserver'
	sh '''daemonize -E BUILD_ID=dontKillMe /usr/bin/Xvfb :1 -screen 0 1024x768x16
DISPLAY=:1.0'''

	stage 'Install additional Arduino libraries'
	sh '''export DISPLAY=:1.0
./arduino-1.6.4/arduino --install-library "U8glib,LiquidCrystal"'''
	
	stage 'Build'
	sh '''export DISPLAY=:1.0
/usr/local/bin/generate_version_header_for_marlin ./Marlin/_Version.h "BigBox Hybrid-Dual"
cat ./Marlin/_Version.h
./arduino-1.6.4/arduino --verify --board arduino:avr:mega:cpu=atmega2560 Marlin/Marlin.ino \\
--pref build.path=./buildroot'''

	stage 'Archive'
	sh '''mv buildroot/Marlin.cpp.hex buildroot/Marlin.${JOB_BASE_NAME##*%2F}.hex'''

	archive 'buildroot/Marlin.*.hex'

	stage 'Clean'
	deleteDir()
}