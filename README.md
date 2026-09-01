# cloud-itonami-carrier

Starlink Direct-to-Cell を前提にした携帯キャリアサービスの**運用** repo
(cloud-itonami / itonami.cloud)。ADR-2609011500 が正本。

## この repo が担うもの

- 加入者の申し込み / 解約 / 請求 / サポートを agent 群(governor ⊣ advisor)で運用する
- 通信状態の観測と障害時の顧客連絡
- 料金の決済は network-awai/nexus-x402(x402/USDC)に委譲

## この repo が担わないもの

- **eSIM プロファイルのライフサイクル** → cloud-itonami/cloud-itonami-esim
  (eSIM Provisioning Advisor ⊣ Governor、プロビジョニングの authority)。
  本 repo は esim actor を呼ぶ側で、実装を作り直さない
- **人間の作業環境** → cloud-itonami/cloud-itonami-app(安全第一の AI workspace)
- **spectrum・カバレッジの真実** → wholesale 相手と Starlink の公開 coverage map。
  自前の coverage 計算は作らない(作った瞬間に嘘になる)

## 設計の前提

- 自前 spectrum なし。Starlink Direct-to-Cell を wholesale 接続として使う構成
- SIM-swap 等の不正経路は esim actor が構造的に拒否(ownership/transfer は
  全 phase の :auto から恒久欠落)。本 repo はこの拒否を上書きしない
- agent 運営事業者として、人的対応は cloud-itonami-app の
  human-work-assurance 市場(ADR-0084)に外注する

## 段階

| 段階 | 内容 | 対外接続 |
|---|---|---|
| R0 | blueprint + governor/advisor actor 骨格 + test | なし |
| R1 | coverage/report 自動化 | wholesale 相手の実在確認後 |
| R2 | 加入フロー end-to-end(eSIM → 支払い → 開通) | wholesale 契約が前提 |

## 規律

- main 直 push なし。1 task = 1 branch = 1 PR
- 全数値は実測か公開 commit の引用。modelled 値は明示ラベル付き
- credential は commit しない(鍵は kagi vault / Keychain)

## License

AGPL-3.0-or-later
