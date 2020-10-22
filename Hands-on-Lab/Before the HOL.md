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
  - [Task 5: Azure Backup のプロビジョニング](#task-5-azure-backup-のプロビジョニング)

## Task 1: リソース グループのプロビジョニング

### パラメーター
- **Resource Group Name**: リソース グループ名
- **Location**: リージョン

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fmain%2FHands-on-Lab%2Ftemplates%2Fdeploy-resource-group.json)

<br />

## Task 2: 仮想ネットワークのプロビジョニング
仮想ネットワークと Azure Bastion の展開  
仮想ネットワーク内には Azure Bastion 用とその他２つのサブネットを作成  

### パラメーター
- **vnetName**: 仮想ネットワーク名
- **vnetAddressPrefix**: 仮想ネットワークの IPv4 アドレス空間
- **bastionSubnetPrefix**: Azure Bastion サブネット アドレス範囲
- **subnet1Name**: サブネットの名前
- **subnet1Prefix**: サブネット アドレス範囲
- **subnet2Name**: サブネットの名前
- **subnet2Prefix**: サブネット アドレス範囲
- **subnet3Name**: サブネットの名前
- **subnet3Prefix**: サブネット アドレス範囲
- **bastionHostName**: Azure Bastion のホスト名

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fmain%2FHands-on-Lab%2Ftemplates%2Fdeploy-vnet-three-subnets-with-bastion.json)

<br />

## Task 3: Active Directory Domain Services の構築
仮想マシンによりドメイン コントローラーを構築

### パラメーター
- **vnetResourceGroup**: 仮想マシンを展開する仮想ネットワークが属すリソース グループ名
- **virtualNetworkId**: 仮想マシンを展開する仮想ネットワーク名
- **subnetName**: 仮想マシンを展開するサブネット名
- **adminUsername**: 管理者アカウント名
- **adminPassword**: パスワード

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fmain%2FHands-on-Lab%2Ftemplates%2Fdeploy-vm-as-domain-member.json)

### 展開後の手動設定
- 仮想マシン展開後のドメイン コントローラーへの昇格
- 仮想ネットワークの DNS サーバーをカスタムに変更し、展開したドメイン コントローラーの IP アドレスを設定

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

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2main%2FHands-on-Lab%2Ftemplates%2Fdeploy-vm-web-and-sql.json)


### 展開後の手動設定
- Web サーバー
  - C:\_setup\set-webserver.ps1 を実行
    - Internet Information Services をインストール
    - ASP.NET アプリケーション（dotnet-sqldb）を wwwroot フォルダにコピー
    - コンソール アプリケーションを C:\time-recorder フォルダにコピー
    - hosts ファイルを C:\Windows\System32\drivers\etc にコピー
  - IIS 管理コンソールを使用し、コピーした dotnet-sqlb をアプリケーションに変換
  - データベース サーバー名を SQL-SVR 以外に指定した場合
    - ASP.NET アプリケーションの Web.config の SQL Server 接続文字列を変更
    - hosts ファイルを変更（IP アドレスが 10.1.2.5 以外の場合は、IP アドレスも変更）
    - C:\time-recorder\time-recorder.exe.config の接続文字列を変更
- データベース サーバー
  - C:\_setup\set-database.ps1 を実行
    - CloudWorkshop データベースをアタッチ
  - SQL Server Management Studio を起動
    - SQL 認証で アカウント SqlUser、パスワード Password.1!! で接続
    - CloudWorkshop データベースに t1 テーブルが存在することを確認
- Web サーバーで C:\time-recorder\time-recorder.exe を実行
  - t1 テーブルにレコードが登録されることを確認
  - Ctrl + C で終了
- http://<Web サーバー名>/dotnet-sqldb でアプリケーションが動作することを確認

<br />

## Task 5: Azure Backup のプロビジョニング
Recovery Services の作成とSQL Server on VM のバックアップを構成

### パラメーター
- **vaultName**: Recovery Services の名前
- **storageType**: バックアップ ストレージの冗長オプション (GeoRedundant)
- **scheduleRunTimes**: バックアップの取得時間 (yyyy-mm-ddThh:00:00.000Z)
- **timeZone**: タイムゾーン (Tokyo Standard Time)

<br />

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhiroyay-ms%2FDeploying-to-Azure-for-CSA%2Fmain%2FHands-on-Lab%2Ftemplates%2Fdeploy-recovery-services.json)

### 展開後の手動設定
- バックアップ構成の **リージョンをまたがる復元** を有効化
- SQL Server 仮想マシンのバックアップを構成
  - サーバー: SQL-SVR (上記手順で作成したデータベース サーバー)
  - バックアップ対象のデータベース: CloudWorkshop
  - ポリシー: SQLServerBackupPolicy  
   (Recovery Services 作成時に作成、日時の完全バックアップ、15 分ごとのログバックアップ、保持期間 7 日)