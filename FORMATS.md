# TWSE Message Format Naming Conventions

This file lists the defined TWSE message formats based on the Operation Manual (B.12.11) and the corresponding naming conventions used within this project's codebase.

**Naming Rules:**

*   **`UnifiedEvent` Enum Variant:** `FormatNN` + `DescriptiveName` (PascalCase)
*   **Payload Struct:** `DescriptiveName` + `FmtNN` (PascalCase)

## Defined Formats

| Format Code | Description                                                     | Enum Variant (`UnifiedEvent::`)         | Struct Name                           |
|-------------|-----------------------------------------------------------------|-----------------------------------------|---------------------------------------|
| 01          | Basic Data of Individual Common Stocks on TWSE Market           | `Format01BasicStockData`                | `BasicStockDataFmt01`                 |
| 02          | Statistics of Auction Trading of Common Stocks on TWSE Market   | `Format02AuctionTradingStats`           | `AuctionTradingStatsFmt02`            |
| 04          | Statistics on Auction Consignments of Common Stocks on TWSE Market| `Format04AuctionConsignmentStats`       | `AuctionConsignmentStatsFmt04`        |
| 05          | Stock Market Announcements                                      | `Format05MarketAnnouncement`            | `MarketAnnouncementFmt05`             |
| 06          | Real-time Auction Quotes of Common Stocks on TWSE Market        | `Format06RealtimeAuctionQuoteStock`     | `RealtimeAuctionQuoteStockFmt06`      |
| 07          | Statistics on Finished Fixed-price Transactions of Common Stocks| `Format07FixedPriceTradeStats`          | `FixedPriceTradeStatsFmt07`           |
| 08          | Statistics on Fixed-price Transaction Consignments of Common Stocks| `Format08FixedPriceConsignmentStats`    | `FixedPriceConsignmentStatsFmt08`     |
| 09          | Trading Information of Individual Common Stocks in Fixed-price  | `Format09FixedPriceTradeInfoStock`    | `FixedPriceTradeInfoStockFmt09`       |
| 10          | Updated Information of TWSE-complied Indices                  | `Format10IndexUpdate`                   | `IndexUpdateFmt10`                    |
| 12          | Information of Auction Opening (Closing) Price of Common Stocks | `Format12AuctionOpenClosePriceStock`  | `AuctionOpenClosePriceStockFmt12`   |
| 13          | Real-time Odd Lot Transaction Information on Stock Market       | `Format13RealtimeOddLotTrade`           | `RealtimeOddLotTradeFmt13`            |
| 14          | Full-name Information of Callable Bull(Bear) Contracts          | `Format14CbbcFullName`                  | `CbbcFullNameFmt14`                   |
| 15          | Information of Suspended Stocks on the Stock Market             | `Format15SuspendedStockInfo`            | `SuspendedStockInfoFmt15`             |
| 16          | Quote Transmission System t Data on Stock Market                | `Format16QuoteTransmissionSystemT`    | `QuoteTransmissionSystemTFmt16`     |
| 17          | Real-time Auction Quotes of call (put) warrant on TWSE Market   | `Format17RealtimeAuctionQuoteWarrant`   | `RealtimeAuctionQuoteWarrantFmt17`    |
| 18          | Information of Auction Opening (Closing) Price of call (put) warrants | `Format18AuctionOpenClosePriceWarrant`| `AuctionOpenClosePriceWarrantFmt18` |
| 19          | Data on Stocks Suspended/Resumed for trading in TWSE          | `Format19SuspendedResumedStock`         | `SuspendedResumedStockFmt19`          |
| 20          | Snapshot Data on Stocks for continuous trading in TWSE        | `Format20SnapshotStockContinuous`       | `SnapshotStockContinuousFmt20`        |
| 21          | Realtime Index Definition of TWSE                             | `Format21RealtimeIndexDefinition`       | `RealtimeIndexDefinitionFmt21`        |
| 22          | Basic Data of Intraday odd lot trading Stocks                 | `Format22BasicDataOddLot`               | `BasicDataOddLotFmt22`                |
| 23          | Real-time Market Data of Intraday odd lot trading stock         | `Format23RealtimeMarketDataOddLot`    | `RealtimeMarketDataOddLotFmt23`     |
| 24          | Snapshot Data on call (put) warrant for continuous trading    | `Format24SnapshotWarrantContinuous`     | `SnapshotWarrantContinuousFmt24`      |
| 25          | Available Balance for Securities borrowing and lending          | `Format25SecuritiesLendingBalance`    | `SecuritiesLendingBalanceFmt25`     |

*Note: Format numbers 3 and 11 appear to be missing from the provided Table of Contents.*
