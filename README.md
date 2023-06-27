# AzureFirewallRule-Structured-ForSentinel
> 目的
本レポジトリは Microsoft Sentinel で提供されている Azure Firewall のテンプレートルール (Analytics Rule) を　Structured 形式に対応した形式に書き換えた分析ルールサンプルを提供しています。

- AzureFirewall 構造型ファイアウォールログとは
 - 構造化ログ (Structured Log) は、特定の形式で構成されたログデータの一種です。これまでの AzureFirewall ログは AzureDiagnostic テーブルに含まれていたため、5-tuples (送受信 IP, ポート, プロトコル) などの情報は AzureDiagnostic テーブルのカラムをユーザー側で正規化して抽出する作業が必要でした。<BR>
 - Azure Firewall の構造化ログでは、ネットワークルール、アプリケーションルール専用に Log Analytics ワークスペースのテーブルが作成され、標準で送信/宛先の IP アドレス、プロトコル、ポート番号、ファイアウォールによって実行されたアクションなどの情報が含まれます。また、イベントの時刻や Azure Firewall インスタンスの名前など、さらに多くのメタデータも含まれます。ハ
 - Microsoft Sentinel には AzureFirewall 向けのテンプレートルールが提供されていますが、2023.7.1 現在 Structured Log 形式のルールが提供されていません。本レポジトリは有志で　Structured Log 向けのテンプレートルールを変換しています。
<img width="872" alt="image" src="https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/assets/55295601/cb247e61-713d-4b1d-8abc-a4ad185763aa">

# テンプレートルールリスト
> Microsoft Sentinel で提供されているテンプレートルールのマッピング表です

|  テンプレートルール名  |  Status  | リンク |
| ---- | ---- | ---- |
| TI map IP entity to AzureFirewall | TBD | - |
| Several deny actions registered | TBD | - |
| Multiple Sources Affected by the Same TI Destination | TBD | - |
| Port Sweep | TBD | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/PortSweep.json) |
| Abnormal Deny Rate for Source IP | Done | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/AbnormalDenyRateforSourceIP.json) |
| Abnormal Port to Protocol | Done | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/AbnormalPortProtocol.json) |
| Port Scan | Done | [json](https://github.com/hisashin0728/AzureFirewallRule-Structured-ForSentinel/blob/main/PortScan.json) |
