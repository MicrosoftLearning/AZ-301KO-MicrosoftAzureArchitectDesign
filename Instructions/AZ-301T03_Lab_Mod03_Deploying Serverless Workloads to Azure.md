---
lab:
    title: 'Azure에 서버리스 워크로드 배포'
    module: '모듈 3: Azure에서 서버리스 응용 프로그램 제작'
---

# Azure에서 서버리스 응용 프로그램 제작

# 랩 답변 키: Azure에 서버리스 워크로드 배포

## 시작하기 전

1. 다음 자격 증명을 사용하여 Windows 10 랩 가상 머신에 로그인되어 있는지 확인합니다.

    - 사용자 이름: **Admin**

    - 암호: **Pa55w.rd**

1. Windows 10 바탕 화면 하단에 있는 작업 표시줄을 검토합니다. 작업 표시줄에는 랩에서 사용할 일반적인 응용 프로그램에 대한 아이콘이 포함되어 있습니다.

    - Microsoft Edge

    - 파일 탐색기

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **참고**: **시작 메뉴** 에서 이러한 응용 프로그램의 바로 가기를 찾을 수 있습니다.

## 연습 1: 웹앱 만들기

#### 작업 1: Azure Portal 열기

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 브라우저 창이 열리면 **Azure Portal**(<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩에서 사용할 Azure 구독의 소유자 역할이 있는 사용자 계정으로 인증합니다.

#### 작업 2: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

    > **참고**: **Cloud Shell** 아이콘은 *보다 큼* 문자와 *밑줄* 문자의 조합으로 구성된 기호입니다.

1. 구독을 사용하여 **Cloud Shell** 을 처음 연 경우라면 처음 사용할 수 있도록 **Cloud Shell** 을 구성하는 마법사가 표시됩니다. 메시지가 표시되면 **Azure Cloud Shell 시작** 창에서 **Bash(Linux)** 를 클릭합니다.

    > **참고**: **Cloud Shell** 에 대한 구성 옵션이 표시되지 않으면 이 과정의 랩에서 기존 구독을 사용하고 있기 때문일 수 있습니다. 이 경우 다음 작업을 바로 진행합니다. 

1. **탑재된 스토리지가 없음** 창에서 **고급 설정 표시** 를 클릭하고 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값으로 설정된 채로 둡니다.

    - **Cloud Shell 지역** 드롭다운 목록에서 이 랩에서 리소스를 배포하려는 위치와 일치하거나 가까운 Azure 지역을 선택합니다.

    - **리소스 그룹** 섹션에서 **새로 만들기** 옵션을 선택한 후 텍스트 상자에 **AADesignLab0901-RG** 를 입력합니다.

    - **스토리지 계정** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 3~24자 길이의 문자와 숫자 조합으로 구성된 고유한 이름을 입력합니다. 

    - **파일 공유** 섹션에서 **새로 만들기** 옵션을 선택한 다음 아래 텍스트 상자에 **cloudshell** 을 입력합니다.

    - **스토리지 만들기** 단추를 클릭합니다.

1. 다음 작업을 진행하기 전에 **Cloud Shell** 이 첫 번째 설치 절차를 완료할 때까지 기다립니다.

#### 작업 3: App Service 계획 만들기

1. 포털 하단에 있는 **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 연습에 사용할 리소스 그룹의 이름을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_APP='AADesignLab0502-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 Azure 지역을 지정하는 값에 대한 변수를 만듭니다(메시지가 표시되면 지역 이름 입력).

    ```sh
    read -p 'Region: ' LOCATION
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 리소스 그룹을 만듭니다.

    ```sh
    az group create --name $RESOURCE_GROUP_APP --location $LOCATION
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 App Service 계획을 만듭니다.

    ```sh
    az appservice plan create --is-linux --name "AADesignLab0502-$LOCATION" --resource-group $RESOURCE_GROUP_APP --location $LOCATION --sku B2
    ```

#### 작업 4: 웹앱 인스턴스 만들기

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 Linux 기반 App Service 웹앱 인스턴스에 사용 가능한 런타임 목록을 표시합니다. 

    ```sh
    az webapp list-runtimes --linux
    ``` 

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 웹앱의 이름으로 사용할 무작위로 생성된 문자열 값에 대한 새 변수를 만듭니다.

    ```sh
    WEBAPPNAME1=webapp05021$RANDOM$RANDOM
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 고유한 이름을 사용하여 새 웹앱을 만듭니다.

    ```sh
    az webapp create --name $WEBAPPNAME1 --plan AADesignLab0502-$LOCATION --resource-group $RESOURCE_GROUP_APP --runtime "DOTNETCORE|2.1"
    ```

    > **참고**: 중복된 웹앱 이름으로 인해 명령이 실패하는 경우 명령이 성공적으로 완료될 때까지 마지막 두 단계를 다시 실행합니다.

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

#### 작업 5: 배포 결과 보기

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0502-RG** 를 클릭합니다.

1. **AADesignLab0502-RG** 블레이드에서 이 연습의 앞부분에서 만든 Azure 웹앱을 나타내는 항목을 클릭합니다.

1. 웹앱 블레이드에서 블레이드 상단의 **찾아보기** 단추를 클릭합니다.

1. Azure App Service에서 생성된 기본 페이지를 검토합니다.

1. 새 브라우저 탭을 닫고 Azure Portal을 표시하는 브라우저 탭으로 돌아갑니다.

> **복습**: 이 연습에서는 빈 웹앱이 포함된 Linux 기반 App Service 계획을 만들었습니다. 


## 연습 2: 웹앱 코드 배포

#### 작업 1: Azure Resource Manager 템플릿 및 GitHub를 사용하여 웹앱 확장을 포함한 코드 배포

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 연습에서 사용할 리소스 그룹의 이름을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_APP='AADesignLab0502-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 Azure 지역을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0502-RG'].location" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 웹앱의 이름으로 사용할 무작위로 생성된 문자열 값에 대한 새 변수를 만듭니다.

    ```sh
    WEBAPPNAME2=webapp05022$RANDOM$RANDOM
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 고유한 이름을 사용하여 새 웹앱을 만듭니다.

    ```sh
    az webapp create --name $WEBAPPNAME2 --plan AADesignLab0502-$LOCATION --resource-group $RESOURCE_GROUP_APP --runtime "NODE|9.4"
    ```

    > **참고**: 중복된 웹앱 이름으로 인해 명령이 실패하는 경우 명령이 성공적으로 완료될 때까지 마지막 두 단계를 다시 실행합니다.


1. **Cloud Shell** 창에서 **파일 업로드/다운로드** 아이콘을 클릭하고 드롭다운 메뉴에서 **업로드** 를 클릭합니다. 

1. **열기** 대화 상자에서 **\\allfiles\\AZ-301T03\\Module_03\\Labfiles\\Starter\\** 폴더로 이동하고 **github.json** 파일을 선택한 다음 **열기** 를 클릭합니다. 파일에는 다음과 같은 Azure Resource Manager 템플릿이 포함되어 있습니다.

    ```json
    {
        "$schema": "http://schemas.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "webAppName": {
                "type": "string"
            },
            "repositoryUrl": {
                "type": "string"
            },
            "branch": {
                "type": "string",
                "defaultValue": "master"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/sites",
                "name": "[parameters('webAppName')]",
                "location": "[resourceGroup().location]",
                "properties": {},
                "resources": [
                    {
                        "apiVersion": "2015-08-01",
                        "name": "web",
                        "type": "sourcecontrols",
                        "dependsOn": [
                            "[resourceId('Microsoft.Web/Sites', parameters('webAppName'))]"
                        ],
                        "properties": {
                            "RepoUrl": "[parameters('repositoryUrl')]",
                            "branch": "[parameters('branch')]",
                            "IsManualIntegration": true
                        }
                    }
                ]
            }
        ]
    }
    ```

1. **Cloud Shell** 창에서 **파일 업로드/다운로드** 아이콘을 클릭하고 드롭다운 메뉴에서 **업로드** 를 클릭합니다. 

1. **열기** 대화 상자에서 **\\allfiles\\AZ-301T03\\Module_03\\Labfiles\\Starter\\** 폴더로 이동하고 **parameters.json** 파일을 선택한 다음 **열기** 를 클릭합니다. 이 파일에는 이전에 업로드한 Azure Resource Manager 템플릿에 대한 다음과 같은 매개 변수가 포함되어 있습니다.

    ```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "webAppName": {
          "value": "$WEBAPPNAME2"
        },
        "repositoryUrl": {		
          "value": "$REPOSITORY_URL"
        },
        "branch": {
          "value": "master"
        }
      }
    }
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 웹앱 코드를 호스팅하는 GitHub 리포지토리의 이름을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    REPOSITORY_URL='https://github.com/Azure-Samples/nodejs-docs-hello-world'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 웹앱 코드를 호스팅하는 GitHub 리포지토리의 이름을 지정하는 값을 포함하고 URL에 포함될 수 있는 특수 문자를 고려하는 변수를 만듭니다.

    ```sh
    REPOSITORY_URL_REGEX="$(echo $REPOSITORY_URL | sed -e 's/\\/\\\\/g; s/\//\\\//g; s/&/\\\&/g')"
    ```

    > **참고**: 이 변수는 **sed** 유틸리티를 사용하여 이 문자열을 Azure Resource Manager 템플릿 매개 변수 파일에 삽입하기 위해 필요합니다. 또는 파일을 열고 URL 문자열을 파일에 직접 입력할 수 있습니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **webAppName** 매개 변수 값의 자리 표시자를 매개 변수 파일의 **$WEBAPPNAME2** 변수 값으로 바꿉니다.

    ```sh
    sed -i.bak1 's/"$WEBAPPNAME2"/"'"$WEBAPPNAME2"'"/' ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 **repositoryUrl** 매개 변수 값의 자리 표시자를 매개 변수 파일의 **$REPOSITORY_URL** 변수 값으로 바꿉니다.

    ```sh
    sed -i.bak2 's/"$REPOSITORY_URL"/"'"$REPOSITORY_URL_REGEX"'"/' ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 매개 변수 파일에서 자리 표시자가 성공적으로 바뀌었는지 확인합니다.

    ```sh
    cat ~/parameters.json
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 로컬 Azure Resource Manager 템플릿과 로컬 매개 변수 파일을 사용하여 GitHub 상주 웹앱 코드를 배포합니다.

    ```sh
    az group deployment create --resource-group $RESOURCE_GROUP_APP --template-file github.json --parameters @parameters.json
    ```

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

    > **참고**: 배포에는 약 1분이 소요됩니다.

#### 작업 2: 배포 결과 보기

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0502-RG** 를 클릭합니다.

1. **AADesignLab0502-RG** 블레이드에서 이전 작업에서 만든 Azure 웹앱을 나타내는 항목을 클릭합니다.

1. 웹앱 블레이드에서 블레이드 상단의 **찾아보기** 단추를 클릭합니다.

1. GitHub에서 배포된 샘플 Node.js 웹 응용 프로그램을 검토합니다.

1. 새 브라우저 탭을 닫고 Azure Portal을 표시하는 브라우저 탭으로 돌아갑니다.

#### 작업 3: Docker Hub 컨테이너 이미지로 코드 배포

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 작업에서 사용할 리소스 그룹의 이름을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_CONTAINER='AADesignLab0502-RG'
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 배포에 사용할 Azure 지역을 지정하는 값에 대한 변수를 만듭니다.

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab0502-RG'].location" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 새 웹앱의 이름으로 사용할 무작위로 생성된 문자열 값에 대한 새 변수를 만듭니다.

    ```sh
    WEBAPPNAME3=webapp05023$RANDOM$RANDOM
    ```

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 고유한 이름을 사용하여 새 웹앱을 만듭니다.

    ```sh
    az webapp create --name $WEBAPPNAME3 --plan AADesignLab0502-$LOCATION --resource-group $RESOURCE_GROUP_CONTAINER --deployment-container-image ghost
    ```

    > **참고**: 중복된 웹앱 이름으로 인해 명령이 실패하는 경우 명령이 성공적으로 완료될 때까지 마지막 두 단계를 다시 실행합니다.

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

    > **참고**: 배포에는 1분 미만이 소요됩니다.

1. **Cloud Shell** 창을 닫습니다.


#### 작업 4: 배포 결과 보기

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0502-RG** 를 클릭합니다.

1. **AADesignLab0502-RG** 블레이드에서 이전 작업에서 만든 Azure 웹앱을 나타내는 항목을 클릭합니다.

1. 웹앱 블레이드에서 블레이드 상단의 **찾아보기** 단추를 클릭합니다.

    > **참고**: 응용 프로그램이 나타나지 않으면 웹앱 블레이드로 전환하고 블레이드 상단의 **다시 시작** 단추를 클릭한 다음 **찾아보기** 를 다시 클릭합니다. 

1. Docker Hub에서 배포된 블로그 응용 프로그램을 검토합니다. 

1. 새 브라우저 탭을 닫고 Azure Portal을 표시하는 브라우저 탭으로 돌아갑니다.

> **복습**: 이 연습에서는 Azure Resource Manager 템플릿 및 Docker Hub 이미지를 사용하여 App Service 웹앱에 코드를 배포했습니다.


## 연습 3: 함수 앱 배포

#### 작업 1: Azure Resource Manager 템플릿을 사용하여 코드로 함수 앱 배포

1. Azure Portal의 왼쪽 상단에서 **리소스 만들기** 를 클릭합니다.

1. **새로 만들기** 블레이드 상단에 있는 **마켓플레이스 검색** 텍스트 상자에 **템플릿 배포** 를 입력하고 **Enter** 키를 누릅니다.

1. **모두** 블레이드의 검색 결과에서 **템플릿 배포** 를 클릭합니다.

1. **템플릿 배포** 블레이드에서 **만들기** 단추를 클릭합니다.

1. **사용자 지정 배포** 블레이드에서 **편집기에서 사용자 고유의 템플릿을 빌드합니다.** 링크를 클릭합니다.

1. **템플릿 편집** 블레이드에서 **빠른 시작 템플릿** 링크를 클릭합니다.

1. **빠른 시작 템플릿 로드** 창의 **템플릿 선택** 드롭다운 목록에서 **201-function-app-dedicated-github-deploy** 템플릿을 선택합니다.

1. **확인** 단추를 클릭합니다.

1. **템플릿 편집** 블레이드로 돌아가 **저장** 단추를 클릭하여 템플릿을 유지합니다.

1. **사용자 지정 배포** 블레이드로 돌아가서 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값으로 설정된 채로 둡니다.

    - **리소스 그룹** 섹션에서 **새로 만들기** 옵션을 선택한 후 아래 텍스트 상자에 **AADesignLab0503-RG** 를 입력합니다.

    - **App Name(앱 이름)** 텍스트 상자에 새 함수 앱에 대한 고유한 이름을 입력합니다.

    - **Sku** 드롭다운 목록에서 **Basic(기본)** 옵션을 선택합니다.

    - **Worker Size(작업자 크기)** 드롭다운 목록을 기본값으로 설정된 채로 둡니다.

    - **Storage Account Type(스토리지 계정 형식)** 드롭다운 목록을 기본값으로 설정된 채로 둡니다.

    - **Repo URL(리포지토리 URL)** 필드를 기본값으로 설정된 채로 둡니다.

    - **Branch(분기)** 텍스트 상자를 기본값으로 설정된 채로 둡니다.

    - **Location(위치)** 텍스트 상자를 기본값으로 설정된 채로 둡니다.

    - **사용 약관** 섹션에서 **위에 명시된 사용 약관에 동의함** 확인란을 선택합니다.

    - **구매** 단추를 클릭합니다.

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

    > **참고**: 배포에는 약 1분이 소요됩니다.


#### 작업 2: 배포 결과 보기

1. Azure Portal의 허브 메뉴에서 **리소스 그룹** 을 클릭합니다.

1. **리소스 그룹** 블레이드에서 **AADesignLab0503-RG** 를 클릭합니다.

1. **AADesignLab0503-RG** 블레이드에서 이전 작업에서 만든 함수 앱을 나타내는 항목을 클릭합니다.

1. 함수 앱 블레이드에서 **URL** 항목을 찾고 아래에 있는 하이퍼링크를 클릭하여 새 브라우저 탭에 함수 앱 방문 페이지를 표시합니다.

1. 새 브라우저 탭을 닫고 Azure Portal을 표시하는 브라우저 탭으로 돌아갑니다.

> **복습**: 이 연습에서는 Azure Resource Manager 템플릿을 사용하여 함수 앱 및 코드를 배포했습니다.


## 연습 4: 랩 리소스 제거

#### 작업 1: Cloud Shell 열기

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 모든 리소스 그룹을 나열합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab05')]".name --output tsv
    ```

1. 출력에 이 랩에서 만든 리소스 그룹만 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **Enter** 키를 눌러 이 랩에서 만든 리소스 그룹을 삭제합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab05')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.


> **복습**: 이 연습에서는 이 랩에서 사용한 리소스를 제거했습니다.
