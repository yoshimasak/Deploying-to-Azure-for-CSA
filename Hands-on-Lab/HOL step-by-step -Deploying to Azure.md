![Microsoft Cloud Workshop](images/ms-cloud-workshop.png)

Deploy Azure Resources by using GitHub Actions  
Hands-on lab  
Oct 2020

<br />

**Contents**
- [Exercise 1: 手動トリガーのワークフローによるサーバーの展開](#exercise-1-手動トリガーのワークフローによるサーバーの展開)
- [Exercise 2: イベントをトリガーにしたワークフローによるサーバーの展開](#exercise-2-イベントをトリガーにしたワークフローによるサーバーの展開)

<br />

# **Deploying to Azure hands-on training**

## **要約および学習目標**
Azure Resource Manager (ARM) テンプレートを使用することで Azure リソースに IaC (Infrastructure as Code) を提供することができます。Azure リソースの構成をコード化することにより反復可能な一貫性のある展開が可能になり、且つリポジトリにコードを保存することで変更履歴の管理も容易に行えます。GitHub リポジトリでは、GitHub Actions を使用することでワークフローを定義し、CI/CD を実現することが可能です。  
本ワークショップでは、GitHub を使用した ARM テンプレートの管理、ならびに GitHub Actions の Azure CLI アクションによる Azure Resource Manager (ARM) テンプレートのデプロイ方法を学習します。  

### **ワークショップで使用する環境**

<br />

### **事前準備**
- GitHub の Organization を作成し、メンバーを追加
  - Organization の作成  
  <https://docs.github.com/ja/enterprise-server@2.19/admin/user-management/creating-organizations>
  - Organization へのメンバーの追加  
  <https://docs.github.com/ja/enterprise-server@2.19/github/setting-up-and-managing-organizations-and-teams/adding-people-to-your-organization>
- ワークショップで使用するリポジトリを Organiation に fork
  - ワークショップで使用するリポジトリ  
  <https://github.com/hiroyay-ms/Deploying-to-Azure-Hands-on-Lab>
  - リポジトリをフォークする  
  <https://docs.github.com/ja/free-pro-team@latest/github/getting-started-with-github/fork-a-repo>
- Azure ポータルへのアクセス確認
  - Azure ポータル  
  <https://portal.azure.com>

<br />

# **Exercise 1: 手動トリガーのワークフローによるサーバーの展開**
Azure Resource Manager (ARM) テンプレートが保存されている GitHub リポジトリにワークフローを作成し、既存の仮想ネットワークに仮想マシンを展開します。

## **Task 1**: サーバーの展開に使用する資格情報の構成
- Azure リソースを展開するために必要な権限が付与されたサービス プリンシパルの作成
- GitHub シークレットとして資格情報を保存

## **Task 2**: ワークフローを作成し仮想マシンを展開
- 仮想マシンの展開に使用する ARM テンプレートのパラメーター ファイルを作成
- GitHub Actions のワークフローを作成

<br />

## **criteria**
- GitHub Actions で仮想マシンを展開するワークフローが作成されていること
- 仮想マシンの展開に使用する ARM テンプレートのパラメーターはパラメーター ファイルで指定すること
- ワークフローは手動で実行でき、実行時に Resource Group 名を指定できること

<br />

### **参考情報**
- **GitHub Actions のワークフロー構文**  
<https://docs.github.com/ja/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions>
- **ワークフローをトリガーするイベント**  
<https://docs.github.com/ja/free-pro-team@latest/actions/reference/events-that-trigger-workflows>
- **GitHub Actions のメタデータ構文**  
<https://docs.github.com/ja/free-pro-team@latest/actions/creating-actions/metadata-syntax-for-github-actions>
- **GitHub Actions を使用した Azure Resource Manager テンプレートのデプロイ**  
<https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/templates/deploy-github-actions>
- **ARM テンプレートの構造と構文について**  
<https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/templates/template-syntax>
- **Resource Manager パラメーター ファイルを作成する**  
<https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/templates/parameter-files>
- **ARM テンプレートと Azure CLI でリソースをデプロイする**  
<https://docs.microsoft.com/ja-jp/azure/azure-resource-manager/templates/deploy-cli>
- **Create an Azure service principal with th Azure CLI**  
<https://docs.microsoft.com/ja-jp/cli/azure/create-an-azure-service-principal-azure-cli>
- **Encrypted secrets**  
<https://docs.github.com/ja/free-pro-team@latest/actions/reference/encrypted-secrets>

<br />

# **Exercise 2: イベントをトリガーにしたワークフローによるサーバーの展開**
Azure Resource Manager (ARM) テンプレートが保存されている GitHub リポジトリにワークフローを作成し、Azure リソースを展開します。  
展開する Azure リソースは、「リソース グループ」「仮想ネットワーク」「仮想マシン」の3種類です。  
Azure リソースを展開するデータセンターは既存システムが展開されているペアのリージョンを選択してください。  
GitHub Actions のワークフローは、main ブランチへの push 時に実行されます。  
main ブランチを保護は保護され、Pull Request が Merge される前に2人の Reviewer から承認を得るようにします。

## **Task 1**: リポジトリの設定
- ブランチ保護を設定し、Pull Request 時のレビューを必須に設定

## **Task 2**: ワークフローを作成し、Azure リソースを展開
- 仮想マシンの展開に使用する ARM テンプレートのパラメーター ファイルを作成
- GitHub Actions のワークフローを作成

<br />

## **criteria**
- GitHub Actions で Pull Request をトリガーにしたワークフローが作成されていること
- ワークフローは ARM テンプレートのパラメーター ファイルの変更がマージされた際に実行されること
- main ブランチが保護されていること
- Pull Request 時に変更をレビューできること
- SQL Server のデータベースが指定した時間で復元されていること

<br />

### **参考情報**
- **ワークフローをトリガーするイベント**  
<https://docs.github.com/ja/free-pro-team@latest/actions/reference/events-that-trigger-workflows>
- **保護されたブランチの設定**  
<https://docs.github.com/ja/free-pro-team@latest/github/administering-a-repository/configuring-protected-branches>
- **プルリクエスト時の必須レビューを有効にする**  
<https://docs.github.com/ja/free-pro-team@latest/github/administering-a-repository/enabling-required-reviews-for-pull-requests>
- **Azure のペアになっているリージョン** 
<https://docs.microsoft.com/ja-jp/azure/best-practices-availability-paired-regions>
- **Azure VM における SQL Server のバックアップと復元**  
<https://docs.microsoft.com/ja-jp/azure/azure-sql/virtual-machines/windows/backup-restore>
- **Azure VM 上の SQL Server データベースを復元する**  
<https://docs.microsoft.com/ja-jp/azure/backup/restore-sql-database-azure-vm>