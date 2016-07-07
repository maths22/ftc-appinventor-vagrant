$app_engine_version = "1.9.23"

$app_engine_directory = "appengine-java-sdk-#{$app_engine_version}"
$app_engine_zip = "#{$app_engine_directory}.zip"
$app_engine_url = "https://storage.googleapis.com/appengine-sdks/featured/#{$app_engine_zip}"

$script = <<SCRIPT
sudo apt-get update
sudo apt-get install -y openjdk-7-jdk unzip

if ! [ -d /opt/#{$app_engine_directory} ]; then
  wget --progress=bar:force -P /tmp/ #{$app_engine_url}
  unzip /tmp/#{$app_engine_zip} -d /opt/
  rm /tmp/#{$app_engine_zip}
fi

sudo cp /vagrant/ftc-appinventor/init/* /etc/init/

sudo service ftc-appinventor-appengine restart
sudo service ftc-appinventor-buildserver restart

SCRIPT


Vagrant.configure("2") do |config|

    config.vm.box = "ubuntu/trusty32"

    config.vm.network :forwarded_port, guest: 8888, host: 8888

    config.vm.synced_folder "data/", "/opt/ftc-appinventor-war/WEB-INF/appengine-generated"
    config.vm.synced_folder "ftc-appinventor/war/", "/opt/ftc-appinventor-war"
    config.vm.synced_folder "ftc-appinventor/buildserver/", "/opt/ftc-appinventor-buildserver"

    config.vm.provision "shell", inline: $script

    config.vm.provider "virtualbox" do |v|
      v.memory = 1024
    end

end
