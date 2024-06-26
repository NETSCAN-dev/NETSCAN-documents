---
#### Windows10 の自動再起動を抑制する方法
---

+ ユーザーがログオンしている限り自動再起動させない
> gpedit.msc を実行してグループポリシーエディタを起動し、  
> [コンピューターの構成] -> [管理用テンプレート] -> [Windows コンポーネント] -> [Windows Update] で下記２項目を実行する。  
> - **[スケジュールされた自動更新のインストールで、ログオンしているユーザーがいる場合には自動的に再起動しない ]** を"有効"にする。  
> - **[自動更新を構成する]** を”有効”とし、オプションで **[4–自動ダウンロードしインストール日時を指定]** に指定する。  
>  
>> [スケジュールされた自動更新のインストールで、ログオンしているユーザーがいる場合には自動的に再起動しない] の設定は、  
>> 必ず [4–自動ダウンロードしインストール日時を指定] と合わせて設定をしないと効果が無いので注意。  
>> https://blogs.technet.microsoft.com/jpwsus/2018/02/08/manage-reboot/  
>>
