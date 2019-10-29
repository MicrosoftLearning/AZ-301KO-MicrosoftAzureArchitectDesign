---
lab:
    title: 'Azure 솔루션에 사용할 네트워크 인프라 배포'
    module: '모듈 3: Azure 응용 프로그램 구성 요소 네트워킹'
---

# Azure 솔루션에 사용할 네트워크 인프라 배포

# 랩 응답 키: Azure 솔루션에 사용할 네트워크 인프라 배포

## 시작하기 전에

1. 다음 자격 증명을 사용하여 Windows 10 랩 가상 컴퓨터에 로그인되어 있는지 확인합니다.

    - 사용자 이름: **Admin**

    - 암호: **Pa55w.rd**

1. Windows 10 바탕 화면 하단에 있는 작업 표시줄을 검토합니다. 작업 표시줄에는 랩에서 사용할 일반적인 응용 프로그램 아이콘이 포함되어 있습니다.

    - Microsoft Edge

    - 파일 탐색기

    - [Visual Studio Code](https://code.visualstudio.com/)

    - [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/)

    - Bash on Ubuntu on Windows

    - Windows PowerShell

    > **참고**: **시작 메뉴**에서 이러한 응용 프로그램에 대한 바로 가기를 찾을 수도 있습니다.

## 실습 1: 랩 환경 구성

#### 작업 1: Azure Portal 엽니다.

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열려 있는 브라우저 창에서 **Azure Portal** (<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩에서 사용할 Azure 구독의 소유자 역할이 있는 사용자 계정으로 인증합니다.

#### 작업 2: Cloud Shell 엽니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

    > **참고**: **Cloud Shell** 아이콘은 *초과*와 *밑줄* 문자의 조합으로 구성된 기호입니다.

1. 구독을 사용하여 **Cloud Shell**을 처음 여는 경우, 처음에만 사용할 수 있도록 **Cloud Shell**을 구성하는 마법사가 표시됩니다. 메시지가 표시되면 **Azure Cloud Shell 환영 인사** 창에서 **Bash (Linux)**를 클릭합니다.

    > **참고**: **Cloud Shell**에 대한 구성 옵션이 표시되지 않는 경우, 대부분은 이 과정의 랩에서 기존 구독을 사용하고 있기 때문일 수 있습니다. 그런 경우, 다음 작업으로 직접 진행합니다. 

1. **장착 스토리지가 없습니다** 창에서 **고급 설정 보기**를 클릭한 다음, 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값 설정으로 유지합니다.

    - **Cloud Shell 영역** 드롭다운 목록에서 이 랩에서 리소스를 배포할 위치와 일치 또는 근처에 있는 Azure 영역을 선택합니다

    - **리소스 그룹** 섹션에서 **기존 리소스 사용** 옵션을 선택하고 드롭다운 목록에서 **AADesignLab0901-RG**를 선택합니다.

    - **스토리지 계정** 섹션에서 **새로 생성** 옵션을 선택한 다음, 아래 텍스트 상자에 3~24개 문자와 숫자로 구성된 고유한 이름을 입력합니다. 

    - **파일 공유하기** 섹션에서 **새로 생성** 옵션을 선택한 다음 아래 텍스트 상자에 **cloudshell**을 입력합니다.

    - **스토리지 생성** 버튼을 클릭합니다.

1. 다음 작업을 진행하기 전에 **Cloud Shell**이 첫 번째 설치 절차를 완료할 때까지 기다립니다.

#### 작업 3: Azure Cloud Shell 에 Azure Building Blocks npm 패키지를 설치합니다.

1. 포털 하단의 **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure Building Blocks npm 패키지를 설치하는 로컬 디렉터리를 만듭니다.

    ```sh
    mkdir ~/.npm-global
    ```

1. *Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 npm 구성을 업데이트하여 새 로컬 디렉터리를 포함시킵니다.

    ```sh
    npm config set prefix '~/.npm-global'
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 편집을 위해 ~./bashrc 구성 파일을 엽니다.

    ```sh
    vi ~/.bashrc
    ```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 파일을 아래쪽으로 스크롤(또는 **G**를 입력), 마지막 줄의 오른쪽 가장 끝 문자까지 오른쪽으로 스크롤(또는 **$**를 입력), **a**를 입력하여 **삽입** 모드를 시작하고, **엔터**를 눌러 새 줄을 시작한 다음, 다음을 입력하여 새로 만든 디렉터리를 시스템 경로에 추가합니다.

    ```sh
    export PATH="$HOME/.npm-global/bin:$PATH"
    ```

1. **Cloud Shell** 명령 프롬프트의 vi 편집기 인터페이스에서 변경 내용을 저장하고 파일을 닫기 위해 ** Esc**를 누르고 ** :**를 누른 다음, ** wq!**를 입력한 후 **엔터**를 누릅니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure Buiding Blocks npm 패키지를 설치합니다.

    ```sh
    npm install -g @mspnp/azure-building-blocks
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 셸을 종료합니다.

    ```sh
    exit
    ```

1. **Cloud Shell 시간 초과** 창에서 **재연결**을 클릭합니다. 

    > **참고**: Buliding Blocks npm 패키지를 설치하려면 Cloud Shell을 다시 시작해야 합니다.


#### 작업 3: Building Blocks Hub 및 Spoke 매개 변수 파일 준비합니다.

1. **Cloud Shell** 창에서 **파일 업로드/다운로드** 아이콘을 클릭하고 드롭다운 메뉴에서 **업로드**를 클릭합니다. 

1. **열기** 대화상자에서 **\\allfiles\\AZ-301T04\\Module_03\\LabFiles\\Starter\\**폴더로 이동하여 **hub-nva.json**, **hub-vnet.json**, **hub-vnet-peering.json**, **spoke1.json** 및 **spoke2.json** 파일을 선택한 다음, **열기**를 클릭합니다. 

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminUsername** 매개 변수의 자리 표시자를 **hub-vnet.json** Building Blocks 매개 변수 파일에서 **학생** 값으로 바꿉니다.

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/hub-vnet.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminPassword** 매개 변수의 자리 표시자를 **hub-vnet.json** Building Blocks 매개 변수 파일에서 **Pa55w.rd1234** 값으로 바꿉니다.

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/hub-vnet.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **hub-vnet.json** Building Block 매개 변수 파일에서 매개 변수 값이 성공적으로 변경되었는지 확인합니다.

    ```sh
    cat ~/hub-vnet.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminUsername** 매개 변수의 자리 표시자를 **hub-nva.json** Building Blocks 매개 변수 파일에서 **학생** 값으로 바꿉니다.

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/hub-nva.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminPassword** 매개 변수의 자리 표시자를 **hub-nva.json** Building Blocks 매개 변수 파일에서 **Pa55w.rd1234** 값으로 바꿉니다.

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/hub-nva.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **hub-nva.json** Building Blocks 매개 변수 파일에서 매개 변수 값이 성공적으로 변경되었는지 확인합니다.

    ```sh
    cat ~/hub-nva.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminUsername** 매개 변수의 자리 표시자를 **spoke1.json** Building Blocks 매개 변수 파일에서 **학생** 값으로 바꿉니다.

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/spoke1.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminPassword** 매개 변수의 자리 표시자를 **spoke1.json** Building Blocks 매개 변수 파일에서 **Pa55w.rd1234** 값으로 바꿉니다.

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/spoke1.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **spoke1.json** Building Blocks 매개 변수 파일에서 매개 변수 값이 성공적으로 변경되었는지 확인합니다.

    ```sh
    cat ~/spoke1.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminUsername** 매개 변수의 자리 표시자를 **spoke2.json** Building Blocks 매개 변수 파일에서 **학생** 값으로 바꿉니다.

    ```sh
    sed -i.bak1 's/"adminUsername": ""/"adminUsername": "Student"/' ~/spoke2.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **adminPassword** 매개 변수의 자리 표시자를 **spoke2.json** Building Blocks 매개 변수 파일에서 **Pa55w.rd1234** 값으로 바꿉니다.

    ```sh
    sed -i.bak2 's/"adminPassword": ""/"adminPassword": "Pa55w.rd1234"/' ~/spoke2.json
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 **spoke2.json** Building Blocks 매개 변수 파일에서 매개 변수 값이 성공적으로 변경되었는지 확인합니다.

    ```sh
    cat ~/spoke2.json
    ```

#### 작업 4: Hub 및 Spoke 설계 허브 구성 요소를 구현합니다

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure 구독의 이름을 지정하는 변수를 만듭니다.

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 허브 가상 네트워크를 포함할 리소스 그룹의 이름을 지정하는 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_HUB_VNET='AADesignLab08-hub-vnet-rg'
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다(자리 표시자인 '<Azure region>'를 이 랩에서 리소스를 배포하려는 Azure 영역의 이름으로 대체합니다):

    ```sh
    LOCATION='<Azure region>'
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure Building Blocks를 사용함으로써 Hub-and-Spoke 토폴로지의 허브 구성 요소를 배포합니다.

    ```sh
    azbb -g $RESOURCE_GROUP_HUB_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/hub-vnet.json --deploy
    ```

1. 배포가 완료되기까지 기다리지 말고 다음 작업으로 진행하십시오.


#### 작업 5: Hub 및 Spoke 설계 스포크 구성 요소를 구현합니다

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열려 있는 브라우저 창에서 **Azure Portal** (<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩의 앞에서 사용한 것과 동일한 사용자 계정으로 인증합니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure 구독의 이름을 지정하는 변수를 만듭니다.

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 첫 번째 스포크 가상 네트워크를 포함할 리소스 그룹의 이름을 지정하는 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_SPOKE1_VNET='AADesignLab08-spoke1-vnet-rg'
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다.

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure Building Blocks를 사용함으로써 Hub-and-Spoke 토폴로지의 첫 번째 스포크 구성 요소를 배포합니다.

    ```sh
    azbb -g $RESOURCE_GROUP_SPOKE1_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/spoke1.json --deploy
    ```

1. 배포가 완료되기까지 기다리지 말고 다음 단계로 진행하십시오.

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열려 있는 브라우저 창에서 **Azure Portal** (<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩의 앞에서 사용한 것과 동일한 사용자 계정으로 인증합니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure 구독의 이름을 지정하는 변수를 만듭니다.

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 두 번째 스포크 가상 네트워크를 포함할 리소스 그룹의 이름을 지정하는 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_SPOKE2_VNET='AADesignLab08-spoke2-vnet-rg'
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다.

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure Building Blocks를 사용함으로써 Hub-and-Spoke 토폴로지의 두 번째 스포크 구성 요소를 배포합니다.

    ```sh
    azbb -g $RESOURCE_GROUP_SPOKE2_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/spoke2.json --deploy
    ```

1. 배포가 완료되기까지 기다리지 말고 다음 작업으로 진행하십시오.

#### 작업 6: Hub 및 Spoke 설계 VNet 피어링을 구성합니다.

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열려 있는 브라우저 창에서 **Azure Portal** (<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩의 앞에서 사용한 것과 동일한 사용자 계정으로 인증합니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure 구독의 이름을 지정하는 변수를 만듭니다.

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 허브 가상 네트워크를 포함하는 리소스 그룹의 이름을 지정하는 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_HUB_VNET='AADesignLab08-hub-vnet-rg'
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다.

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure Building Blocks를 사용하여 Hub-and-Spoke 토폴로지의 가상 네트워크의 피어링을 프로비저닝합니다.

    ```sh
    azbb -g $RESOURCE_GROUP_HUB_VNET -s $SUBSCRIPTION_ID -l $LOCATION -p ~/hub-vnet-peering.json --deploy
    ```

1. 배포가 완료되기까지 기다리지 말고 다음 작업으로 진행하십시오.


#### 작업 7: Hub 및 Spoke 설계 라우팅을 구현합니다.

1. 작업 표시줄에서 **Microsoft Edge** 아이콘을 클릭합니다.

1. 열려 있는 브라우저 창에서 **Azure Portal** (<https://portal.azure.com>)로 이동합니다.

1. 메시지가 표시되면 이 랩의 앞에서 사용한 것과 동일한 사용자 계정으로 인증합니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 새 셸 인스턴스를 엽니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure 구독의 이름을 지정하는 변수를 만듭니다.

    ```sh
    SUBSCRIPTION_ID=$(az account list --query "[0].id" | tr -d '"')
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 라우터 기능을 하는 허브 Network Virtual Appliance (NVA)를 포함할 리소스 그룹의 이름을 지정하는 값의 변수를 만듭니다.

    ```sh
    RESOURCE_GROUP_HUB_NVA='AADesignLab08-hub-nva-rg'
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 배포에 사용할 Azure 영역을 지정하는 변수를 만듭니다.

    ```sh
    LOCATION=$(az group list --query "[?name == 'AADesignLab08-hub-vnet-rg'].location" --output tsv)
    ```

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 Azure Building Blocks를 사용함으로써 Hub-and-Spoke 토폴로지의 NVA 구성 요소를 배포합니다.

    ```sh
    azbb -g $RESOURCE_GROUP_HUB_NVA -s $SUBSCRIPTION_ID -l $LOCATION -p ~/hub-nva.json --deploy
    ```

1. 다음 작업을 진행하기 전에 배포가 완료될 때까지 기다립니다.

    > **참고**: 배포에는 약 10분 정도 걸릴 수 있습니다.


## 실습 2: Hub-spoke 토폴로지 검토

#### 작업 1: 피어링 구성 검사합니다.

1. Azure Portal의 허브 메뉴에서 **모든 서비스**를 클릭합니다.

1. **필터** 텍스트 상자의 **모든 서비스** 메뉴에서 **가상 네트워크**를 입력하고 **엔터**를 누릅니다.

1. 결과 목록에서 **가상 네트워크**를 클릭합니다.

1. **가상 네트워크** 블레이드에서 **hub-vnet**을 클릭합니다.

1. **hub-vnet** 블레이드에서 **피어링**을 클릭합니다.

1. **hub-vnet - 피어링** 블레이드에서 피어링 목록과 해당 상태를 검토합니다.

1. 다시 **가상 네트워크** 블레이드로 다시 이동하고 **spoke1-vnet**를 클릭합니다.

1. **spoke1-vnet** 블레이드에서 **피어링**을 클릭합니다.

1. **spoke1-vnet - 피어링** 블레이드에서 기존 피어링 및 해당 상태를 검토합니다.

1. 다시 **가상 네트워크** 블레이드로 이동하고 **spoke2-vnet**를 클릭합니다.

1. **spoke2-vnet** 블레이드에서 **피어링**을 클릭합니다.

1. **spoke2-vnet - 피어링** 블레이드에서 기존 피어링 및 해당 상태를 검토합니다.

#### 작업 2: 라우팅 구성 검사합니다

1. **필터** 텍스트 상자의 **모든 서비스** 메뉴에 **라우팅 테이블**을 입력하고 **엔터**를 누릅니다.

1. 결과 목록에서 **라우팅 테이블**을 클릭합니다.

1. **라우팅 테이블** 블레이드에서 **spoke1-rt**를 클릭합니다.

1. **spoke1-rt** 블레이드에서 경로 목록을 검토합니다. **toSpoke2** 경로에 대한 **NEXT HOP** 항목에 유의하십시오. 

1. **라우팅 테이블** 블레이드로 돌아가서 **spoke2-rt**를 클릭합니다.

1. **spoke2-rt** 블레이드에서 경로 목록을 검토합니다. **toSpoke1** 경로에 대한 **NEXT HOP** 항목에 유의하십시오. 

#### 작업 3: 스포크 사이의 연결을 확인합니다

1. Azure Portal의 허브 메뉴에서 **모든 서비스**를 클릭합니다.

1. **필터** 텍스트 상자의 **모든 서비스** 메뉴에서 **네트워크 감시기**를 입력하고 **엔터**를 누릅니다.

1. 결과 목록에서 **네트워크 감시기**를 클릭합니다.

1. **네트워크 진단 도구** 섹션의 **네트워크 감시기** 블레이드에서 **연결 문제 해결**을 클릭합니다.

1. **네트워크 감시기 - 연결 문제 해결** 블레이드에서 다음 작업을 수행합니다.

    - **구독** 드롭다운 목록 항목을 기본값 설정으로 유지합니다.

    - **리소스 그룹** 드롭다운 목록에서 **AADesignLab08-spoke1-vnet-rg** 항목을 선택합니다.

    - **가상 컴퓨터** 드롭다운 목록에서 기본 항목을 그대로 둡니다.

    - **포트** 텍스트 상자를 비워 둡니다. 

    - **대상** 옵션이 **수동 지정**으로 설정되어 있는지 확인합니다.

    - **URI, FQDN 또는 IPv4** 텍스트 상자에 **10.2.0.68** 항목을 입력합니다.

    - **포트** 텍스트에 3389를 입력합니다. 

    - **확인** 버튼을 클릭합니다.

1. W연결 확인 결과가 반환될 때까지 기다렸다가 상태가 **도달 가능**인지 확인합니다.

    > **참고**: 네트워크 감시기를 처음 사용하는 경우, 검사는 최대 5분 정도 걸릴 수 있습니다.


## 실습 3: 랩 리소스 제거

#### 작업 1: Cloud Shell 엽니다.

1. 포털 상단에서 **Cloud Shell** 아이콘을 클릭하여 Cloud Shell 창을 엽니다.

1. **Cloud Shell** 명령 프롬프트에 다음 명령을 입력하고 **엔터**를 눌러 이 랩에서 생성한 모든 리소스 그룹을 나열합니다.

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab08')]".name --output tsv
    ```

1. 출력에 이 랩에서 만든 리소스 그룹만 포함되어 있는지 확인합니다. 이러한 그룹은 다음 작업에서 삭제됩니다.

#### 작업 2: 리소스 그룹 삭제합니다.

1. **Cloud Shell** 명령 프롬프트에서 다음 명령을 입력하고 **엔터**를 눌러 이 랩에서 생성한 리소스 그룹을 삭제합니다

    ```sh
    az group list --query "[?starts_with(name,'AADesignLab08')]".name --output tsv | xargs -L1 bash -c 'az group delete --name $0 --no-wait --yes'
    ```

1. 포털 하단에 있는 **Cloud Shell** 프롬프트를 닫습니다.

> **검토**: 이 실습에서는 이 랩에서 사용된 리소스를 제거했습니다.
