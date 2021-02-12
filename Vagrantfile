$install_script = <<-'SCRIPT'
wuauclt.exe /updatenow
choco feature enable -n allowGlobalConfirmation
choco install vcredist140 -y
choco install git -y
choco install 7zip.install -y
choco install firefox -y 
choco install windows-sdk-10-version-1803-windbg -y
choco install dotpeek -y
choco install dnspy -y
choco install choco install javadecompiler-gui -y
choco install cfr -y
choco install classyshark -y
choco install jadx -y
choco install dex2jar -y
choco install apktool -y
choco install vscode-java-debug -y
choco install burp-suite-free-edition -y
choco install vscode -y
choco install python3 --version=3.7.3
choco install pip -y
choco install sysinternals -y
choco install processhacker -y
choco install cygwin -y
choco install msys2 -y
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
pip install frida-tools ipython jupyterlab pandas requests

Add-Type -AssemblyName System.IO.Compression.FileSystem
function Unzip
{
    param([string]$zipfile, [string]$outpath)

    [System.IO.Compression.ZipFile]::ExtractToDirectory($zipfile, $outpath)
}


Invoke-WebRequest -Uri "http://www.split-code.com/files/pd_v2_1.zip" -OutFile "C:\\pd.zip"
Unzip "C:\\pd.zip" "C:\\Windows\\pd"
Remove-Item -Path C:\\pd.zip

New-NetFirewallRule -DisplayName 'jupyter' -Profile 'Private' -Direction Inbound -Action Allow -Protocol TCP -LocalPort 5050

$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
Start-Job -ScriptBlock {
  & jupyter notebook --ip='*' --NotebookApp.token='' --NotebookApp.password='' --no-browser --port 5050 >console.out 2>console.err
}


SCRIPT


Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
    v.memory = 4096
  end

  config.vm.box = "win10pro"
  config.vm.communicator = "winrm"
  config.vm.network :forwarded_port, guest: 3389, host: 3389, id: "rdp", auto_correct: true
  config.vm.network :forwarded_port, guest: 5985, host: 5985, id: "winrm", auto_correct: true
  config.vm.network :forwarded_port, guest: 5050, host: 5050, id: "jupyter", auto_correct: true

  config.vm.provision "shell",
    inline: $install_script
end
