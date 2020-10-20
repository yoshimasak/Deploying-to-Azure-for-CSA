![Microsoft Cloud Workshop](images/ms-cloud-workshop.png)

Deploy Azure Resources by using GitHub Actions  
Hands-on lab  
Oct 2020

<br />

**Contents**
- Deploying to Azure before the hands-on lab setup guide
  - [Task 1: リソース グループのプロビジョニング](#task-1-リソース-グループのプロビジョニング)
  - [Task 2: 仮想ネットワークのプロビジョニング](#task-2-仮想ネットワークのプロビジョニング)
  - [Task 3: Active Directory Domain Services の構築](#task-3-active-directory-domain-services-の構築)
  - [Task 4: 仮想マシンのプロビジョニング](#task-4-仮想マシンのプロビジョニング)

## Task 1: リソース グループのプロビジョニング

### パラメーター
- **Resource Group Name**: リソース グループ名
- **Location**: リージョン

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fhiroyay%2FHands-on-Lab%2Ftemplates%2Fdeploy-resource-group.json)

<br />

## Task 2: 仮想ネットワークのプロビジョニング
仮想ネットワークと Azure Bastion の展開  
仮想ネットワーク内には Azure Bastion 用とその他２つのサブネットを作成  

### パラメーター
- **vnetName**: 仮想ネットワーク名
- **vnetAddressPrefix**: 仮想ネットワークの IPv4 アドレス空間
- **bastionSubnetPrefix**: Azure Bastion サブネット アドレス範囲
- **subnet1Prefix**: サブネット アドレス範囲
- **subnet1Name**: サブネットの名前
- **subnet2Prefix**: サブネット アドレス範囲
- **subnet2Name**: サブネットの名前
- **bastionHostName**: Azure Bastion のホスト名

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fhiroyay%2FHands-on-Lab%2Ftemplates%2Fdeploy-vnet-two-subnets-with-bastion.json)

<br />

## Task 3: Active Directory Domain Services の構築
仮想マシンによりドメイン コントローラーを構築

### パラメーター
- **vnetResourceGroup**: 仮想マシンを展開する仮想ネットワークが属すリソース グループ名
- **virtualNetworkId**: 仮想マシンを展開する仮想ネットワーク名
- **subnetName**: 仮想マシンを展開するサブネット名
- **adminUsername**: 管理者アカウント名
- **adminPassword**: パスワード

### 展開後の手動設定
- 仮想マシン展開後のドメイン コントローラーへの昇格
- 仮想ネットワークの DNS サーバーをカスタムに変更し、展開したドメイン コントローラーの IP アドレスを設定

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fhiroyay%2FHands-on-Lab%2Ftemplates%2Fdeploy-vm-as-domain-controller.json)

<br />

## Task 4: 仮想マシンのプロビジョニング
２台の仮想マシンを展開（ドメイン参加なし）  
- Web サーバー (Web-SVR: Windows Server 2019)
- データベース (SQL-SVR: SQL Server 2019 on Windows Server 2019)

### パラメーター
- **vnetResourceGroup**: 仮想マシンを展開する仮想ネットワークが属すリソース グループ名
- **virtualNetworkId**: 仮想マシンを展開する仮想ネットワーク名
- **subnetName**: 仮想マシンを展開するサブネット名
- **webVirtualMachineName**: Web サーバー名 (Web-SVR)
- **webOSVersion**: Web サーバーの OS バージョン (2019-Datacenter)
- **webVMSize**: Web サーバーの仮想マシン インスタンス サイズ (Standard_D2s_v3)
- **sqlVirtualMachineName**: データベース サーバー名 (SQL-SVR)
- **sqlVersion**: SQL Server のバージョン (SQL2910-WS2019)
- **sqlVMSize**: SQL Server の仮想マシン インスタンス サイズ (Standard_D2s_v3)
- **adminUsername**: 管理者アカウント名
- **adminPassword**: パスワード

### 展開後の手動設定
- Web サーバーに IIS を追加し、ASP.NET アプリケーションを展開

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fhiroyay%2FHands-on-Lab%2Ftemplates%2Fdeploy-vm-web-and-sql.json)

<br />
