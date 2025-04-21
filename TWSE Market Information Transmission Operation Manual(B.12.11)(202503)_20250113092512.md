# Taiwan Stock Exchange

## Market Information Transmission

## Operation Manual

## (IP Feed Specification)

```
Version: B.12. 11
Document No.: O- 127 - A
Produced by the Taiwan Stock Exchange
```

## Table of Contents


- 1. Introduction
- 2. Connection Architecture
- 3. Data Format
   - Format 1 Basic Data of Individual Common Stocks on TWSE Market
   - Format 2 Statistics of Auction Trading of Common Stocks on TWSE Market
   - Format 4 Statistics on Auction Consignments of Common Stocks on TWSE Market
   - Format 5 Stock Market Announcements
   - Format 6 Real-time Auction Quotes of Common Stocks on TWSE Market
   - Format 7 Statistics on Finished Fixed-price Transactions of Common on TWSE Markets
   - Format 8 Statistics on Fixed-price Transaction Consignments of Common Stocks on TWSE Market
      - Market Format 9 Trading Information of Individual Common Stocks in Fixed-price Transactions on TWSE
   - Format 10 Updated Information of TWSE-complied Indices
   - Format 12 Information of Auction Opening (Closing) Price of Common Stocks on TWSE
   - Format 13 Real-time Odd Lot Transaction Information on Stock Market
   - Format 14 Full-name Information of Callable Bull(Bear) Contracts on the Stock Market
   - Format 15 Information of Suspended Stocks on the Stock Market
   - Format 16 Quote Transmission System t Data on Stock Market
   - Format 17 Real-time Auction Quotes of call (put) warrant on TWSE Market.............................................
   - Format 18 Information of Auction Opening (Closing) Price of call (put) warrants on TWSE Market
   - Format 19 Data on Stocks Suspended/Resumed for trading in TWSE
   - Format 20 Snapshot Data on Stocks for continuous trading in TWSE
   - Format 21 Realtime Index Definition of TWSE
   - Format 22 Basic Data of Intraday odd lot trading Stocks
   - Format 23 Real-time Market Data of Intraday odd lot trading stock
   - Format 24 Snapshot Data on call (put) warrant for continuous trading in TWSE
   - Format 25 Available Balance for Securities borrowing and lending
- 4. Version Update Log
- Appendix A: Description of Transmission Sequence................................................
- Appendix B: Stock coding rules
- Appendix C: Stock Industry Category (Cf Format 1 3.1.3)
- Appendix D: Stock Category Code Table (Cf Format 1 3.1.4)
- Appendix E: Table of Transmission Setting
- Appendix F: Table of Online Transmission Formats
- Appendix G: Trading Currency Code


Taiwan Stock Exchange

## 1. Introduction

1.1 Information Format Development History
1.1.1 The Taiwan Stock Exchange (TWSE) implemented four policies as of 1 July 2002 to promote market
internationalization and liberalization, to ensure fair and efficient trade, and to assure information
transparency. These policies included (1) integration of abolition of tick size restrictions and auction;
(2) stabilization of instantaneous intraday stock price; (3) 5-minute collective auction at closing; and
(4) disclosure of the one unexecuted stock with the highest buying and the lowest bids and their
volumes. Further in 2004, the TWSE implemented the policy of disclosure of the five unexecuted
stocks with the highest buying and the lowest bids and their volumes. Information transmission lag
occurred as a result because the baud rate at 9.6Kbps of the data transmission circuits of market
information companies (securities companies) was unable to handle the augmentation of data volume
from the auction system adjustment of the TWSE.
1.1.2 In consideration of the data transmission baud rate, future expandability and the requirements of the
new-generation system architecture and with reference to the recommendations from information
companies, the TWSE decided to modify the protocol and baud rate of data transmission circuits for
information companies as solutions for the information lag problem. In addition to enhancing quote
data transmission by adopting the TCP/IP protocol and by upgrading the baud rate to 512Kbps, the
TWSE reviewed the content of data transmission formats in order to enhance information
transmission volume and data processing efficiency.
1.1.3 In the data transmission format review, relevant fields were either simplified or combined and field
expandability was reserved to meet the future business development needs in order to enhance
program maintainability.

1.2 Modifying transmission data for high-speed quote transmission architecture and protocol
The bandwidth will be extensively increased after using the TCP/IP protocol and upgrading baud rate to
512Kbps. In condition of future system planning and architecture and data item expandability, the HEADER
field is added to the formats of quote data transmission based on the TCP/IP protocol in order to enhance the
processing efficiency of information companies after receiving quote data transmitted from the TWSE.
1.2.1 The information field length is increased for the receiving programs of information companies to
justify packet length.
1.2.2 Shared quote transmission circuits are planned to transmit quote information of different businesses
and data sources are remarked and identified by business to reduce the hardware replacement cost
and to enhance circuit use efficiency of information companies.
1.2.3 The field for transmission format codes is moved to the HEADER field.
1.2.4 Remarks for transmission format versions are increased to facilitate the receiving programs of
information companies to justify old or new data by version in future transmission format update
tests.
1.2.5 The transmission serial number field is added for receiving programs to check if data received are
complete because the telecast function of TCP/IP has no guaranteed delivery mechanisms.


Taiwan Stock Exchange

## 2. Connection Architecture

2.1 Connection requirements
2.1.1 Routers/switch routers equipped with IGMP feature to connect to the IP Data Transmission Network.
2.1.2 The TCP/IP shall be used as the protocol for all transmission circuits.
2.1.3 Each channel’s market data is published in duplicate on two separate multicast addresses.

2.2 System Architecture Chart

## TWSE Data Center

**Securities Companies / Information Companies**

**Transmission Path**

**IP Network**

```
Leased
Line
```
```
Leased
Line
```

Taiwan Stock Exchange

## 3. Data Format

3.1 Transmission Data Compression
Data are compressed with PACK BCD before transmission.

```
Description of PACK BCD
The last 4 bits of a byte from numbers “0” - “9” in ASCII code are transmitted. For example, the bit
value of “1” in ASCII code is 00110001, so its PACK BCD bit value is 0001; and the bit value of “5”
in ASCII code is 00110101, so its PACK BCD bit value is 0101.
Example of trading price displayed in Format 6:
Trading price displayed data format is 9(0 5 )V9(04) (including 4 integers and 2 decimals)
Trading price = 12345 .6 789
Original data format transmitted expressed in ASCII:” 1 ” “ 2 ” “ 3 ” “ 4 ” “ 5 ” “ 6 ” “ 7 ” “ 8 ” “ 9 ”
(It needs to transmit 9 bytes) in hexagonal expressions:
0x31 0x32 0x33 0x34 0x35 0x36 0x37 0x38 0x
(Totally 9 bytes) (the recipient will decipher the codes into integers and decimals)
Compressed with PACK BCD = 0x01 0x23 0x45 0x67 0x
(PACK BCD expression)
(It needs to transmit only 5 bytes): 0000 0001 0010 0011 0100 0101 0110 0111 1000 1001
(Totally 5 bytes) (the recipient will decipher the codes into integers and decimals)
```
3.2 Transmission Sequence

```
Please refer to Appendix A for details of the transmission sessions and intervals of different
data formats.
```
3.3 Data Formats


Taiwan Stock Exchange

### Format 1 Basic Data of Individual Common Stocks on TWSE Market

## TWSE Data Transmission Format

Format 1 : Basic Data of Individual Stocks on TWSE Page: _1_^

Length (RL): Fixed length 114 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “ 0114 ”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2 .3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “01”
2.4 Transmission Format
Version 9(02)^1 6 -^6 PACK BCD^ “0^9 ”^
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD

3 BODY (^101 11) - 111
3.1 (^) General Stock Data (^54 11) - 64
3.1.1 Stock Code X(06) 6 11 - 16 ASCII
3.1.2 Stock Abbreviation in
Chinese

#### X(06) 16 17 - 32 ASCII

3.1.3 Industry Category X(02) (^2 33) - 34 ASCII
3.1.4 Stock Category X(02) (^2 35) - 36 ASCII
3.1.5 Stock Entries X(02) (^2 37) - 38 ASCII
3.1.6 Stock Anomaly Code 9(02) (^1 39) - 39 PACK BCD
3 .1.7 Board Code X(01) (^1 40) - 40 ASCII ‘ 0 ’/’ 3 ’
3.1.8 Today Reference Price 9(5)V9(4) (^5 41) - 45 PACK BCD
3.1.9 Rise Stop Price 9(5)V9(4)^5 46 - 50 PACK BCD
3.1.10 Fall Stop Price 9(5)V9(4)^5 51 - 55 PACK BCD
3.1.
Non-$10 face value
indicator
X(01) (^1 56) - 56 ASCII
3 .1.
Abnormal
recommendation indicator
X(01) (^1 57) - 57 ASCII

#### 3.1.

```
Abnormal securities
indicator X(01)^1 58 -^58 ASCII^
3.1.14 Day Trading Indicator X(01) 1 59 - 59 ASCII
```
#### 3.1.

```
Exemption of Unchanged
Market Margin Sale
Indicator
```
#### X(01) 1

#### 60 - 60

#### ASCII

#### 3.1.

```
Exemption of Unchanged
Market Securities Lending
Sale Indicator
```
#### X(01) 1

#### 61 - 61

#### ASCII

```
3.1.17 Matching Cycle Seconds 9(06) 3 62 - 64 PACK BCD
```
3.2 Warrant (CBBC)Data (^39 65) - 103
3.2.1 Warrant(CBBC) ID (^) X(01) (^1 65) - 65 ASCII
3.2.2 Exercise (Strike) price 9(6)V9(4) (^5 66) - 70 PACK BCD
3.2.3 Previous Business Day
Exercise Volume 9(10)^
(^5 71) - 75 PACK BCD


Taiwan Stock Exchange

```
3.2.4 Previous Business Day
Cancellation Volume
```
#### 9(10) 5 76 -^80 PACK BCD

3.2.5 Issuing Balance (Volume) (^) 9(10) (^5 81) - 85 PACK BCD
3.2.6 Strike Ratio (^) 9(06)V99 (^4 86) - 89 PACK BCD
3.2.7 Upper Limit Price 9(6)V9(4) 5 90 - 94 PACK BCD
3.2.8 Lower Limit Price 9(6)V9(4) 5 95 - 99 PACK BCD
3.2.9 Maturity Date 9(8) 4 100 - 103 PACK BCD
3.3 Foreign Stock
Information 7 104 -^110
3.3.1 Foreign Stock ID (^) X(01) (^1 104) - 104 ASCII
3.3.2 Trading Unit (^) 9(05) (^3 105) - 107 PACK BCD
3.3.3 Trading Currency Code (^) X(03) (^3 108) - 110 ASCII
3.4 Note on market information
line 9 (02)^1 111 -^111 PACK BCD^ 01/^
4 Checksum (^) X(01) (^1 112) - 112 XOR VALUE
5 TERMINAL-CODE (^) X(02) (^2 113) - 114 (HEXACODE：0D 0A)


Taiwan Stock Exchange

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE

2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD
“01” (length: 1 byte).

2.3 Transmission Format Code: Expresses Format 1 in PACK BCD “01” (length: 1 byte).

2.4 Transmission Format Version
(1) Expressed in PACK BCD “ 09 ”, where “0 9 ” means version 9 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.

2.5 Transmission S/N

```
(1) Expresses Transmission S/N in PACK BCD (length: 4 bytes).
(2) The basic data of common stocks on stock market are transmitted repeatedly with Format
1 before the market opens; and the basic data of new common stocks are transmitted
repeatedly with Format 1 after the market opens. All cycles are numbered from 1.
```
3 BODY: Records general stock data, warrant(CBBC) data and foreign stock data, with a total
of 101 bytes.

3.1 General Stock Data: totally 54 bytes.

3.1.1 Stock Code: Expressed in ASCII codes, totally 6 bytes. Please refer to Appendix B for

details of the Stock coding rules.

3.1.2 Stock Abbreviation: Expressed in ASCII 16 BYTEs.

3.1.3 Industry Category: Expressed in ASCII codes, totally 2 bytes.
(1) As the industry category of a stock can no longer be identified directly from the first two
digits of a stock code in the new stock coding rules, please justify the industry category
of industry-specific common stocks and preferred stocks from this field (see Appendix C
for details of stock industry code).
(2) The said preferred stocks include preferred stocks, preferred stocks with
warrants(CBBCs), stock payment certificate with warrants(CBBCs) and conversion
certificates. Please note that this field shows only the industry category code of
respective stocks, and users should determine the category of stocks, e.g. preferred stock,
preferred stock with warrants(CBBCs) etc, with reference to the original stock coding
rules (Appendix B).
(3) Non-industry-specific stocks, such as beneficiary certificates (closed-end funds),
warrants(CBBCs), depository receipts, foreign stocks, exercised bonds with warrants,
convertible bonds and corporate bonds with warrants, are expressed in “00” in this field.
Please justify stock of such category based on the original stock coding rules (Appendix
B).
(4) Government bonds are displayed as usual; the value expressed in this field is the first
and fourth codes of the stock code (see Item 1, Stock Industry Code Category Code,


Taiwan Stock Exchange

Appendix C, for details).
3.1.4 Stock Category: Expressed in ASCII codes, totally 2 bytes. As some stocks contain special

```
meanings (e.g. proportionally and un-proportionally issued Callable Bull(Bear) Contracts,
securities stock of domestic listed companies, bank stocks of domestic listed companies, and
stocks of foreign companies listed in Taiwan) that are unable to identified directly from the
stock code, information contained in this filed facilitates users to identify the special
meanings of such stocks (see Stock Category Code Table, Appendix D, for details).
```
3.1.5 Stock Entries

```
(1) Presented by ASCII 2 BYTE for identifying the data in the field of “Stock Code”..
(2) If the field of Stock Entries shows SPACES, the definition of “Stock Code” field
remained unchanged.
(3) Before the trading starts, the last entry of all basic information on individual stocks
being transmitted in cyclical sequence will be marked by “AL” and the “Stock Code”
field will be presented by “Total entries of Stocks”.
(4) The data transmitted in cyclical sequence during trading hours will include the stocks
added to the listing of TWSE on the same day and the last entry will be marked by “NE”
in the field of “Stock Entries” while the field of “Stock Code” will be marked as “total
new entries of stocks”.
```
3.1.6 Stock Anomaly Code

```
(1) Expressed in PACK BCD (length: 1 byte)
00 - Normal
01 - Attention
02 - Disposition
03 - Attention and Disposition
04 - Further Disposition
05 - Attention and Further Disposition
06 - Flexible Disposition
07 - Attention and Flexible Disposition
(2) See Articles 2, 3 and 6 for attention and disposition actions in the Taiwan Stock
Exchange Corporation Directions for Announcement or Notice of Attention to Trading
Information and Dispositions
```
3.1.7 Board Code: Presented in ASCII 1 BYTE; default value is “0”. A value of “3” denotes a

Taiwan Innovation Board security.

3.1.8 Today Reference Price

```
(1) Expressed in PACK BCD (length: 5 bytes)
(2) Government Bonds-0 (no rise and stop limits)
```
3.1.9 Rise Stop Price

```
(1) Expressed in PACK BCD (length: 5 bytes)
(2) Government Bonds-Last trading price
```
3.1.10 Fall Stop Price
(1) Expressed in PACK BCD (length: 5 bytes)
(2) Government Bonds-Last trading price

3.1.11 Non-$10 face value indicator: Presented in ASCII 1 BYTE; its values are either “Y” or

SPACE, with SPACE as the default. A value of “Y” denotes that the underlying stock was
issued at a face value other than $10.
3.1.12 Abnormal recommendation indicator: Presented in ASCII 1 BYTE; its values are either “Y”

or SPACE, with SPACE as the default. A value of “Y” indicates that the security is under the


Taiwan Stock Exchange

influence of abnormal recommendation on cable television.
3.1.13 Abnormal securities indicator: Presented in ASCII 1 BYTE; its values are either “Y” or

SPACE, with SPACE as the default. A value of “Y” denotes an abnormal security.

3.1.14 Day Trading Indicator: Presented with ASCII 1 BYTE, its value is "A", "B" or SPACE with

```
SPACE as the default. A value of "A" denotes Buy first then Sell, or Sell first then Buy Day
Trading Securities. A value of "B" denotes Buy first then Sell Day Trading Securities. A
value of SPACE denotes Non-Day Trading Securities.
```
3.1.15 Exemption of Unchanged Market Margin Sale Indicator: Presented with ASCII 1 BYTE, its

```
value is "Y" or SPACE with SPACE as the default. A value of "Y" denotes Unchanged
Market Margin Sale Securities.
```
3.1.16 Exemption of Unchanged Market Lending Sale Indicator: Presented with ASCII 1 BYTE, its
value is "Y" or SPACE with SPACE as the default. A value of "Y" denotes Unchanged
Market Lending Sale Securities.

3.1.17 Matching Cycle Seconds: 6 digits number presented with PACK BCD (Length: 3 BYTE),

records the matching cycle seconds of the individual stock using aggregate auction. A value
of "0" denotes the individual stock using continuous market method.
3.2 Warrant(CBBC) Data: Totally 39 bytes

3.2.1 Warrant(CBBC) ID: Expressed in ASCII code (1 byte); default value is space. A value of

```
“Y”, denotes the stock has warrant (CBBC) data, and users must read the Warrant (CBBC)
Data field in the BODY. If the value is “ ”, the warrant(CBBC) data are “0”.
```
3.2.2 Performance Price: Expressed in PACK BCD (lengths: 5 bytes) and informs the latest
performance price. In index certificates, this informs the latest performance index data.

3.2.3 Previous Business Day Exercise Volume: Expressed in PACK BCD (length: 5 BYTE) for

1000 every warrants(CBBC).

3.2.4 Previous Business Day Cancellation Volume: Expressed in PACK BCD (length: 5 BYTE)

for every 1000 warrants(CBBC).
3.2.5 Issued Balance (Volume): Expressed in PACK BCD (length: 5 BYTE) for every 1000

warrants(CBBC).

3.2.6 Exercise Rate: Expressed in PACK BCD (lengths: 4 bytes) and records the latest amount of

```
convertible shares per thousand units. For index certificates, this refers to the amount of
convertible index value per thousand units in certificate. For example, if the underlining of
the warrant(CBBC) is stock and the value in this field is “1000.00”, the exercise rate per unit
is 1. If the value is “300.00”, the exercise rate per unit is “0.3”. If the certificate target is
index and the value in this field is “1000.00”, the exercise rate per unit is 1. If the value is
“500.00”, the exercise rate per unit is “0.5”.
```
3.2.7 Upper Limit Price: Presented with PACK BCD (length: 5 BYTES), display the warrant upper

limit price.
3.2.8 Lower Limit Price: Presented with PACK BCD (length: 5 BYTES), display the warrant

lower limit price.

3.2.9 Maturity Date: Presented with PACK BCD (length: 4 BYTE), display the warrant maturity

date (Western Date).

3.3 Other Information: (length: 7 BYTE)

3.3.1 Foreign Stock ID: Expressed in ASCII code (1 byte); the record value in “Y” or SPACE and

default value is SPACE. If the record value is “Y”, this means the stock is a foreign stock.

3.3.2 Trading Unit: The amount of shares for every trading unit of a stock (the amount of warrants

```
for warrant and beneficiary certificates for beneficiary certificate) expressed in PACK BCD
(length: 3 bytes). The default value is 1000. If the record value is 1000, this means every
trading unit is 1000 shares. If it is 500, this means every trading unit is 500 shares.
```

Taiwan Stock Exchange

3.3.3 Trading Currency Code: The code of currency that is used by a particular stock expressed in
ASCII code (3 bytes). The default is SPACES. If the record value is “ “, this means the
currency is New Taiwan Dollar; or other currencies otherwise(please see Appendix 7 for
trading currency codes).

3.4 Note on market information line: in 2 digits, expressed by PACK BCD (length 1 BYTE). This

```
is for the identification of IP and format of the information on individuals’ stocks of the day
during trading hours.
```
The value of the field and definitions are shown in the table below:

(^)
Note on market
information line
IP and corresponding format of real time information on individual stock
during trading hours
Transmission line Data format Format of price at
opening (close)
01 1 st IP Format 6 Format 12
02 2 nd^ IP^ Format 17^ Format 18^
The format for upload market information via the 1st and 2nd IP is shown in Appendix F.
4 Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5 TERMINAL-CODE: The ending byte of every record, default is HEXACODE:0D 0A.


Taiwan Stock Exchange

### Format 2 Statistics of Auction Trading of Common Stocks on TWSE Market

## TWSE Data Transmission Format

Format 2：Statistics of Auction Trading of Common Stocks

on TWSE Market

Page: _1_

Length (RL): Fixed length 142 Bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “ 0142 ”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “02”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “ 03 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 129 11 - 139
3.1 Statistics Time 9(06) 3 11 - 13 PACK BCD
3.2 Total Trading Amount 9(15) 8 14 - 21 PACK BCD

```
3.3 Trading Volume 9(15)^8 22 -^29 PACK BCD^
3.4 Trading Records 9(10) 5 30 - 34 PACK BCD
```
```
3.5 Total fund transaction value^ 9(15)^8 35 -^42 PACK BCD^
```
```
3.6 Fund transaction volume^ 9(15)^8 43 -^50 PACK BCD^
```
```
3.7 Fund transaction records^ 9(10)^5 51 -^55 PACK BCD^
```
```
3.8 Stock transaction value^ 9(15)^8 56 -^63 PACK BCD^
```
```
3.9 Stock transaction volume^ 9(15)^8 64 -^71 PACK BCD^
```
```
3.10 Stock transaction^ records^ 9(10)^5 72 -^76 PACK BCD^
```
```
3.
```
```
Callable Bull Contract transaction
value 9(15)^8 77 -^84 PACK BCD^
```
```
3.
```
```
Callable Bull Contract transaction
volume
```
#### 9(15) 8 85 - 92 PACK BCD

#### 3.

```
Callable Bull Contract transaction
records
```
#### 9(10) 5 93 - 97 PACK BCD

#### 3.

```
Callable Bear Contract transaction
value
```
#### 9(15) 8 98 - 105 PACK BCD

#### 3.

```
Callable Bear Contract transaction
volume 9(15)^8 106 -^113 PACK BCD^
```
```
3.
```
```
Callable Bear Contract transaction
records
```
#### 9(10) 5 114 - 118 PACK BCD

#### 3.

```
Taiwan Innovation Board
transaction value
```
#### 9(15) 8 119 - 126 PACK BCD

#### 3.

```
Taiwan Innovation Board
transaction volume
```
#### 9(15) 8 127 - 134 PACK BCD

#### 3.

Taiwan Innovation Board
transaction records 9(10)^5 135 -^139 PACK BCD^
4 Checksum X(01) 1 140 - 140 XOR VALUE

(^5) TERMINAL-CODE X(02) 2 141 - 142 (HEXACODE: 0D 0A)
Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission


Taiwan Stock Exchange

formats.
2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”
(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 2 in PACK BCD “02” (length: 1 byte).

2.4 Transmission Format Version

(1) Expresses version in PACK BCD, e.g. “ 03 ” for Version 3 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) Statistics data are transmitted once every 5 seconds, and the same statistics time has the
same serial number.
```
3. BODY: Totally 129 bytes.

3.1 Statistics Time

```
(1) Expressed in PACK BCD (length: 3 bytes) to record time in hour, minute, second format
(HHMMSS).
(2) Before the market opens, the system is displayed in the field, and the value in other
trading statistics fields is “0”.
(3) The value recorded in this field is the system time produced by the auction market trading
statistics of common stocks on the market. At present, statistics are produced every 5
seconds. When the value displayed in the field is “999999”, it means that data displayed
in the field are the last statistics, and they will be transmitted repeated for about 15
minutes.
```
3.2~3.16. Statistics on transactions

Note to information type:

3.2~3.4 Statistics on market-wide transactions:

```
The information on market-wide transactions includes all securities traded in the market,
and comprises total transaction value, volume and records. Refer to Appendix B for the
principle of the codification of stock.
```
3.5~3.7 Statistics on fund transactions:

```
The statistics on fund transactions shall include data on beneficiary certificates, ETF,
REAT, securitized financial assets, and REIT, and comprise total transaction volume,
value and records. Refer to Appendix B for the principle of the codification of stock.
```
3.8~3.10 Statistics on stock trade:

```
The information on the statistics of common stock (refer to common stock of Appendix B:
Stock coding rules, does not include Taiwan Innovation Board Securities.) and comprises
total transaction value, volume and records.
```
3.11~3.13 Statistics on Callable Bull Contract trade:


Taiwan Stock Exchange

Information on the statistics of Callable Bull Contract trade includes Callable Bull
Contracts with domestic securities or index as underlying assets and foreign securities or
index as underlying assets. The content of information comprises the total transaction
value, volume, and records. Refer to Appendix B for the principle of the codification of
securities.
3.14~3.16 Statistics on Callable Bear Contract:

The information on the statistics of Callable Bear Contracts includes Callable Bear
Contracts with domestic securities or index as underlying assets or foreign securities or
index as underlying assets. The content of the information includes the total transaction
value, volume and records. Refer to Appendix B for the principle of the codification of
securities.
3.17~3.1 9 Statistics on Taiwan Innovation Board:

```
The information on the statistics of Taiwan Innovation Board. The content of the
information includes the total transaction value, volume and records. Refer to Appendix B
for the principle of the codification of securities.
```
Notes to the attributes of fields:

```
3.2, 3.5, 3.8, 3.11, 3.14, 3.17 total transaction value:
Represented by PACK BCD with length of 8 BYTE tracking on total transaction value on
an accumulative basis.
```
```
3.3, 3.6, 3.9, 3.12, 3.15, 3.18 transaction volume:
Represented by PACK BCD, with length of 8 BYTE, tracking on the transaction volume
on an accumulative basis. The unit volume is a trading unit.
```
```
3.4, 3.7, 3.10, 3.13, 3.16, 3.19 counts of transactions:
Represented by PACK BCD with length of 5 BYTE tracking on the records of transaction
on an accumulative basis.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE:0D 0A.


Taiwan Stock Exchange

### Format 4 Statistics on Auction Consignments of Common Stocks on TWSE Market

## TWSE Data Transmission Format

Format 4 : Statistics on Auction Consignments of Common Stocks

on TWSE Market

Page: _1_

Length (RL): Fixed length 304 Byte
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “ 0304 ”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “04”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “ 03 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 291 11 - 301

```
3.1 Consignment Accumulated Time 9(06) 3 11 - 13 PACK BCD
```
```
3.2 Overall market consigned purchase
records
```
#### 9(08) 4 14 - 17 PACK BCD

```
3.3 Overall market consigned sales records 9(08) 4 18 - 21 PACK BCD
```
```
3.
Overall market consigned purchase
volume
```
#### 9(08) 4 22 - 25 PACK BCD

```
3.5 Overall market consigned sales volume 9(08) 4 26 - 29 PACK BCD
3.6 Fund consigned purchase records 9(08) 4 30 - 33 PACK BCD
3.7 Fund consigned sales records 9(08) 4 34 - 37 PACK BCD
3.8 Fund consigned purchase volume 9(08) 4 38 - 41 PACK BCD
3.9 Fund consigned sales volume 9(08) 4 42 - 45 PACK BCD
3.10 Stock consigned purchase records 9(08) 4 46 - 49 PACK BCD
3.11 Stock consigned sales records 9(08) 4 50 - 53 PACK BCD
3.12 Stock consigned purchase volume 9(08) 4 54 - 57 PACK BCD
3.13 Stock consigned sales volume 9(08) 4 58 - 61 PACK BCD
```
```
3.
Callable Bull Contract consigned
purchase records
```
#### 9(08) 4 62 - 65 PACK BCD

#### 3.

```
Callable Bull Contract consigned sales
records 9(08)^4 66 -^69 PACK BCD^
3.16 Callable Bull Contract^ consigned
purchase volume
```
#### 9(08) 4 70 - 73 PACK BCD

#### 3.

```
Callable Bull Contract consigned sales
volume
```
#### 9(08) 4 74 - 77 PACK BCD

#### 3.

```
Callable Bear Contract consigned
purchase records
```
#### 9(08) 4 78 - 81 PACK BCD

#### 3.

```
Callable Bear Contract consigned sales
records 9(08)^4 82 -^85 PACK BCD^
3.20 Callable Bear Contract^ consigned
purchase volume
```
#### 9(08) 4 86 - 89 PACK BCD

#### 3.

```
Callable Bear Contract consigned sales
volume
```
#### 9(08) 4 90 - 93 PACK BCD

#### 3.

```
Taiwan Innovation Board consigned
purchase records
```
9(08) (^4 94) - 97 PACK BCD
3.
Taiwan Innovation Board consigned sales
records 9(08)^4 98 -^101 PACK BCD^
3.24 Taiwan Innovation Board consigned
purchase volume
9(08) (^4 102) - 105 PACK BCD


Taiwan Stock Exchange

```
3.25 Taiwan Innovation Board consigned sales
volume
```
9(08) (^4 106) - 109 PACK BCD
3.
Overall market rise stop consigned
purchase records
9(08) (^4 110) - 113 PACK BCD
3.
Overall market rise stop consigned sales
records
9(08) (^4 114) - 117 PACK BCD
3.
Overall market rise stop consigned
purchase volume 9(08)^4 118 -^121 PACK BCD^
3.29 Overall market rise stop consigned sales
volume
9(08) (^4 122) - 125 PACK BCD
3.
Overall market fall stop consigned
purchase records
9(08) (^4 126) - 129 PACK BCD
3.
Overall market fall stop consigned sales
records
9(08) (^4 130) - 133 PACK BCD
3.
Overall market fall stop consigned
purchase volume 9(08)^4 134 -^137 PACK BCD^
3.33 Overall market fall stop consigned sales
volume
9(08) (^4 138) - 141 PACK BCD
3.
Funds rise stop consigned purchase
records
9(08) (^4 142) - 145 PACK BCD
3.35 Funds rise stop consigned sales records^9 (08) (^4 146) - 149 PACK BCD
3.
Funds rise stop consigned purchase
volume 9(08)^4 150 -^153 PACK BCD^
3.37 Funds rise stop consigned sales volume^ 9(08) (^4 154) - 157 PACK BCD
3.
Funds fall stop consigned purchase
records
9(08) (^4 158) - 161 PACK BCD
3.39 Funds fall stop consigned sales records^ 9(08) (^4 162) - 165 PACK BCD
3.
Funds fall stop consigned purchase
volume 9(08)^4 166 -^169 PACK BCD^
3.41 Fund fall stop consigned sales volume^ 9(08) (^4 170) - 173 PACK BCD
3.
Stock rise stop consigned purchase
records
9(08) (^4 174) - 177 PACK BCD
3.43 Stock rise stop consigned sales records^ 9(08) (^4 178) - 181 PACK BCD
3.
Stock rise stop consigned purchase
volume 9(08)^4 182 -^185 PACK BCD^
3.45 Stock rise stop consigned sales volume^ 9(08) (^4 186) - 189 PACK BCD
3.
Stock fall stop consigned purchase
records
9(08) (^4 190) - 193 PACK BCD
3.47 Stock fall stop consigned sales records^ 9(08) (^4 194) - 197 PACK BCD
3.
Stock fall stop consigned purchase
volume 9(08)^4 198 -^201 PACK BCD^
3.49 Stock fall stop consigned sales volume^ 9(08) (^4 202) - 205 PACK BCD
3.
Callable Bull Contract rise stop
consigned purchase records
9(08) (^4 206) - 209 PACK BCD
3.
Callable Bull Contract rise stop
consigned sales records
9(08) (^4 210) - 213 PACK BCD
3.
Callable Bull Contract rise stop
Consigned purchase volume 9(08)^4 214 -^217 PACK BCD^
3.53 Callable Bull Contract rise stop
consigned sales volume
9(08) (^4 218) - 221 PACK BCD


Taiwan Stock Exchange

```
3.54 Callable Bull Contract fall stop consigned
purchase records
```
9(08) (^4 222) - 225 PACK BCD
3.
Callable Bull Contract fall stop consigned
sales records
9(08) (^4 226) - 229 PACK BCD
3.
Callable Bull Contract fall stop consigned
purchase volume
9(08) (^4 230) - 233 PACK BCD
3.
Callable Bull Contract fall stop consigned
sales volume 9(08)^4 234 -^237 PACK BCD^
3.58 Callable Bear Contract rise stop
consigned purchase records
9(08) (^4 238) - 241 PACK BCD
3.
Callable Bear Contract rise stop
consigned sales records
9(08) (^4 242) - 245 PACK BCD
3.
Callable Bear Contract rise stop
consigned purchase volume
9(08) (^4 246) - 249 PACK BCD
3.
Callable Bear Contract rise stop
consigned sales volume 9(08)^4 250 -^253 PACK BCD^
3.62 Callable Bear Contract fall stop
consigned purchase records
9(08) (^4 254) - 257 PACK BCD
3.
Callable Bear Contract fall stop
consigned sales records
9(08) (^4 258) - 261 PACK BCD
3.
Callable Bear Contract fall stop
consigned purchase volume
9(08) (^4 262) - 265 PACK BCD
3.
Callable Bear Contract fall stop
consigned sales volume 9(08)^4 266 -^269 PACK BCD^
3.66 Taiwan Innovation Board rise stop
consigned purchase records
9(08) (^4 270) - 273 PACK BCD
3.
Taiwan Innovation Board rise stop
consigned sales records
9(08) (^4 274) - 277 PACK BCD
3.
Taiwan Innovation Board rise stop
consigned purchase volume
9(08) (^4 278) - 281 PACK BCD
3.
Taiwan Innovation Board rise stop
consigned sales volume 9(08)^4 282 -^285 PACK BCD^
3.70 Taiwan Innovation Board fall stop
consigned purchase records
9(08) (^4 286) - 289 PACK BCD
3.
Taiwan Innovation Board fall stop
consigned sales records
9(08) (^4 290) - 293 PACK BCD
3.
Taiwan Innovation Board fall stop
consigned purchase volume
9(08) (^4 294) - 297 PACK BCD
3.
Taiwan Innovation Board fall stop
consigned sales volume 9(08)^4 298 -^301 PACK BCD^
4 Checksum X(01) (^1 302) - 302 XOR VALUE
5 TERMINAL-CODE X(02) (^2 303) - 304

#### (HEXACODE: 0D

#### 0A)


Taiwan Stock Exchange

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.

2.2 Business Type: Expresses common stock transactions on the stock market in PACK BCD

“01” (length: 1 byte).
2.3 Transmission Format Code: Expresses Format 4 in PACK BCD “04” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “ 03 ” for Version 3 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) Statistics data are transmitted once every 5 seconds, and the same statistics time has the
same serial number.

3. BODY: totally 291 bytes.

3.1 Consignment Accumulated Time

```
(1) Expressed in PACK BCD (length: 3 bytes) to record time in hour, minute, second format
(HHMMSS).
(2) The value recorded in this field is the system time produced by the auction market
consignment statistics of common stocks on the market. At present, statistics are produced
every 5 seconds. When the value in the Statistics Time field is “999999”, this means that
data displayed in the field are the last statistics, and they will be transmitted repeated for
about 15 minutes.
```
```
3.2~3.61 Consignment statistics
Notes to data type:
```
```
3.2~3.5、3.26~3.33 Market-wide consignment statistics:
Market-wide consignment statistics information includes all consigned
transactions of securities and the content comprises consigned purchase records,
consigned sales records, consigned purchase volume, consigned sales volume,
rise stop consigned purchase records, rise stop consigned sales records, rise stop
consigned purchase volume, rise stop consigned sales volume, fall stop
consigned purchase records, fall stop consigned sales records, fall stop consigned
purchase volume, and stop consigned sales volume. Refer to Appendix B for the
principle of the codification of securities.
```
```
3.6~3.9、3.34~3.41 Statistics on consignment of funds:
Fund consignment statistics include beneficiary certificates, ETF, REAT, securitized
financial assets, and REIT. The content of information comprises the consigned
purchase records, consigned sales records, consigned purchase volume, consigned
```

Taiwan Stock Exchange

```
sales volume, rise stop purchase records, rise stop sales records, rise stop purchase
volume, rise stop sales volume, fall stop purchase records, fall stop sales records, fall
stop purchase volume, and fall stop sales volume. Refer to Appendix B for the
codification of securities.
```
```
3.10~3.13、3.42~3.49 Statistics on consignment of stocks:
The information on consignment of stocks is the statistics on common shares (refer
to common stock of Appendix B: Stock coding rules, does not include Taiwan
Innovation Board Securities.), and the content comprises consigned purchase records,
consigned sales records, consigned purchase volume, consigned sales volume, rise
stop consigned purchase records, rise stop consigned sales records, rise stop
consigned purchase volume, rise stop consigned sales volume, fall stop consigned
purchase records, fall stop consigned sales records, fall stop consigned purchase
volume, and fall stop consigned sales volume.
```
```
3.14~3.17、3.50~3.57 Statistics on consignment of Callable Bull Contracts:
Information on consignment of Callable Bull Contracts is the statistics on
Callable Bull Contracts with domestic securities or index and foreign securities
or index as underlying assets. The content comprises consigned purchase records,
consigned sales records, consigned purchase volume, consigned sales volume,
rise stop consigned purchase records, rise stop consigned sales records, rise stop
consigned purchase volume, rise stop consigned sales volume, fall stop
consigned purchase records, fall stop consigned sales records, fall stop consigned
purchase volume, and fall stop consigned sales volume. Refer to Appendix B on
the principle of the codification of securities.
```
```
3.18~3.21、3.58~3.65 Statistics on consignment of Callable Bear Contracts:
Information on consignment of Callable Bear Contracts is the statistics on
Callable Bull Contracts with domestic securities or index and foreign securities
or index as underlying assets. The content comprises consigned purchase
records, consigned sales records, consigned purchase volume, consigned sales
volume, rise stop consigned purchase records, rise stop consigned sales records,
rise stop consigned purchase volume, rise stop consigned sales volume, fall stop
consigned purchase records, fall stop consigned sales records, fall stop
consigned purchase volume, and fall stop consigned sales volume. Refer to
Appendix B on the principle of the codification of securities.^
```
```
3.22~3.25、3.66~3.73Statistics on consignment of Taiwan Innovation Board:
The information on consignment of Taiwan Innovation Board is the statistics on
Taiwan Innovation Board. The content comprises consigned purchase records,
consigned sales records, consigned purchase volume, consigned sales volume, rise
stop consigned purchase records, rise stop consigned sales records, rise stop
consigned purchase volume, rise stop consigned sales volume, fall stop consigned
purchase records, fall stop consigned sales records, fall stop consigned purchase
volume, and fall stop consigned sales volume. Refer to Appendix B on the
principle of the codification of securities.^
```
Notes to attributes of fields:


Taiwan Stock Exchange

```
3.2、3. 6 、3.10、3.14、3.18、 3 .22 consigned purchase records:
Represented by PACK BCD (length of 4 BYTE）on the records of accumulated
consigned purchase.
```
```
3.3、3.7、3.11、3.15、3.19、 3 .23 consigned sales records:
Represented by PACK BCD（length of 4 BYTE） on the records of
accumulated consigned sales.
```
```
3.4、3.8、3.12、3.16、3.20、3.24 consigned purchase volume:
Represented by PACK BCD（length of 4 BYTE） on the accumulated consigned
purchase volume. Each volume of transaction is a trading unit.
```
```
3.5、3.9、3.13、3.17、3.21、3.25 consigned sales volume:
Represented by PACK BCD（length of 4 BYTE）on the accumulated consigned
sales volume. Each volume of transaction is a trading unit.
```
```
3.26、3.3 4 、3. 42 、3. 50 、3.5 8 、 3 .66 rise stop consigned purchase records:
Represented by PACK BCD（length of 4 BYTE）on the records of accumulated
rise stop consigned purchase.
```
```
3.27、3.35、3.43、3.51、3.59、3.67 rise stop consigned sales records:
Represented by PACK BCD（length of 4 BYTE）on the records of accumulated
rise stop consigned sales.
```
```
3.28、3. 36 、3.44、3.52、3.60、3.68 rise stop consigned purchase volume:
Represented by PACK BCD（length of 4 BYTE） on
On the records of accumulated rise stop consigned purchase volume. The volume of
each transaction is one trading unit.^
```
```
3.29、3.37、3.45、3.53、3.61、3. 69 rise stop consigned sales volume:
Represented by PACK BCD（length of 4 BYTE） on the accumulated rise stop
consigned sales volume. The volume of each transaction is one trading unit. ）
```
```
3.3 0 、3.38、3.46、3.54、3.62、3.7 0 fall stop consigned purchase records:
Represented by PACK BCD（length of 4 BYTE）on the records of accumulated
rise stop consigned purchase.^
```
```
3.31、3.39、3.47、3.55、3.63、3.71 fall stop consigned sales records:
Represented by PACK BCD（length of 4 BYTE）on the records of accumulated
fall stop consigned sales.^
```
```
3.32、3.4 0 、3.48、3.56、3.64、3.72 fall stop consigned purchase volume: Represented
by PACK BCD（length of 4 BYTE）
On accumulated fall stop consigned purchase volume. The volume of each
transaction is one trading unit.
```

Taiwan Stock Exchange

```
3.33、3.41、3.49、3.57、3.65、3.73 fall stop consigned sales volume:
Represented by^ PACK^ BCD（length of^4 BYTE）^ On accumulated fall stop consigned
sales volume. The volume of each transaction is one trading unit.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 5 Stock Market Announcements

## TWSE Data Transmission Format

Format 5 :Stock Market Announcements Page: _1_^

Length (RL): Variable Length 14 - 74 Byte
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format
Code

#### 9(02) 1 5 - 5 PACK BCD “05”

```
2.4 Transmission Format
Version
```
#### 9(02) 1 6 - 6 PACK BCD “01”

```
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 1 - 61 11 - ??
3.1 Class 9(02) 1 11 - 11 PACK BCD
3.2 Announcement Items X(01)-X(60) 1 - ?? 12 - ?? ASCII > = 60 bytes
4 Checksum X(01) 1 ??-?? XOR VALUE
5 TERMINAL-CODE X(02) 2 ??-?? (HEXACODE: 0D 0A)
```
Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission
    formats.

2.1 Information Length

(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 5 in PACK BCD “05” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “01” for Version 1 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Intraday announcements are transmitted repeatedly at regular intervals through Format 5.
In case of new, emergency announcements, the cycle begins from 1.
```
3. BODY: Variable length from 1-61 bytes.

3.1 Class: Expressed in PACK BCD (length: 1 byte)

```
“00” --- Contents of general announcements
“09” --- End of general announcements
“90” --- Contents of emergency announcements
“99” --- End of emergency announcements
```
3.2 Announcement Items: Expressed in ASCII codes, variable length with a maximum of 60

```
bytes.
(1) When the value is “09”, it means “end of a general accouchement”.
(2) When the value is “99”, it means “end of an emergency accouchement”.
```

Taiwan Stock Exchange

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 6 Real-time Auction Quotes of Common Stocks on TWSE Market

## TWSE Data Transmission Format

## Format 6 :Real-time Auction Quotes of Common Stocks on TWSE Market Page: _1_^

Length (RL): Variable Length : 32 ~ 131 Bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “06”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “0 4 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 19 - 118
3.1 StockCode X (06) 6 11 - 16 ASCII
3.2 Matching Time 9(12) 6 17 - 22 PACK BCD
3.3 Disclosed Item Remarks X(01) 1 23 - 23 BIT MAP
3.4 Rise/Fall Remarks X(01) 1 24 - 24 BIT MAP
3.5 Status Remarks X(01) 1 25 - 25 BIT MAP
3.6 Accumulative Trading Volume 9(08) 4 26 - 29 PACK BCD
3.7 Real-time Quotes 0 - 99 30 - ?? OCCURS
3.7.1 Price Field 9(05)V9(4) 5 ??-?? PACK BCD 0 - 11 TIMES^
3.7.2 Volume Field (Sheet) 9(08) 4 ??-?? PACK BCD
4 Checksum X(01) 1 ??-?? XOR VALUE
5 TERMINAL-CODE X(02) 2 ??-?? (HEXACODE: 0D
0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).
2.3 Transmission Format Code: Expresses Format 6 in PACK BCD “06” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expressed in PACK BCD “0 4 ” for version 4 (length is 1 BYTE).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.

3. BODY: Variable length from 19 to 118 bytes.

3.1 Stock Code
(1) Expressed in ASCII 6 BYTE. Refer to Appendix B for stock coding rules.


Taiwan Stock Exchange

```
(2) If the Stock Code “000000” matched at time “99 9999999999 ”, it means the data on the
last entry of auction transaction of common stocks are being transmitted.
```
3.2 Matching Time

```
(1) Expressed in PACK BCD in the format hour: minute: second: millisecond: microsecond
(HH:MM:SS:MS:μS).
(2) In case of a held match (justifiable with bit 1-0 in Rise/Fall Remarks); the match held start
time is displayed in the Matching Time field.
(3) If the Stock Code “000000” matched at time “999999 999999 ”, it means the data on the
last entry of auction transaction of common stocks has been transmitted.
```
3.3 Disclosed Item Remarks
(1) Displayed items are expressed by bit (binary):

```
Bit 7 (trading price/volume)
0: No trading price/volume, no data transmitted.
1: With trading price/volume, and data transmitted.
```
```
Bit 6-4 (purchasing price/volume)
000 : No purchasing price/volume, no data transmitted.
001 - 101: Display the position of stocks by purchasing price/volume (the top five
positions are displayed in binary codes).
```
```
Bit 3-1 (selling price/volume)
000: No selling price/volume, no data transmitted.
001 - 101: Display the position of stocks by selling price/volume (the top five positions
are displayed in binary codes).
```
```
Bit 0 (Price and Volume of the best five stock transactions) –
0: Display the trading price and volume and the price and volume of the best five
stock transactions.
1: Only display the trading price and volume, the price and volume of the best five
stock transactions are not displayed.
```
Description:

```
After the consignment trading is matched for each transaction, it may have several
trading price and volume displayed. When display the final trading price and volume,
the price and volume of the best five stock transactions are also disclosed, Bit 0 = 0.
When it is not the final trading price and volume displayed, then only display the
trading price and volume but the price and volume of the best five stock transactions
are not displayed, Bit 0 = 1.
```
(2) The data length of every price and volume field is 5 and 4 bytes respectively.

3.4 Rise/Fall Remarks

```
(1) Rise/fall remarks, held match instantaneous price trends, and delayed remarks on market
at close are expressed by bit (default: 0 x 00)
```

Taiwan Stock Exchange

```
Bit 7-6: Trading rise/fall remarks
00: General trading
01: Fall stop trading
10: Rise stop trading
```
```
Bit 5-4: Optimal position purchase rise/fall remarks
00: General purchase
01: Fall stop purchase
10: Rise stop purchase
```
```
Bit 3-2: Optimal position sale rise/fall remarks
00: General sale
01: Fall stop sale
10: Rise stop sale
```
```
Bit 1-0: Instantaneous Price Trend
00: General display
01: Held match and instantaneous fall trend
10: Held match and instantaneous rise trend
11: [Reserved]
```
```
(2) Purchase (Sales) rise/fall remarks display only the purchase (selling) price of the stock at
the optimal position.
```
3.5 Status Remarks
(1) Trial status remarks, delayed open remarks after trial, delayed close remarks after trial and
way of matching remarks, open remarks and close remarks are expressed by individual
bit (default 0X00).

```
Bit 7 Trial status remarks
0: General display
1: Trial display
```
```
Bit 6 Delayed open remarks after trial
0: Negative
1: Positive
```
```
Bit 5 Delayed close remarks after trial
0: Negative
1: Positive
```
```
Bit 4 Way of matching remarks
0: Aggregate auction
1 : Continuous market method
```
```
Bit 3 Open remarks
0: Negative
1: Positive
```

Taiwan Stock Exchange

```
Bit 2 Close remarks
0: Negative
1: Positive
```
Bit 1-0 Reserved

```
(2) If Bit 7=1, it means at the moment real-time quote which is in the field of 3.7 is in the
Trial Status. If Bit 7 =0, it means real-time quote is in the General Display Status, and in
the meantime Bit 5 and 6 remarks are of meaninglessness.
(3) If Bit 3=1, represents current open display data; if Bit 2=1, represents current close display
data.
```
3.6 Accumulative Trading Volume: transmitting the latest accumulative trading volume of

individual stock by the unit of trading.

3.7 Real-time Quotes
Transmit the latest real-time quote information of stocks by trading price/volume, top five
positions by purchasing price/volume, and top five positions by selling price/volume. Every
trading, purchasing and selling volume represents one trading unit.
(1) The length of data expressed in PACK BCD in every price and volume field is 5 and 4
bytes respectively
(2) If Bit 7 of the Disclosed Item Remarks field is 1, this means there are trading
price/volume data.
i. If Instantaneous Price Trend of Rise/Fall remark is General Display i.e. Bit 1-0 = 00,
the item displays the current price and volume.
ii. If Instantaneous Price Trend of Rise/Fall remark is Held Match i.e. Bit 1-0 = 0 1 or
10, the item will display the latest price and volume which expressed by 0; neither
the purchasing nor the selling price/volume is displayed.

```
(3) i. If Bit 6-4 of the Disclosed Item Remarks field is 001-101, this means there are
purchasing price/volume data, and the data at the top five positions and their
purchasing price/volume (sheet or unit) are transmitted in binary codes in ascending
order.
ii. If the “price filed” of purchasing the best position shows 0, it means purchasing at
market price; the “volume field” is the market purchasing volume.
iii. The best purchasing price/volume is shown in ascending order based on the purchasing
price. Market purchasing price/volume, if any, is listed at the top first position.
iv. For government bonds, only data of the consigned purchasing price/volume (unit) of
one bond are displayed; i.e. Disclosed Item Remarks Bit 6-4=001.
```
```
(4) i. If Bit 3-1 of the Disclosed Item Remarks field is 001-101, this means there are selling
price/volume data, and the data at the top five positions and their selling price/volume
(sheet or unit) are transmitted in binary codes in ascending order.
ii. If the “price filed” of selling the best position shows 0, it means sold at market price;
the “volume field” is the market selling volume.
iii. The best selling price/volume is shown in ascending order based on the selling price.
Market selling price/volume, if any, is listed at the top first position.
```
iv. For government bonds, only data of the consigned selling price/volume (unit) of one


Taiwan Stock Exchange

bond are displayed; i.e. Disclosed Item Remarks Bit 3-1=001.

```
(5) For a trial match i.e. Status Remark Bit 7 = 1, the Real-time Quotes data is at the trial
stage.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

Example

1. The order number of the TSMC (2330) in Format 1 is “341”. A transaction is matched by the

```
computer at 09:04:15:61:278. Quote information in Record #4567 is displayed. The trading
price is $99.50 and accumulative trading volume is 16423 sheets. The purchasing price and
volume at the top five positions in order are: $99.50 and 250 sheets; $99.00 and 175 sheets;
$98.50 and 477 sheets; $97.50 and 669 sheets; and $97.00 and 125 sheets. The selling price and
volume of the top three stocks are $100.00 and 80 sheets; $100.50 and 675 sheets; and $101.50
and 460 sheets.
Field Name/
Description
```
```
Attribute Length Position Content Item Description
```
```
ESC-CODE X(01) 1 1 - 1 (ASCII 27)
Information
Length
```
```
9(04) 2 2 - 3 0x0 1 0x 13 Information Length: 113 BYTE
```
```
Business Type 9(02) 1 4 - 4 0x01 01: Common Stock Auction on Stock
Market
Transmission
Format Code
```
```
9(02) 1 5 - 5 0x06 Format 6
```
```
Transmission
Format Version
```
```
9(02) 1 6 - 6 0x0 4 Version 4
```
```
Transmission
S/N
```
```
9(08) 4 7 - 10 0x00 0x00
0x45 0x67
```
```
Record #4567
```
```
Stock Order
Number
```
```
X(0 6 ) 6 11 - 16 0x03 0x41 Relative Stock Order=341
```
```
Matching Time 9( 12 ) 6 17 - 22 0x09 0x04
0x15 0x06
0x12 0x78
```
#### 09:04:15:61:278

```
Disclosed Item
Remarks
```
```
X(01) 1 23 - 23 1 101 011 0 Disclosed Items:
7 1: With trading price and volume
6 - 4 101: Purchasing price/volume at the
top five positions
3 - 1 011: Selling price/volume of the top
three stocks
Rise/Fall
Remarks
```
```
X(01) 1 24 - 24 00 00 00 00 Displays rise/fall remarks of prices
Normal display of general trading,
purchasing and selling prices
Status Remark X(01) 1 25 - 25 0 0 0
0 0000(0x00)
```
```
General Display, Aggregated Auction
Method
Accumulative
Trading volume
```
```
9(08) 4 26 - 29 0x00 0x01
0x64 0x23
```
```
Accumulative Trading volume=16423
```
Trading Price 9(5)V9(4) 5 30 - 34 0x00 0x00
0x99 0x50
0x00

```
Trading Price=99.5000
```
Trading Volume 9(08) 4 35 - 38 0x00 0x 00
0x12 0x34

```
In the last Trading Volume=1234
```
Purchasing Price1 9(5)V9(4) 5 39 - 43 0x00 0x00
0x99 0x50
0x00

```
Purchasing Price=99.5000
```
Purchasing
Volume1

```
9(08) 4 44 - 47 0x00 0x00
0x02 0x50
```
```
Purchasing Volume=250
```
Purchasing Price2 9(5)V9(4) 5 48 - 52 0x00 0x00
0x99 0x00
0x00

```
Purchasing Price=99.0000
```
Purchasing
Volume2

```
9(08) 4 53 - 56 0x00 0x00
0x01 0x75
```
```
Purchasing Volume=175
```
Purchasing Price3 9(5)V9(4) 5 57 - 61 0x00 0x00 Purchasing Price=98.5000


```
Taiwan Stock Exchange
```
```
0x98 0x50
0x00
Purchasing
Volume3
```
```
9(08) 4 62 - 65 0x00 0x00
0x04 0x 77
```
```
Purchasing Volume=477
```
```
Purchasing Price4 9(5)V9(4) 5 66 - 70 0x00 0x00
0x97 0x50
0x00
```
```
Purchasing Price=97.5000
```
```
Purchasing
Volume4
```
```
9(08) 4 71 - 74 0x00 0x00
0x06 0x69
```
```
Purchasing Volume=669
```
```
Purchasing Price5 9(5)V9(4 5 75 - 79 0x00 0x00
0x97 0x00
0x00
```
```
Purchasing Price=97.0000
```
```
Purchasing
Volume5
```
```
9(08) 4 80 - 83 0x00 0x00
0x01 0x25
```
```
Purchasing Volume=125
```
```
Selling Price1 9(5)V9(4) 5 84 - 88 0x00 0x01
0x00 0x00
0x00
```
```
Selling Price=100.0000
```
```
Selling Volume1 9(08) 4 89 - 92 0x00 0x00
0x00 0x80
```
```
Selling Volume=80
```
```
Selling Price2 9(5)V9(4) 5 93 - 97 0x00 0x01
0x00 0x50
0x00
```
```
Selling Price=100.5000
```
```
Selling Volume2 9(08) 4 98 - 101 0x00 0x00
0x06 0x75
```
```
Selling Volume=675
```
```
Selling Price3 9(5)V9(4) 5 102 - 106 0x00 0x01
0x01 0x50
0x00
```
```
Selling Price=101.5000
```
```
Selling Volume3 9(08) 4 107 - 110 0x00 0x00
0x04 0x60
```
```
Selling Volume=460
```
Checksum X(01) 1 111 - 111 0xFB Checksum2~110 Byte XOR VALUE
TERMINAL-CODE X(02) 2 112 - 113 0x0D 0x0A HEXACODE:0D 0A


Taiwan Stock Exchange

2. The order number of the CSC (2002) in Format 1 is “0238”. A transaction is matched by the
    computer at 10:27:33:165:41. Quote information in Record #64323 is displayed. The rise
    stop-trading price is $13.85 and accumulative trading volume is 11921 sheets. The purchasing
    price and volume at the top five positions in order are: $13.85 (rise stop) and 540 sheets; $13.80
    and 230 sheets; $13.75 and 72 sheets; $13.70 and 69 sheets; and $13.65 and 81 sheets. There is
    no selling price and volume.
Field Name/
Description

```
Attribute Length Position Content Item Description
```
```
ESC-CODE X(01) 1 1 - 1 (ASCII 27)
Information
Length
```
```
9(04) 2 2 - 3 0x00 0x 86 Information Length: 86 BYTES
```
```
Business Type 9(02) 1 4 - 4 0x01 01: Common Stock Auction on Stock
Market
Transmission
Format Code
```
```
9(02) 1 5 - 5 0x06 Format 6
```
```
Transmission
Format Version
```
```
9(02) 1 6 - 6 0x0 4 Version 4
```
```
Transmission
S/N
```
```
9(08) 4 7 - 10 0x00 0x06 0x43
0x23
```
```
Record #64323
```
```
Stock Order
Number
```
```
X(0 6 ) 6 11 - 16 0x 3 2 0x30 0x30
0x32 0x20 0x20
```
```
Relative Stock Order=” 2 002 “
```
```
Matching Time 9( 12 ) 6 17 - 22 0x10 0x27 0x33
0x16 0x50 0x41
```
#### 10:27:33:165:41

```
Disclosed Item
Remarks
```
X(01) 1 23 - 23 1 101 000 0 Disclosed Items:
7 1: With Trading Price/Volume
6 - 4 101: Purchasing price/volume at the
top five positions
3 - 1 000: No selling price/volume
Rise/Fall Remarks X(01) 1 24 - 24 10 10 00 00 Displays rise/fall remarks of prices
Rise stop trading, best stock bought at
rise stop price, no instantaneous price
trend
Status Remarks X(01) 1 25 - 25 0000 0000(0x00) General Display, Aggregated Auction
Method
Accumulative
Trading Volume

```
9(08) 4 26 - 29 0x00 0x01 0x19
0x21
```
```
Accumulative Trading Volume=11921
```
Trading Price 9(5)V9(4) 5 30 - 34 0x00 0x00 0x13
0x85 0x00

```
Trading Price =13.8500
```
Trading Volume 9(08) 4 35 - 38 0x00 0x00 0x19
0x21

In the last Trading Volume= 1921
sheets
Purchasing Price1 9(5)V9(4) 5 39 - 43 0x00 0x00 0x13
0x85 0x00

```
Purchasing Price=13.8500
```
```
Purchasing
Volume1
```
```
9(08) 4 44 - 47 0x00 0x00 0x05
0x40
```
```
Purchasing Volume=540
```
Purchasing Price2 9(5)V9(4) 5 48 - 52 0x00 0x00 0x13
0x80 0x00

```
Purchasing Price=13.8000
```
```
Purchasing
Volume2
```
```
9(08) 4 53 - 56 0x00 0x00 0x02
0x30
```
```
Purchasing Volume=230
```
Purchasing Price3 9(5)V9(4) 5 57 - 61 0x00 0x00 0x13
0x75 0x00

```
Purchasing Price=13.7500
```
```
Purchasing
Volume3
```
```
9(08) 4 62 - 65 0x00 0x00 0x00
0x72
```
```
Purchasing Volume=72
```
Purchasing Price4 9(5)V9(4) 5 66 - 70 0x00 0x00 0x13
0x70 0x00

```
Purchasing Price=13.7000
```
Purchasing 9(08) 4 71 - 74 0x00 0x00 0x00 Purchasing Volume=69


```
Taiwan Stock Exchange
```
```
Volume4 0x69
Purchasing Price5 9(5)V9(4) 5 75 - 79 0x00 0x00 0x13
0x65 0x00
```
```
Purchasing Price=13.6500
```
```
Purchasing
Volume5
```
```
9(08) 4 80 - 83 0x00 0x00 0x00
0x81
```
```
Purchasing Volume=81
```
Checksum X(01) 1 84 - 84 0x6E Checksum2-83 Byte XOR VALUE
TERMINAL-CODE X(02) 2 85 - 86 0x0D 0x0A HEXACODE:0D 0A


Taiwan Stock Exchange

3. The order number of the TECO ( 1504 ) in Format 1 is “ 0161 ”. A transaction is matched by the
    computer at 09:50:23. Quote information in Record # 41234 is displayed. The fall stop trading
    price is $11.50 and accumulative trading volume is 650 sheets. The selling price and volume at
    the top five positions in order are: $11.50 (fall stop) and 70 sheets; $11.55 and 35 sheets; $11.60
    and 46 sheets; $11.65 and 28 sheets; and $11.70 and 19 sheets. There is no purchasing price and
    volume.
Field Name/
Description

```
Attribute Length Position Content Item Description
```
```
ESC-CODE X(01) 1 1 - 1 (ASCII 27)
Information
Length
```
```
9(04) 2 2 - 3 0x00 0x 86 Information Length: 86 BYTE
```
```
Business Type 9(02) 1 4 - 4 0x01 01: Common Stock Auction on Stock
Market
Transmission
Format Code
```
```
9(02) 1 5 - 5 0x06 Format 6
```
```
Transmission
Format Version
```
```
9(02) 1 6 - 6 0x0 4 Version 4
```
```
Transmission
S/N
```
```
9(08) 4 7 - 10 0x00 0x04 0x12 0x34 Record #41234
```
```
Stock Order
Number
```
```
X(0 6 ) 6 11 - 16 0x 3 1 0x 35 0x30 0x34
0x20 0x20
```
```
Relative Stock Order=” 1 504 “
```
```
Matching Time 9( 12 ) 6 17 - 22 0x09 0x50 0x23 0x27
0x15 0x34
```
#### 09:50:23:271:534

```
Disclosed Item
Remarks
```
```
X(01) 1 23 - 23 1 000 101 0 Disclosed Items:
7 1: With Trading Price/Volume
6 - 4 000: No purchasing price/ volume
3 - 1 101: With the top five positions for
selling price/volume
Rise/Fall
Remarks
```
X(01) 1 24 - 24 01 00 01 00 Displays rise/fall remarks of prices
Fall stop trading, sold at the best fall
stop position, no instantaneous price
trend
Status Remarks X(01) 1 25 - 25 0000 0000(0x00) General Display, Aggregated Auction
Method
Accumulative
Trading Volume

```
9(08) 4 26 - 29 0x00 0x00 0x 06 0x 50 Accumulative Trading Volume= 650
```
```
Trading Price 9(5)V9(4) 5 30 - 34 0x00 0x00 0x11 0x50
0x00
```
```
Trading Price=11.5000
```
Trading Volume 9(08) 4 35 - 38 0x00 0x00 0x00 0x17 In the last Trading Volume= 17
Selling Price1 9(5)V9(4) 5 39 - 43 0x00 0x11 0x50 0x00 Selling Price=11.5000
Selling
Volume1

```
9(08) 4 44 - 47 0x00 0x00 0x00 0x70 Selling Volume=70
```
```
Selling Price2 9(5)V9(4) 5 48 - 52 0x00 0x11 0x55 0x00 Selling Price=11.5500
Selling
Volume2
```
```
9(08) 4 53 - 56 0x00 0x00 0x00 0x35 Selling Volume=35
```
```
Selling Price3 9(5)V9(4) 5 57 - 61 0x00 0x11 0x60 0x00 Selling Price=11.6000
Selling
Volume3
```
```
9(08) 4 62 - 65 0x00 0x00 0x00 0x46 Selling Volume=46
```
```
Selling Price4 9(5)V9(4) 5 66 - 70 0x00 0x11 0x65 0x00 Selling Price=11.6500
Selling
Volume4
```
```
9(08) 4 71 - 74 0x00 0x00 0x00 0x28 Selling Volume=28
```
```
Selling Price5 9(5)V9(4) 5 75 - 79 0x00 0x11 0x70 0x00 Selling Price=11.7000
Selling
Volume5
```
```
9(08) 4 80 - 83 0x00 0x00 0x00 0x19 Selling Volume=19
```
```
Checksum X(01) 1 84 - 84 0xA2 Checksum2-83Byte XOR VALUE
```

```
Taiwan Stock Exchange
```
TERMINAL-CODE X(02) 2 85 - 86 0x0D 0x0A HEXACODE:0D 0A


```
Taiwan Stock Exchange
```
4. The order number of the FPC ( 1301 ) in Format 1 is “ 75 ”. After instantaneous price stabilization,
    the held match message is transmitted at 09:45:19. Quote information in Record # 8325 is
    displayed. The last trading record is rise stop trading price is $33.50 and accumulative trading
    volume is 1558 sheets. The selling price and volume at the top five positions in order are:
    $11.50 (fall stop) and 70 sheets; $11.55 and 35 sheets; $11.60 and 46 sheets; $11.65 and 28
    sheets; and $11.70 and 19 sheets. There is no purchasing price and volume.
       Field Name/
          Description

```
Attribute Length Position Content Item Description
```
```
ESC-CODE X(01) 1 1 - 1 (ASCII 27)
Information Length 9(04) 2 2 - 3 0x00 0x 41 Information Length: 41 BYTE
Business Type 9(02) 1 4 - 4 0x01 01: Common Stock Auction on Stock
Market
Transmission Format
Code
```
```
9(02) 1 5 - 5 0x06 Format 6
```
```
Transmission Format
Version
```
```
9(02) 1 6 - 6 0x0 4 Version 4
```
```
Transmission S/N 9(08) 4 7 - 10 0x00 0x00
0x83 0x25
```
```
Record #8325
```
Stock Order Number X(0 6 ) 6 11 - (^16) 0x31 0x33
0x30 0x31
0x20 0x20
Stock No.=”1301 “
Matching Time 9( 12 ) 6 17 - (^22) 0x09 0x45
0x19 0x03
0x30 0x17

#### 09:45:19:33:17

```
Disclosed Item
Remarks
```
```
X(01) 1 23 - 23 100000 0 0 Disclosed Items:
7 1: With Trading Price/Volume
6 - 5 00: No purchasing price
4 0: No purchasing volume
3 - 2 00: No selling price
1 0: No selling volume
Rise/Fall Remarks X(01) 1 24 - 24 10 00 00 01 Displays rise/fall remarks of prices
Rise stop trading, instantaneous trend:
fall
Status Remarks X(01) 1 25 - 25 0 0 0
0 0000(0x00)
```
```
General Display, Aggregated Auction
Method
Accumulative Trading
Volume
```
```
9(08) 4 26 - 29 0x00 0x00
0x15 0x58
```
```
Accumulative Trading Volume=1558
```
```
Trading Price 9(5)V9(4) 5 30 - 34 0x00 0x33
0x50 0x00
```
```
Trading Price=33.5000
```
```
Trading Volume 9(08) 4
35 - 38
0x00 0x00
0x00 0x00
```
```
In the last Trading Volume= 0
```
Checksum X(01) 1 39 - 39 0xC6 Checksum2-38 Byte XOR VALUE
TERMINAL-CODE X(02) 2 40 - 41 0x0D 0x0A HEXACODE:0D 0A


Taiwan Stock Exchange

5. The order number of the TSMC (2330) in Format 1 is “341”. A transaction is matched by the
    computer at 09:04:15:61:278. Quote information in Record #4567 is displayed. The trading
    price is $99.50 and accumulative trading volume is 16423 sheets. The purchasing price and
    volume at the top five positions in order are: $0.0 and 250 sheets; $99.00 and 175 sheets; $98.50
    and 477 sheets; $97.50 and 669 sheets; and $97.00 and 125 sheets.
Field Name/
Description

```
Attribute Length Position Content Item Description
```
```
ESC-CODE X(01) 1 1 - 1 (ASCII 27)
Information
Length
```
```
9(04) 2 2 - 3 0x00 0x8 6 Information Length: 86 BYTE
```
```
Business Type 9(02) 1 4 - 4 0x01 01: Common Stock Auction on Stock
Market
Transmission
Format Code
```
```
9(02) 1 5 - 5 0x06 Format 6
```
```
Transmission
Format Version
```
```
9(02) 1 6 - 6 0x04 Version 4
```
```
Transmission
S/N
```
```
9(08) 4 7 - 10 0x00 0x00
0x45 0x67
```
```
Record #4567
```
```
Stock Order
Number
```
```
X(06) 6 11 - 16 0x03 0x41 Relative Stock Order=341
```
```
Matching Time 9 (12) 6 17 - 22 0x09 0x04
0x15 0x06
0x12 0x78
```
#### 09:04:15:61:278

```
Disclosed Item
Remarks
```
```
X(01) 1 23 - 23 1 101 011 0 Disclosed Items:
7 1: With trading price and volume
6 - 4 101: Purchasing price/volume at the
top five positions
3 - 1 000: No selling price/volume
Rise/Fall
Remarks
```
```
X(01) 1 24 - 24 00 00 00 00 Displays rise/fall remarks of prices
Normal display of general trading,
purchasing and selling prices
Status Remark X(01) 1 25 - 25 0 0 0
0 0000(0x00)
```
```
General Display, Aggregated Auction
Method
Accumulative
Trading volume
```
```
9(08) 4 26 - 29 0x00 0x01
0x64 0x23
```
```
Accumulative Trading volume=16423
```
Trading Price 9(5)V9(4) 5 30 - 34 0x00 0x00
0x99 0x50
0x00

```
Trading Price=99.5000
```
Trading Volume 9(08) 4 35 - 38 0x00 0x00
0x12 0x34

```
In the last Trading Volume=1234
```
Purchasing Price1 9(5)V9(4) 5 39 - 43 0x00 0x00
0x99 0x50
0x00

```
Purchasing Price=0.0 000
purchasing at market price
```
Purchasing
Volume1

```
9(08) 4 44 - 47 0x00 0x00
0x02 0x50
```
```
Purchasing Volume=250
```
Purchasing Price2 9(5)V9(4) 5 48 - 52 0x00 0x00
0x99 0x00
0x00

```
Purchasing Price=99.0000
```
Purchasing
Volume2

```
9(08) 4 53 - 56 0x00 0x00
0x01 0x75
```
```
Purchasing Volume=175
```
Purchasing Price3 9(5)V9(4) 5 57 - 61 0x00 0x00
0x98 0x50
0x00

```
Purchasing Price=98.5000
```
Purchasing
Volume3

```
9(08) 4 62 - 65 0x00 0x00
0x04 0x77
```
```
Purchasing Volume=477
```
Purchasing Price4 9(5)V9(4) 5 66 - 70 0x00 0x00 Purchasing Price=97.5000


```
Taiwan Stock Exchange
```
```
0x97 0x50
0x00
Purchasing
Volume4
```
```
9(08) 4 71 - 74 0x00 0x00
0x06 0x69
```
```
Purchasing Volume=669
```
```
Purchasing Price5 9(5)V9(4 5 75 - 79 0x00 0x00
0x97 0x00
0x00
```
```
Purchasing Price=97.0000
```
```
Purchasing
Volume5
```
```
9(08) 4 80 - 83 0x00 0x00
0x01 0x25
```
```
Purchasing Volume=125
```
Checksum X(01) 1 84 - 84 0xFB Checksum2~ 83 Byte XOR VALUE
TERMINAL-CODE X(02) 2 85 - 86 0x0D 0x0A HEXACODE:0D 0A


Taiwan Stock Exchange

### Format 7 Statistics on Finished Fixed-price Transactions of Common on TWSE Markets

## TWSE Data Transmission Format

Format 7 : Statistics on Finished Fixed-price Transactions of Common

Stocks on TWSE Markets

Page: _1_

Length (RL): Fixed length 37 Byte
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “0037”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “07”
2.4 Transmission Format Version 9 (02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 24 11 - 34
3.1 Statistics Time 9(06) 3 11 - 13 PACK BCD
3.2 Total Trading Amount 9(15) 8 14 - 21 PACK BCD
3.3 Trading Volume 9(15) 8 22 - 29 PACK BCD
3.4 Trading Records 9(10) 5 30 - 34 PACK BCD
4 Checksum X(01) 1 35 - 35 XOR VALUE
5 TERMINAL-CODE X(02) 2 36 - 37 (HEXACODE: 0D 0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 7 in PACK BCD “07” (length: 1 byte).
2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “01” for Version 1 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
```
3. BODY: Totally 24 bytes.

3.1 Statistics Time

```
(1) Expressed in PACK BCD (length: 3 bytes) to record time in hour, minute, second format
(HHMMSS).
(2) After the after-hour fixed-price system finishes matching transactions, statistics of the
accumulative total trading amount, trading volume and trading records of after-hour
fixed-price transactions (except the statistics of the accumulative total trading amount,
trading volume and trading records of auctions before closing) will be transmitted once.
The value displayed in the Statistics Time field at this moment is the system processing
```

Taiwan Stock Exchange

```
time. After sending the accumulative trading statistics of after-hour Fixed-price
Transactions for the first time, the TWSE will continue to send the statistics of the
accumulative total trading amount, trading volume and trading records of after-hour
Fixed-price Transactions at about every minute. At this moment, the value displayed in the
Statistics Time field is “999999”, and Transmission S/N remains unchanged.
```
3.2 Total Trading Amount: Expressed in PACK BCD (length 8 bytes) to record the accumulative
total trading amount of Fixed-price Transactions.

3.3 Trading Volume: Expressed in PACK BCD (length 8 bytes) to record the accumulative total

trading volume; every volume represents one trading unit.

3.4 Trading Records: Expressed in PACK BCD (length 5 bytes) to record the accumulative total

trading records of Fixed-price Transactions

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 8 Statistics on Fixed-price Transaction Consignments of Common Stocks on TWSE Market

## TWSE Data Transmission Format

Format 8 :Statistics on Fixed-price Transaction Consignments

of Common Stocks on TWSE Market

Page: _1_

Length (RL): Fixed length 112 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “0112”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “08”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 99 11 - 109
3.1 Consignment Accumulated Time 9(06) 3 11 - 13 PACK BCD
3.2 Consigned purchase records 9(08) 4 14 - 17 PACK BCD
3.3 Consigned sales records 9(08) 4 18 - 21 PACK BCD
3.4 Consigned purchase volume 9(08) 4 22 - 25 PACK BCD
3.5 Consigned Sales volume 9(08) 4 26 - 29 PACK BCD
3.6 Fund Consigned purchase records 9(08) 4 30 - 33 PACK BCD
3.7 Fund Consigned sales Records 9(08) 4 34 - 37 PACK BCD
3.8 Fund Consigned purchase volume 9(08) 4 38 - 41 P PACK BCD
3.9 Fund Consigned sales volume 9(08) 4 42 - 45 P PACK BCD
3.10 Total Rise Stop Consigned purchase records 9(08) 4 46 - 49 PACK BCD
3.11 Total Rise Stop Consigned sales records 9(08) 4 50 - 53 PACK BCD
3.12 Total Rise Stop Consigned purchase volume 9(08) 4 54 - 57 PACK BCD
3.13 Total Rise Stop Consigned sales volume 9(08) 4 58 - 61 PACK BCD
3.14 Total Fall Stop Consigned purchase records 9(08) 4 62 - 65 PACK BCD
3.15 Total Fall Stop Consigned sales records 9(08) 4 66 - 69 PACK BCD
3.16 Total Fall Stop Consigned purchase volume 9(08) 4 70 - 73 PACK BCD
3.17 Total Fall Stop Consigned sales Volume 9(08) 4 74 - 77 PACK BCD
3.18 Fund Rise Stop Consigned purchase records 9(08) 4 78 - 81 PACK BCD
3.19 Fund Rise Stop Consigned sales records 9(08) 4 82 - 85 PACK BCD
3.20 Fund Rise Stop Consigned purchase volume 9(08) 4 86 - 89 PACK BCD
3.21 Fund Rise Stop Consigned sales volume 9(08) 4 90 - 93 PACK BCD
3.22 Fund Fall Stop Consigned purchase records 9(08) 4 94 - 97 PACK BCD
3.23 Fund Fall Stop Consigned sales records 9(08) 4 98 - 101 PACK BCD
3.24 Fund Fall Stop Consigned purchase volume 9(08) 4 102 - 105 PACK BCD
3.25 Fund Fall Stop Consigned sales volume 9(08) 4 106 - 109 PACK BCD
4 Checksum X(01) 1 110 - 110 XOR VALUE
5 TERMINAL-CODE X(02) 2 111 - 112 (HEXACODE: 0D
0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.
2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```

Taiwan Stock Exchange

2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”
(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 8 in PACK BCD “08” (length: 1 byte).

2.4 Transmission Format Version

(1) Expresses version in PACK BCD, e.g. “01” for Version 1 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) Statistics are transmitted once every 30 seconds; and the same statistics time uses the
same S/N.
```
3. BODY: Totally 99 bytes.

3.1 Consignment Accumulated Time

```
(1) Expressed in PACK BCD (length 3 bytes) to record time in hour, minute, second format
(HHMMSS).
(2) The value displayed in this field is the system time of statistics produced on the
fixed-price transaction consignments of common stocks on the market. At present,
statistics are produced once a minute. If the value in the Statistics Time is “999999”, this
means they are the last statistics and will be transmitted repeatedly for about 15 minutes.
```
3.2-3.25: Consignment Volume: Expressed in PACK BCD (length: 4 bytes) to express the
accumulative total consigned purchase and sales volume; each volume represents one
trading unit.

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

```
Format 9 Trading Information of Individual Common Stocks in Fixed-price Transactions on TWSE
```
## Market

## TWSE Data Transmission Format

Format 9 : Trading Information of Individual Common Stocks

## in Fixed-price Transactions on TWSE Market

Page: _1_

Length (RL): Fixed Length 31 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “0031”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “09”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “03”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 18
3.1 Stock Code X (06) 6 11 - 16 ASCII
3.2 Matching Time 9(06) 3 17 - 19 PACK BCD
3.3 Trading Price 9(5)V9(4) 5 20 - 24 PACK BCD
3.4 Trading Volume (sheet) 9(08) 4 25 - 28 PACK BCD
4 Checksum X(01) 1 29 - 29 XOR VALUE
5 TERMINAL-CODE X(02) 2 30 - 31 (HEXACODE: 0D 0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 9 in PACK BCD “09” (length: 1 byte).

2.4 Transmission Format Version:
(1) Version 3 is represented by PACK BCD “0 3 ”(length: 1 BYTE).
(2) Versions are numbered from 1, and every format has individual version numbers.

2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) After the after-hour fixed-price system finishes matching transactions, data of the trading
price and volume of all stocks (except the accumulative trading volume of auctions). After
transmitting such data of every stock for one cycle, data of the trading price and
accumulative trading volume of every stock are transmitted repeatedly every minute. The
Transmission S/N of each cycle begins from 1.
```
3. BODY: Totally 16 bytes.

3.1 Stock -Code

(1) TASCII 6 BYTE. Refer to Appendix B for stock coding rules.


Taiwan Stock Exchange

```
(2) After the after-hour fixed-price system finishes matching transactions, data of the trading
price and volume of every stock will be transmitted repeatedly. In the last entry on record
of each cyclical sequence, the Stock Code is expressed in “0000 00 ” and the Matching
Time in “999999”.
```
3.2 Matching Time

(1) Expressed in PACK BCD to display matching time in hour, minute, second (HH:MM:SS)
format.
(2) After the after-hour fixed-price system finishes matching transactions, data of the trading
price and volume of every stock will be transmitted repeatedly. In the last entry on record
of each cyclical sequence, the Stock code is expressed in “0000 00 ” and the Matching
Time in “999999”.
3.3 Trading Price (length: 5 bytes): Expressed in PACK BCD to record the trading price of every

stock in fixed-price transactions.

3.4 Trading Volume: Expressed in PACK BCD (length 4 bytes) to record the accumulative total

trading volume of individual stocks; every volume represents one trading unit.

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


```
Taiwan Stock Exchange
```
```
Example:
A transaction is matched at 14:30:00. The trading price is NT$11.50 and accumulative trading
volume is 650 sheets.
Field Name/
Description
```
```
Attribute Length Position Content Item Description
```
```
ESC-CODE X(01) 1 1 - 1 (ASCII 27)
Information
Length
```
```
9(04) 2 2 - 3 0x00 0x25 Information Length: 31 BYTE
```
```
Business Type 9(02) 1 4 - 4 0x01 01: Common Stock Auction on Stock
Market
Transmission
Format Code
```
```
9(02) 1 5 - 5 0x09 Format 9
```
```
Transmission
Format Version
```
```
9(02) 1 6 - 6 0x0 3 Version 3
```
```
Transmission
S/N
```
```
9(08) 4 7 - 10 0x00 0x00 0x01
0x70
```
```
Record #170
```
```
Stock Order
Number
```
```
9(04) 2 11 - 12 0x01 0x61 Relative Stock Order=161
```
```
Matching Time 9(06) 3 13 - 15 0x14 0x30 0x00 14:30:00
Trading Price
9(5)V9(4) 5 20 - 24
0x00 0x00 0x11
0x50 0x00
```
```
Trading Price=11.50
```
```
Trading
Volume
```
9(08) (^4 25) - 28 0x00 0x00 0x06
0x50
Accumulative Trading Volume=650
sheets
Checksum X(01) 1 29 - 29 0x0E Checksum2~23 Byte XOR VALUE
TERMINAL-CODE X(02) 2 30 - 31 0x0D 0x0A HEXACODE: 0D 0A


Taiwan Stock Exchange

### Format 10 Updated Information of TWSE-complied Indices

## TWSE Data Transmission Format

## Format 10 : Information of TWSE-compiled Indices Page: _1_^

Length (RL): Fixed length 26 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “0026”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “10”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 13 11 - 23
3.1 Index Code X(06) 6 11 - 16 ASCII
3.2 Index Time 9(06) 3 17 - 19 PACK BCD
3.3 Latest Index 9( 6 )V99 4 20 - 23 PACK BCD
4 Checksum X(01) 1 24 - 24 XOR VALUE
5 TERMINAL-CODE X(02) 2 25 - 26 HEXACODE:0D 0A

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission
    formats.

2.1 Information Length

(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 10 in PACK BCD “10” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “01” for Version 1 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) S/N “00000000” represents the previous-day closing index.
```
3. BODY

3.1 Index Code: Expressed in ASCII 6 BYTE to record data of indices established by FTSE

Group and Research Affiliates, LLC, and calculated by the TWSE. New indices established in
the future will also be transmitted in this format. Please refer to Format 21, for details.
3.2 Index Time

```
(1) Expressed in PACK BCD, length 3 bytes to record time in the hour, minute, second
(HH:MM:SS) format.
(2) The value displayed in this field is the system time of index data received by the FTSE
Group calculated at every 5 seconds.
(3) When the value of the Index Time and Transmission S/N is “0”, this means such index
data are the closing index data of the previous day.
```

Taiwan Stock Exchange

```
(4) When the Index Time is “999999”, this means data are the closing indices of today.
Closing index data are transmitted at every 5 seconds over a period of about 15 minutes.
```
3.3 Latest Index: Expressed and transmitted in PACK BCD, length: 4 bytes.

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 12 Information of Auction Opening (Closing) Price of Common Stocks on TWSE

## TWSE Data Transmission Format

Format 12 : Information of Auction Opening (Closing) Price Page: _1_

of Common Stocks on TWSE

Length (RL): Fixed length 374 Bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “ 0374 ”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “12”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “ 03 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 361 11 - 371
3.1 Stock Entries 9(02) 1 11 - 11 PACK BCD
3.2 Data Content X(25) 360 OCCURS 10
TIMES
3.21 Stock Code X(06) 6 ?-? ASCII
3.22 Opening Price 9(5)V9(4) 5 ?-? PACK BCD
3.23 Highest Trading Price 9(5)V9(4) 5 ?-? PACK BCD
3.24 Lowest Trading Price 9(5)V9(4) 5 ?-? PACK BCD
3.25 Recent trading price 9(5)V9(4) 5 ?-? PACK BCD
3.26 Accumulative Trading Volume 9(8) 4 ?-? PACK BCD
3.27 Time 9( 12 ) 6 ?-? PACK BCD
4 Checksum X(01) 1 372 - 372 XOR VALUE
5 TERMINAL-CODE X(02) 2
373 - 374

#### (HEXACODE ︰

#### 0D 0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.

2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).
2.3 Transmission Format Code: Expresses Format 12 in PACK BCD “12” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “ 03 ” for Version 3 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) Opening (Closing) price data are transmitted at every 10 minutes during the transactions.
Opening (Closing) price data are transmitted at every 5 minutes after the market is closed.

3. BODY: (Length = 361 bytes)


Taiwan Stock Exchange

3.1 Stock Entries
(1) Expressed in PACK BCD; length 1 byte.
(2) Information includes data content and stock entries (including the entry of Stock Code =
“000000” in the end of every cycle).
(3) When there are less than 10 entries, the stock code in the Data Content field is expressed
in spaces; whiles others are in low-value.
3.2 Data Content: length 360 bytes ( 36 bytes * 10 times)

3.2.1 Stock Code

(1) Length: 6 bytes.
(2) When the value of Stock Code is “000000”, this means the cycle ends. At this moment,
the value displayed in the Trading Price, Trading Volume and Trading Time fields is “0”.
3.2.2 Opening Price

```
(1) Expressed in PACK BCD, length 3 bytes, to record the auction opening price of
common stocks.
(2) Opening Price = 0 means:
a. No opening price has not come out today; or
b. The Stock Code is “000000” when the cycle ends.
```
3.2.3 Highest Trading Price:

```
Expressed in PACK BCD, length 5 bytes, to record the highest trading price of the auction
of common stocks after the market opens.
```
3.2.4 Lowest Trading Price:
Expressed in PACK BCD, length 5 bytes, to record the lowest trading price of the auction of
common stocks after the market opens.

3.2.5 Recent trading price:

```
(1) Expressed in PACK BCD, length 5 bytes, to record the recent trading price of the
auction of common stocks in the market.
(2) If the closing time is “999999 999999 ” when the market is closed, the closing price of the
auction of common stocks is recorded in this field.
```
3.2.6 Accumulative Trading Volume:

```
(1) Expressed in PACK BCD, length 4 bytes, to record the accumulative trading volume
(sheet) so far of the day.
(2) If the closing time is “999999 999999 ”, the total trading volume of stocks is recorded in
this field.
```
3.2.7 Time

```
(1) Expressed in PACK BCD, length 3 bytes, to record time in the hour: minute: second
(HH:MM:SS) , 3-digit millisecond (mmm), and 3-digit microsecond(μμμ)format.
(2) The time of the recent trading price is recorded in this field during the market is
operating.
(3) The value in this field after the market is closed is always “999999 999999 ”; and the
closing price is transmitted until the transmission ends.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

Format 12 transmits 922 entries of data every cycle (e.g. there are 921 stocks on the market during
09:00-13:30)

Transmission Time: 09:05:00

Field Name/Description Attribute Entry 1 Entry 2 ．．．． ．．．． Entry 92
ESC-CODE X(01)

Information Length 9(04) 0374 0374 0374

Business Type 9(02) 01 01 01

Transmission Format Code 9(02) 12 12 12

Transmission Format Version 9(02) 03 03 03

Transmission S/N 9(08) 00000001 00000002 00000092

Stock Entries 9(02) 10 10 10

Stock Code (01) X(06) 1101 1201 9933

Opening Price (01) 9(5)V9(4) 000125000 000093000 000125000

Highest Trading Price (01) 9(5)V9(4) 000128000 000098000 000128000

Lowest Trading Price (01) 9(5)V9(4) 000124000 000094000 000124000

Recent trading price (01) 9(5)V9(4) 000125000 000095000 000125000

Accumulative Trading Volume
(01)

```
9(8) 00000008 00000218 00000048
```
Time (01) 9(6) 091003 091011 091003

Stock Code (02) X(06) 1102 1202 9934

Opening Price (02) 9(5)V9(4) 000145000 000105000 000145000

Highest Trading Price (02) 9(5)V9(4) 000148000 000108000 000148000

Lowest Trading Price (02) 9(5)V9(4) 000144000 000104000 000144000

Recent trading price (02) 9(5)V9(4) 000145000 000105000 000145000

Accumulative Trading Volume
(02)

```
9(8) 00000022 00000062 00000022
```
Time (02) 9(6) 091005 091015 091005

```
． ． ． ． ． ． ．
． ． ． ． ． ． ．
```
Stock Code (09) X(06) 1109 1215 9938

Opening Price (09) 9(5)V9(4) 000095000 000055200 000095000

Highest Trading Price (09) 9(5)V9(4) 000098000 000058500 000098000

Lowest Trading Price (09) 9(5)V9(4) 000094000 000054500 000094000

Recent trading price (09) 9(5)V9(4) 000094000 000054500 000094000

Accumulative Trading Volume
(09)

```
9(8) 00000010 00000310 00000010
```
Time (09) 9(6) 091023 091003 091023

Stock Code (10) X(06) 1110 1216 9939

Opening Price (10) 9(5)V9(4) 000072200 000142200 000072200

Highest Trading Price (10) 9(5)V9(4) 000073400 000143400 000073400

Lowest Trading Price (10) 9(5)V9(4) 000072100 000142100 000072100

Recent trading price (10) 9(5)V9(4) 000072300 000142300 000072300

Accumulative Trading Volume
(10)

```
9(8) 00000148 00000077 00000328
```
Time (10) 9(6) 091053 091003 091053

Checksum X(01)


Taiwan Stock Exchange

TERMINAL-CODE X(02)

Field Name/Description Attribute Entry 93
ESC-CODE X(01)

Information Length 9(04) 0374

Business Type 9(02) 01

Transmission Format Code 9(02) 12

Transmission Format Version 9(02) 01

Transmission S/N 9(08) 00000093

Stock Entries 9(02) 2

Stock Code (01) X(06) 9945

Opening Price (01) 9(5)V9(4) 000045000

Highest Trading Price (01) 9(5)V9(4) 000048000

Lowest Trading Price (01) 9(5)V9(4) 000044000

Recent trading price (01) 9(5)V9(4) 000045000

Accumulative Trading Volume
(01)

```
9(8) 00000042
```
Time (01) 9( 12 ) 091010117873

Stock Code (02) X(06) 000000

Opening Price (02) 9(5)V9(4) 000000000

Highest Trading Price (02) 9(5)V9(4) 000000000

Lowest Trading Price (02) 9(5)V9(4) 000000000

Recent trading price (02) 9(5)V9(4) 000000000

Accumulative Trading Volume
(02)

```
9(8) 00000000
```
Time (02) (^) 9(12) 000000000000
Stock Code (03) X(06)
Opening Price (03) 9(5)V9(4) 000000000
Highest Trading Price (03) 9(5)V9(4) 000000000
Lowest Trading Price (03) 9(5)V9(4) 000000000
Recent trading price (03) 9(5)V9(4) 000000000
Accumulative Trading Volume
(03)
9(8) 00000000
Time (03) (^) 9(12) 000000000000
．．．．．． ．．．．．． ．．．．．．
．．．．．． ．．．．．． ．．．．．．
Stock Code (10) X(06)
Opening Price (10) 9(5)V9(4) 000000000
Highest Trading Price (10) 9(5)V9(4) 000000000
Lowest Trading Price (10) 9(5)V9(4) 000000000
Recent trading price (10) 9(5)V9(4) 000000000
Accumulative Trading Volume
(10)
9(8) 00000000
Time (10) 9(6) 000000
Checksum X(01)
TERMINAL-CODE X(02)


Taiwan Stock Exchange

1. In Entry 93 of every cycle, there are no data for stocks 3-10; attribute X is expressed in spaces
    and 9 in low-value.
2. After the market is closed, the time value is “999 999999 999”, and data of the closing price

(recent trading price) and in related fields are displayed repeatedly until the transmission ends.


Taiwan Stock Exchange

.......Transmission time after market closed: 13:40-end of transmission

Field Name/Description Attribute Entry 1 Entry 2 (^) ．．．． ．．．． Entry 92
ESC-CODE X(01)
Information Length 9(04) 0374 0374 0374
Business Type 9(02) 01 01 01
Transmission Format Code 9(02) 12 12 12
Transmission Format Version 9(02) 03 03 03
Transmission S/N 9(08) 00000001 00000002 00000092
Stock Entries 9(02) 10 10 10
Stock Code (01) X(06) 1101 1201 9933
Opening Price (01) 9(5)V9(4) 000125000 000093000 000125000
Highest Trading Price (01) 9(5)V9(4) 000128000 000098000 000128000
Lowest Trading Price (01) 9(5)V9(4) 000124000 000094000 000124000
Recent trading price (01) 9(5)V9(4) 000125000 000095000 000125000
Accumulative Trading Volume
(01)
9(8) 00002208 00001218 00003348
Time (01) (^) 9(12) 999999999999 999999999999 999999999999
Stock Code (02) X(06) 1102 1202 9934
Opening Price (02) 9(5)V9(4) 000145000 000105000 000145000
Highest Trading Price (02) 9(5)V9(4) 000148000 000108000 000148000
Lowest Trading Price (02) 9(5)V9(4) 000144000 000104000 000144000
Recent trading price (02) 9(5)V9(4) 000145000 000105000 000145000
Accumulative Trading Volume
(02)
9(8) 00003422 00005552 00005512
Time (02) (^) 9(12) 999999999999 999999999999 999999999999
． ． ． ． ． ． ．
． ． ． ． ． ． ．
Stock Code (09) X(06) 1109 1215 9938
Opening Price (09) 9(5)V9(4) 000095000 000055200 000095000
Highest Trading Price (09) 9(5)V9(4) 000098000 000058500 000098000
Lowest Trading Price (09) 9(5)V9(4) 000094000 000054500 000094000
Recent trading price (09) 9(5)V9(4) 000094000 000054500 000094000
Accumulative Trading Volume
(09)
9(8) 00001210 00006310 00004110
Time (09) (^) 9(12) 999999999999 999999999999 999999999999
Stock Code (10) X(06) 1110 1216 9939
Opening Price (10) 9(5)V9(4) 000072200 000142200 000072200
Highest Trading Price (10) 9(5)V9(4) 000073400 000143400 000073400
Lowest Trading Price (10) 9(5)V9(4) 000072100 000142100 000072100
Recent trading price (10) 9(5)V9(4) 000072300 000142300 000072300
Accumulative Trading Volume
(10)
9(8) 00001148 00003377 00008848
Time (10) (^) 9(12) 999999999999 999999999999 999999999999
Checksum X(01)
TERMINAL-CODE X(02)


Taiwan Stock Exchange

Field Name/Description Attribute Entry 93
ESC-CODE X(01)

Information Length 9(04) 0374

Business Type 9(02) 01

Transmission Format Code 9(02) 12

Transmission Format Version 9(02) 03

Transmission S/N 9(08) 00000093

Stock Entries 9(02) (^2)
Stock Code (01) X(06) 9902
Opening Price (01) 9(5)V9(4) 000045000
Highest Trading Price (01) 9(5)V9(4) 000047000
Lowest Trading Price (01) 9(5)V9(4) 000044000
Recent trading price (01) 9(5)V9(4) 000047000
Accumulative Trading Volume (01) 9(8) 00002342
Time (01) (^) 9(12) 999999999999
Stock Code (02) X(06) 000000
Opening Price (02) 9(5)V9(4) 000000000
Highest Trading Price (02) 9(5)V9(4) 000000000
Lowest Trading Price (02) 9(5)V9(4) 000000000
Recent trading price (02) 9(5)V9(4) 000000000
Accumulative Trading Volume (02) 9(8) 00000000
Time (02) (^) 9(12) 000000000000
Stock Code (03) X(06)
Opening Price (03) 9(5)V9(4) 000000000
Highest Trading Price (03) 9(5)V9(4) 000000000
Lowest Trading Price (03) 9(5)V9(4) 000000000
Recent trading price (03) 9(5)V9(4) 000000000
Accumulative Trading Volume (03) 9(8) 00000000
Time (03) (^) 9(12) 000000000000
．．．．．． ．．．．．． ．．．．．．
．．．．．． ．．．．．． ．．．．．．
Stock Code (10) X(06)
Opening Price (10) 9(5)V9(4) 000000000
Highest Trading Price (10) 9(5)V9(4) 000000000
Lowest Trading Price (10) 9(5)V9(4) 000000000
Recent trading price (10) 9(5)V9(4) 000000000
Accumulative Trading Volume (10) 9(8) 00000000
Time (10) (^) 9(12) 000000000000
Checksum X(01)
TERMINAL-CODE X(02)


Taiwan Stock Exchange

### Format 13 Real-time Odd Lot Transaction Information on Stock Market

## TWSE Data Transmission Format

Format 13: Real-time Odd Lot Transaction Information on Page: _1_

Stock Market

Length (RL): Fixed length 44 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “00 44 ”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “13”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “ 03 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 11 - 35
3.1 Stock Code X (06) 6 11 - 16 ASCII
3.2 Matching Time 9(06) 3 17 - 19 PACK BCD
3.3 Rise/Fall Remarks X(01) 1 20 - 20 BIT MAP
3.4 Trading Price 9(5)V9(4) 5 21 - 25 PACK BCD
3.5 Trading Volume 9(12) 6 26 - 31 PACK BCD
3.6 Purchasing Price 9(5)V9(4) 5 32 - 36 PACK BCD
3.7 Selling Price 9(5)V9(4) 5 37 - 41 PACK BCD
4 Checksum X(01) 1 42 - 42 XOR VALUE

5 TERMINAL-CODE X(02) (^2 43) - 44 (HEXACODE: 0D
0A)
Description
A. Trial and Official Matching: Information transmitted in Format 13 includes the trail marching
and official match trading information. Securities companies should justify the nature of
information from the Matching Time field. If it is trial matching information, only data of the
purchasing and selling prices at the best position are disclosed, and there is no trading
price/volume. If it is official match trading information, data of the purchasing and selling prices
and trading price/volume at the best position are disclosed.
B. End of Trading Information: After all information concerning match trading is transmitted, data
displayed in the Stock Order Number is “0000” and Matching Time is “999999”
C. Repeated Transmission: After the end of transmission of the match trading information of
tradable odd lots of the day, information of odd lot transactions will be re-transmitted repeatedly.
Please be noted that the trial matching information is transmitted on a real-time basis and will
not be re-transmitted.
D. Please refer to Format 13, Transmission Sequence, Appendix A for details of the transmission
sequence of information concerning odd lot transactions.
E. Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”


Taiwan Stock Exchange

(length: 1 byte).
2.3 Transmission Format Code: Expresses Format 13 in PACK BCD “13” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “0 3 ” for Version 3 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) Please refer to attachments for details.

3. BODY: (Length = 21 bytes)
3.1 Stock Code

(1) In ASCII 6 Bytes. Refer to Appendix B for stock coding rules.
(2) When the Stock Code is “00 0000 ” and Matching Time is “999999”, this means that all
match trading information of odd lot transactions for the current day have been
transmitted.
3.2 Matching Time

```
(1) Expressed in PACK BCD in the hour, minute, second (HH:MM:SS) format.
(2) Implications
a. Value<143000: Trail Matching Data
b. Value=143000: Official match trading
c. Value=999999 and Stock Code=“ 0000 00”: all match trading information of odd lot
transactions for the current day have been transmitted.
```
3.3 Rise/Fall Remarks:

```
Rise/fall remarks of Trading Price (3.4), Purchasing Price (3.5) and Selling Price are
expressed in individual bits (3.6)
```
```
Bit 7-6 Trading
00: General trading or no trading
01: Fall stop trading
10: Rise stop trading
```
```
Bit 5-4 Purchase
00: General purchasing price/volume or no purchasing price/volume
01: Fall stop purchase
10: Rise stop purchase
```
```
Bit 3-2 Selling
00: General selling price/volume or no selling g price/volume
01: Fall stop sale
10: Rise stop sale
```
Bit 1-0 Reserved

3.4 Trading Price: 5 integers and 4 decimals, totally 5 bytes after compression.

```
(1) Matching Time <143000: No trail trading price, trading price and trading volume is
displayed, and the value “0” is transmitted.
(2) Matching Time=143000: real-time transmission of trading price and volume; if none, the
value “0” is transmitted.
```

Taiwan Stock Exchange

3.5 Trading Volume: 12 integers, totally 6 bytes are compression expressed in “shares”. Other

description is the same as (1) and (2) in 3.4.

3.6 Purchasing Price: 5 integers and 4 decimals, totally 5 bytes after compression. This field

displays the purchasing price at the best position of after trial and official matching before
trading; if none, the value “0” is transmitted.
3.7 Selling Price: 5 integers and 4 decimals, totally 5 bytes after compression. This field displays

```
the selling price at the best position of after trial and official matching before trading; if none,
the value “0” is transmitted.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

Attachment: Description of Transmission S/N in Format 13
(1) Supposing that there are 612 types of tradable odd lots whose trial matching is displayed at

```
14:25:00. A total of 5011 entries are produced during the matching. The official match trading is
executed at 14:30:00, and the trading data are displayed repeatedly at every 30 seconds until
14:50.
```
(2) Data in Format 13 begin to produce at 14:25:00, and the Transmission S/N starts from

00000001. Until 14:29:59, the Transmission S/N of the last entry is 00005011.

(3) The official match trading begins at 14:30:00. After all match trading is completed, the trading

```
data of odd lots of individual stocks (Transmission S/N00005012-00005623) are displayed until
the end of trading message is transmitted (Transmission S/N 00005624).
```
(4) Data of Transmission S/N 00005012-00005624 are transmitted repeatedly every 30 seconds
until 14:50.

```
ESC-CODE HEADER BODY CHECK TERMINAL
1 2.1~2.4 2.5 3.1 3.2 3.3.~3.7 4 5
00000001 142500
00000002 142500
00000003 142501
00000004 142501
00000005 142501
.
.
.
.
```
```
.
.
.
.
00005003 142958
00005004 142958
00005005 142958
00005006 142959
00005007 142959
00005008 142959
00005009 142959
00005010 142959
00005011 142959
00005012 143000
00005013 143000
00005014 143000
00005015 143000
00005016 143000
.
.
.
.
00005619 143000
00005620 143000
00005621 143000
00005622 143000
00005623 143000
00005624 000000 999999
```
(^) End of transmission
Trial Matching
(^) Data
Match trading data
Re
peated transmission


Taiwan Stock Exchange

### Format 14 Full-name Information of Callable Bull(Bear) Contracts on the Stock Market

## TWSE Data Transmission Format

Format 14 : Full-name Information of Callable Bull(Bear) Contracts Page: _1_

on the Stock Market

Length (RL): Fixed length 69 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “00 69 ”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “14”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “ 02 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 56 11 - 66
3.1 Callable Bull(Bear) Contract Code X(06) 6 11 - 16 ASCII
3.2 Full Name of Callable Bull(Bear)
Contract

#### X( 50 ) 50 17 - 66

```
4 Checksum X(01) 1 67 - 67 XOR VALUE
5 TERMINAL-CODE X(02) 2 68 - 69 (HEXACODE: 0D 0A)
```
Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.
2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”
(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 14 in PACK BCD “14” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “ 02 ” for Version 2 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially everyday, and every format is numbered individually.

3. BODY: Totally 56 bytes to record the data of Callable Bull(Bear) Contracts.

3.1 Callable Bull(Bear) Contract Code: Expressed in ASCII code (6 bytes). See Stock coding
rules, Appendix B, for details.

3.2 Full Name of Callable Bull(Bear) Contract: Totally 50 bytes to display the full name of

```
Callable Bull(Bear) Contracts. The Full Name of Callable Bull(Bear) Contract field consists
of the following subfields.
A. Warrant(CBBC)
Abbreviation
```
```
Separation
character
B. Warrant
Target
```
```
C. Maturity
Date
```
```
D. Warrant
Form
```
```
E. Warrant
Variety
```
#### F.

```
Warrant
Type
```
#### G.

```
Reserved
Field^
TSMC
Fubon 73
Put 01
```
#### －

#### TSMC

```
20080320 Euro Put Down Blank
```

Taiwan Stock Exchange

#### TSMC

```
Fubon 75
Call 02
```
(^) －
TSMC
20080520 Am Call Up Blank
Taiwan
Index Fubon
78 Put 03
(^) －
Taiwan Index
20080820 Euro Put  Blank

#### TSMC

```
Fubon 79
Bull 04
```
(^) －
TSMC
20080920 Am Call Bull Blank

#### TSMC

```
Fubon 7B
Bear 05
```
(^) －
TSMC
20081120 Euro Put Bear Blank
Length 16 2 16 8 2 2 2 2
Subfield description (totally 50 bytes)
A. Warrant(CBBC) Abbreviation: 16 Bytes, contains the Warrant Target, Issuer, Maturity Date,
Warrant Type and S/N, the same way as the Stock Name display at present.
B. Warrant(CBBC) Target: 16 Bytes, if it is Domestic Target, then represented by the Security
Name (16 Bytes, the same way as the Stock Name display at present) or the Index Name (10
Bytes), if it is Foreign Underlying Asset, then display foreign underlying asset Type, including
"foreign securities", (including stock and depository receipts”, “overseas indexes”, and “foreign
ETF”. [ □= 2 Bytes blank]
C. Maturity Date: Western year, month and day (8 numbers).
D. Warrant(CBBC) Form: European-Euro and American-Am (1 Chinese character)
E. Warrant (CBBC)Variety: Call-Call; Put-Put (1 Chinese character).
F. Warrant(CBBC)Type:
Currently issued warrants(CBBCs): General—; Up-and-Out Callable Bull Contract—Up;
Down-and-Out Callable Bear Contract—Down; Bull (down-and-out warrant within the price)—
Bull, Bear (up-and-out warrant within the price)—Bear (1 Chinese character).
G. Reserved Field: For new warrant information in future.

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 15 Information of Suspended Stocks on the Stock Market

## TWSE Data Transmission Format

Format 15 :Information of Suspended Stocks on the Stock Market Page: _1_

Length (RL): Fixed length 20 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “0020”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “15”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 7 11 - 17
3.1 Code of Suspended Stock X(06) 6 11 - 16 ASCII
3.2 Suspension Cause Code X(01) 1 17 - 17 ASCII
4 Checksum X(01) 1 18 - 18 XOR VALUE
5 TERMINAL-CODE X(02) 2 19 - 20 (HEXACODE: 0D 0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.

2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).
2.3 Transmission Format Code: Expresses Format 15 in PACK BCD “15” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “01” for Version 1 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Data of suspended stocks on the market during the day are transmitted repeatedly before
the market opens in Format 15. Transmission stops after the market opens. All cycles
begin from “0”.
(3) When the S/N is “000000”, the Stock Code in that record represents the amount of stocks
suspended on that day.
```
3. BODY: Totally 7 bytes to record the data of suspended stocks on the day.

3.1 Stock Code

```
(1) Code of Suspended Stock: Expressed in ASCII code; totally 6 bytes. See Stock coding
rules, Appendix B for details.
(2) Stocks stored in the general stock data in the Quote Transmission Format 1 of the
previous business day that do not appear today are defined as suspended stocks.
(3) When S/N is “000000”, the amount of stocks suspended for today is displayed in the
Stock Code, and the Suspension Cause Code is in blank.
```

Taiwan Stock Exchange

3.2 Suspension Cause Code: Expressed in ASCII code, 1 byte.
T- Listing terminated
S-Transaction suspended
Blank-the amount of stocks suspended on the day is displayed in the Stock Code.

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 16 Quote Transmission System t Data on Stock Market

## TWSE Data Transmission Format

Format 16: Quote Transmission System HeartBeat Data on Page: _1_

## Stock Market

Length (RL): Fixed length 17 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10

2.1 Information Length 9(04) 2 2 - 3 PACK BCD “0017”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “16”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 4 11 - 14
3.1 System Time 9(06) 3 11 - 13 PACK BCD
3.2 System Transmission Status X(01) 1 14 - 14 ASCII
4 Checksum X(01) 1 15 - 15 XOR VALUE
5 TERMINAL-CODE X(02) 2 16 - 17 (HEXACODE: 0D 0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 16 in PACK BCD “16” (length: 1 byte).
2.4 Transmission Format Version

```
(1) Expresses version in PACK BCD, e.g. “01” for Version 1 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) HeartBeat data are transmitted once every 30 minutes, using the same S/N as that of the
Statistics Time.
```
3. BODY: A total of 4 bytes to record the present system time and system status.
3.1 System Time

```
(1) Expressed in PACK BCD, length 3 bytes, to record time in the hour, minute, second
(HH:MM:SS) format.
(2) The value recorded in this field is the system time of market quote transmission currently
at a frequency of once every 30 seconds; starting from 08:00:00 to 17:15:00. The last
HeartBeat format entry is transmitted repeatedly.
(3) If the System Time is “999999”, this means it is the last HeartBeat format entry which is
transmitted repeatedly for 5 minutes.
```

Taiwan Stock Exchange

3.2 System Transmission Status
(1) Records the present status of quote system transmission expressed in PACK BCD (1
byte).
S: Start HeartBeat information transmission
L: Normal HeartBeat information transmission
R: Restart HeartBeat information transmission
T: Terminate HeartBeat information transmission.
(2) The status of transmission of the first HeartBeat information after the quote transmission
system starts every day is “S”. When the quote network connection is normal, the status of
transmission of subsequent HeartBeat information is “L”. At present, HeartBeat
information is transmitted once every 30 seconds.
(3) When the status of transmission of HeartBeat information is “T”, this means it is the last
HeartBeat information which is transmitted repeatedly for 5 minutes. At this moment, the
quote transmission stops transmitting data in all other formats, except the last HeartBeat
information.
(4) When the status of transmission of HeartBeat information is “R”, this means the
transmission of HeartBeat information is restarted as a result of data transmission errors.
Also, interrupted HeartBeat information as a result of system errors will not be
re-transmitted.
(5) The HeartBeat information is only a reference for the status of the TWSE quote
transmission system. When the transmission is interrupted, this may suggest a connection
error but not the system failure of the quote system at the TWSE. The normal transmission
of information in other formats is subject to the receiving condition of the information
companies.
(6) See Attachment for details.

4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

Attachment: Description of Transmission S/N in Format 16
(1) Supposed that the transmission of HeartBeat information begins at 08:00:00, at a frequency of

```
once every 30 seconds. The transmission is interrupted at 10:15:3, resumes at 10:20:00 and
terminates at 17:20:00. The format of transmission data is shown below.
```
(2) The Transmission S/N of the first entry of data transmitted at 08:00:00 begins from “1”. The

```
transmission status of the first entry is “S” and changes to “L” since the second entry; and the
Transmission S/N increases according to order of entries.
```
(3) The Transmission S/N of the first entry transmitted after system recovery at 10:20:00 continues

```
from the last entry before interruption. The transmission status is “R”. HeartBeat information
interrupted as a result of transmission errors is not re-transmitted.
```
(4) After 17:15:00, the last HeartBeat information is transmitted repeatedly for about 5 minutes. At
this moment, the value displayed in System Time is “999999”, and the transmission status is
“T”.

```
ESC-CODE HEADER BODY CHECK TERMINAL
1 2.1~2.4 2.5 3 .1 3.2 4 5
```
(^00000001 080000) S
00000002 080030 L
00000003 080100 L
00000004 080130 L
00000005 080200 L
.
.
.
.
.
.
.
.
00000269 101400 L
00000270 101430 L
(^00000271 101500) L
HeartBeat information transmission interrupted.
(^00000272 102000) R
00000273 102030 L
00000274 102100 L
00000275 102130 L
.
.
.
.
.
.
.
.
00001100 171400 L
00001101 171430 L
(^00001102 171500) L
(^00001103 999999) T
00001103 999999 T
.
.
.
.
.
.
.
.
00001103 999999 T
00001103 999999 T
Normal Transmission
Last entry
repeatedTransmit
ly
system restarted.Transmission after


Taiwan Stock Exchange

### Format 17 Real-time Auction Quotes of call (put) warrant on TWSE Market.............................................

## TWSE Data Transmission Format

Format 17 : Real-time Auction Quotes of call (put) warrant on Page: _1_

TWSE Market

Length (RL): Variable Length 32 - 131 Byte
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10

```
2.1 Information Length 9(04) 2 2 - 3 PACK BCD
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
```
2.3 (^) Transmission Format Code 9(02) 1 5 - 5 PACK BCD “17”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “0 4 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 19 - 118
3.1 Stock Code X(06) 6 11 - 16 ASCII
3.2 Matching time 9 (1 2 ) 6 17 - 22 PACK BCD
3.3 Note on Disclosure item X(01) 1 23 - 23 BIT MAP
3.4 Rise/Fall Remark X(01) 1 24 - 24 BIT MAP
3.5 Status Remarks X(01) 1 25 - 25 BIT MAP
3.6 Accumulated Trading Volume 9(08) 4 26 - 29 PPACK BCD
3.7 Real-time quotes 0 - 99 30 - ?? Occurs
0 - 11
times
3.7.1 Price Field 9(05)V9(4
)

#### 5 ??-?? PACK BCD

3. 7 .2 Volume Field 9(08) 4 ??-?? PACK BCD
4 Checksum X(01) 1 ??-?? XOR VALUE
5 TERMINAL-CODE X(02) 2 ??-?? (HEXACODE: 0D 0A)

Notes: The format of this data is transmitted via the 2nd IP.

Field Description

(1) ESC-CODE: Initial byte of every record, fixed value (ASCII 27).

(2) HEADER: Information header field. The same header field is applied to all transmission
formats.
2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”
(length: 1 byte).
2.3 Transmission format code; use PACK BCD “17” (length: 1 byte)
2.4 Transmission Format Version
(1) Version 4 is represented by PACK BCD “0 4 ” (length: 1 BYTE).
(2) Versions are numbered from 1, and every format has individual version numbers.
2.5 Transmission S/N
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.

3. BODY: Variable length from 19 - 118 bytes.
    3.1 Stock Code
       (1) In ASCII 6 Bytes. Refer to Appendix B for stock coding rules


Taiwan Stock Exchange

```
(2) When the Stock Code is “0000 00 ” and the Matching Time is “999 999999 999”, this
means the last entry of real-time data of the auction of common stocks are being
transmitted.
```
```
3.2 Matching Time
(1) Expressed in PACK BCD in the format hour, minute, second,3-digit milisecond,3-digit
microsecond (HH:MM:SS:mmm:μμμ).
(2) In case of a held match (justifiable with bit 1-0 in Rise/Fall Remarks); the match held start
time is displayed in the Matching Time field.
(3) When the Stock Code is “0000 00 ” and the Matching Time is “9999 999999 99”, this means
the last real-time quote data of the auction of common stocks are being transmitted.
```
```
3.3 Disclosed Item Remarks
(1) Displayed items are expressed by bit (binary):
```
```
Bit 7 (trading price/volume)
0: No trading price/volume, no data transmitted.
1: With trading price/volume, and data transmitted.
```
```
Bit 6-4 (purchasing price/volume)
000: No purchasing price/volume, no data transmitted.
001 - 101: Display the position of stocks by purchasing price/volume (the top five
positions are displayed in binary codes).
```
```
Bit 3-1 (selling price/volume)
000: No selling price/volume, no data transmitted.
001 - 101: Display the position of stocks by selling price/volume (the top five positions
are displayed in binary codes).
```
Bit 0 (the price/quantity of the top five stocks) – 0: disclosure of the transaction prices

```
of the top 5 stocks.
1: disclosure of the transaction prices
but not the top 5 stocks.
```
Notes:

```
Individual match for transaction may result in the disclosure of individual stock
transaction price/quantity. When the price/quantity of the last transaction is disclosed, the
price/quantity for the transactions of the top 5 stocks will also be disclosed, Bit 0 = 0. If
there is no disclosure of the price/quantity of the last transaction, only the price/quantity
in the transaction will be disclosed, but not the price/quantity of the top 5 stocks, Bit 0 =
1.
```
(2) The data length of every price and volume field is 5 and 4 bytes respectively.

```
3.4 Rise/Fall Remarks
(1) Rise/fall remarks, held match instantaneous price trends, and remarks on delayed close of
the market are expressed by bit (default: 0 x 00)
```

Taiwan Stock Exchange

```
Bit 7-6: Trading rise/fall remarks
00: General trading
01: Fall stop trading
10: Rise stop trading
```
```
Bit 5-4: Optimal position purchase rise/fall remarks
00: General purchase
01: Fall stop purchase
10: Rise stop purchase
```
```
Bit 3-2: Optimal position sale rise/fall remarks
00: General sale
01: Fall stop sale
10: Rise stop sale
```
```
Bit 1-0: Instantaneous Price Trend
00: General display
01: Held match and instantaneous fall trend
10: Held match and instantaneous rise trend
11: Reserved
```
```
(2) Purchase (Sales) rise/fall remarks display only the purchase (selling) price of the stock at
the optimal position.
```
3.5 Status Remarks
(1) Trial status remarks, delayed open remarks after trial, delayed close remarks after trial and
way of matching remarks, open remarks and close remarks are expressed by individual
bit (default 0X00).

```
Bit 7 Trial status remarks
0: General display
1: Trial display
```
```
Bit 6 Delayed open remarks after trial
0: Negative
1: Positive
```
```
Bit 5 Delayed close remarks after trial
0: Negative
1: Positive
```
```
Bit 4 Way of matching remarks
0: Aggregate auction
1 : Continuous market method
```
```
Bit 3 Open remarks
0: Negative
1: Positive
```

Taiwan Stock Exchange

```
Bit 2 Close remarks
0: Negative
1: Positive
```
Bit 1-0 Reserved

```
(2) If Bit 7=1, it means at the moment real-time quote which is in the field of 3.7 is in the
Trial Status. If Bit 7=0, it means real-time quote is in the General Display Status, and in
the meantime Bit 5 and 6 remarks are of meaninglessness.
(3) If Bit 3=1, represents current open display data; if Bit 2=1, represents current close display
data.
```
3.6 Accumulative Trading Volume: transmitting the latest accumulative trading volume of

individual stock by the unit of trading.

3.7 Real-time Quotes
Transmit the latest real-time quote information of stocks by trading price/volume, top five
positions by purchasing price/volume, and top five positions by selling price/volume. Every
trading, purchasing and selling volume represents one trading unit.
(1) The length of data expressed in PACK BCD in every price and volume field is 5 and 4
bytes respectively
(2) If Bit 7 of the Disclosed Item Remarks field is 1, this means there are trading
price/volume data.
i. If Instantaneous Price Trend of Rise/Fall remark is General Display i.e. Bit 1-0 = 00,
the item displays the current price and volume.
ii. If Instantaneous Price Trend of Rise/Fall remark is Held Match i.e. Bit 1-0 = 0 1 or
10, the item will display the latest price and volume which expressed by 0; neither
the purchasing nor the selling price/volume is displayed.

```
(3) i. If Bit 6-4 of the Disclosed Item Remarks field is 001-101, this means there are
purchasing price/volume data, and the data at the top five positions and their
purchasing price/volume (sheet or unit) are transmitted in binary codes in ascending
order.
ii. If the “price filed” of purchasing the best position shows 0, it means purchasing at
market price; the “volume field” is the market purchasing volume.
iii. The best purchasing price/volume is shown in ascending order based on the purchasing
price. Market purchasing price/volume, if any, is listed at the top first position.
iv. For government bonds, only data of the consigned purchasing price/volume (unit) of
one bond are displayed; i.e. Disclosed Item Remarks Bit 6-4=001.
```
```
(4) i. If Bit 3-1 of the Disclosed Item Remarks field is 001-101, this means there are selling
price/volume data, and the data at the top five positions and their selling price/volume
(sheet or unit) are transmitted in binary codes in ascending order.
ii. If the “price filed” of selling the best position shows 0, it means sold at market price;
the “volume field” is the market selling volume.
iii. The best selling price/volume is shown in ascending order based on the selling price.
Market selling price/volume, if any, is listed at the top first position.
iv. For government bonds, only data of the consigned selling price/volume (unit) of one
bond are displayed; i.e. Disclosed Item Remarks Bit 3-1=001.
```

Taiwan Stock Exchange

```
(5) For a trial match i.e. Status Remark Bit 7 = 1, the Real-time Quotes data is at the trial
stage.
```
4 Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.

5 TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 18 Information of Auction Opening (Closing) Price of call (put) warrants on TWSE Market

## TWSE Data Transmission Format

Format 18: Information of Auction Opening (Closing) Price of Page: _1_

call (put) warrants on TWSE Market

Length (RL): Fixed length 374 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10

2.1 Information Length 9(04) 2 2 - 3 PACK BCD “0374”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “18”
2.4 Transmission Format
Version

#### 9(02) 1 6 - 6 PACK BCD “03”

```
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
```
3 BODY (^361 11) - 371
3.1 Stock Entries 9(02) 1 11 - 11 PACK BCD
3.2 Data content X(2 8 ) 360 OCCURS 10
TIMES
3.21 Stock code X(06) 6 ?-? ASC II
3.22 Opening Price 9(5)V9(4) 5 ?-? PACK BCD
3.23 Highest Trading Price 9(5)V9(4) 5 18 -? PACK BCD
3.24 Lowest Trading Price 9(5)V9(4) 5 ??-?? PACK BCD
3.25 Recent Trading Price 9(5)V9(4) 5 ??-?? PACK BCD
3.26 Accumulated Trading
volume

#### 9(8) 4 ??-?? PACK BCD

3.27 Time 9(12) 6 ??-?? PACK BCD
4 Checksum X(01) 1 372 - 372 XOR VALUE
5 TERMINAL-CODE X(02) 2
373 - 374

#### (HEXACODE: 0D

#### 0A)

Note: This data format will be transmitted via the 2nd IP. A number of price/quantity data will be

```
generated as a result of several counts of transactions at the same time. The real time express
news of the “latest bid price” and “accumulated trading volume” transmitted via format 18 is
not necessarily the final price/quantity of that point in time.
```
```
For example, if a particular stock has an offer and quantity in the X file of the order record
book, and at this point in time comes a broad lot order for purchase and the bid price can
satisfy the offer of the seller, the match will instantaneously generate X transaction
price/quantity and is the identical transaction price at the nearest point in time. The
information on the most recent transaction price/quantity pop out in this format is in real time
and can be one of the transactions from the 1st to the Xth transaction price/quantity at that
specific point in time.
```
Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission
    formats.

2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).

(2) Records of the length (byte) of the entire information, including the ESC-CODE,


Taiwan Stock Exchange

HEADER, BODY, Checksum and TERMINAL-CODE.
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission format code; use PACK BCD “18” for Format 18 (length: 1 byte)

2.4 Transmission Format Version

(1) Expresses version in PACK BCD, e.g. “0 3 ” for Version 3 (length: 1 byte).
(2) Versions are numbered from 1, and every format has individual version numbers.

2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
(3) Opening (Closing) price data are transmitted at every 10 minutes during the transactions.
Opening (Closing) price data are transmitted at every 5 minutes after the market is closed.
```
3. BODY: (Length = 361 bytes)

```
3.1 Stock Entries
(1) Expressed in PACK BCD; length 1 byte.
(2) Information includes data content and stock entries (including the entry of Stock Code =
“000000” in the end of every cycle).
(3) When there are less than 10 entries, the stock code in the Data Content field is expressed
in spaces; whiles others are in low-value.
3.2 Data Content: length 360 bytes ( 36 bytes * 10 times)
3.2.1 Stock Code
(1) Length: 6 bytes.
(2) When the value of Stock Code is “000000”, this means the cycle ends. At this
moment, the value displayed in the Trading Price, Trading Volume and Trading Time
fields is “0”.
3.2.2 Opening Price
(1) Expressed in PACK BCD, length 5 bytes, to record the auction opening price of
common stocks.
(2) Opening Price = 0 means:
a. No opening price has not come out today; or
b. The Stock Code is “000000” when the cycle ends.
3.2.3 Highest Trading Price:
(1) Expressed in PACK BCD, length 5 bytes, to record the highest trading price of the
auction of common stocks after the market opens.
3.2.4 Lowest Trading Price:
(1) Expressed in PACK BCD, length 5 bytes, to record the lowest trading price of the
auction of common stocks after the market opens.
3.2.5 Recent trading price:
(1) Expressed in PACK BCD, length 5 bytes, to record the recent trading price of the
auction of common stocks in the market.
(2) If the closing time is “9999 999999 99” when the market is closed, the closing price
of the auction of common stocks is recorded in this field.
3.2.6 Accumulative Trading Volume:
(1) Expressed in PACK BCD, length 4 bytes, to record the accumulative trading volume
(sheet) so far of the day.
(2) If the closing time is “99 999999 9999”, the total trading volume of stocks is recorded
in this field.
3.2.7 Time
```

Taiwan Stock Exchange

```
(1) Expressed in PACK BCD, length 6 bytes, to record time in the hour, minute, second ,
millisecond, microsecond(HH: MM: SS: mmm: μμμ) format.
(2) The time of the recent trading price is recorded in this field during the market is
operating.
(3) The value in this field after the market is closed is always “999 999999 999”; and the
closing price is transmitted until the transmission ends.
```
4 Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.

TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 19 Data on Stocks Suspended/Resumed for trading in TWSE

## TWSE Data Transmission Format

Format 19: Data on Stocks Suspended/Resumed for

trading in TWSE

Page: _1_

Length (RL): Fixed length 26 bytes
Order Field Name/Description Attribute Length Positio
n

```
Storage Method Note
```
1 ESC-CODE X (01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9 (04) 2 2 - 3 PACK BCD “0026”
2.2 Business Type 9 (02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9 (02) 1 5 - 5 PACK BCD “19”
2.4 Transmission Format Version 9 (02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9 (08) 4 7 - 10 PACK BCD
3 BODY 13 11 - 23
3.1 Stock Code X (06) 6 11 - 16 ASCII
3.2 Time of the day suspended for trading 9 (06) 3 17 - 19 PACK BCD
3.3 Time of the day resumed for trading 9 (06) 3 20 - 22 PACK BCD
3.4 Transmission status X (01) 1 23 - 23 ASCII
4 Checksum X (01) 1 24 - 24 XOR value
5 TERMINAL – CODE X (02) 2 25 - 26 (HEXACODE:
0D 0A)

Note:

I. Field Description:

1. ESC-CODE: the fixed value (ASCII 27) of the starting BYTE of each entry record.
2. HEADER: The Header field of the information. The same HEADER field applied each
    transmission format.
    2.1 Information length:
       (1) Represented by PACK BCD (length: 2 BYTE).
       (2) The entire length of the information entry (BYTE), including ESC-CODE,
          HEADER,BODY, checksum, and TERMINAL-CODE.
    2.2 Business type: PACK BCD “01” represents common stock transactions in TWSE
       (length: 1BYTE).
    2.3 Transmission Format Code: PACK BCD “19” represents Format 19 (length: 1
       BYTE).
    2.4 Transmission Format Version:
       (1) Version 1 is represented by PACK BCD “01” (length: 1 BYTE).
       (2) Format version will be arranged in sequential order starting from 1 and the
          format is independently codified.
    2.5 Transmission S/N:
       (1) PACK BCD represents Transmission Code (length: 4 BYTE).
       (2) Data on stocks suspended/resumed for trading will be transmitted at regular
          intervals of the day in cycles. Newly added data will be transmitted
          instantaneously. Each transmission starts from 1 in sequential order and the
          format is codified independently.
3. BODY: Total length: 13 bytes
    3.1 Stock Code:
       (1) Represented by ASCII 6 BYTES. Refer to Appendix B for stock coding


Taiwan Stock Exchange

```
rules.
(2) The record values of the time of the day for suspended/resumed trading of
stocks are represented by “999999”. The Stock Code of particular entry of
record represents the data of the quantity of the stocks suspended/resumed for
trading for such transmission and the ending of the transmission.
(3) When Stock Code is represented by “000000”, it means the full market is being
suspended for trading.
```
```
3.2 Time for suspended trade of the day:
(1) Represented by PACK BCD with length of 3 BYTES for recording the hour,
minute, and second (HHMMSS),
(2) If there is particular stock being suspended for trading on particular day, the
record of value in this field is the time that such stock is being suspended for
trading, and the field indicating the time of resumed for trading will display the
value of “999999”.
(3) When Stock Code is represented by “000000”, the record of value in this field is
the time that the full market is being suspended for trading.
```
```
3.3 Time for resumed trading of the day:
(1) Represented by PACK BCD with length of 3 BYTES for recording the hour,
minute, and second (HHMMSS),
(2) Once the stock being suspended for trading on the day is resumed for trading,
the record value shown in this field shall be the time that such stock is resumed
for trading.
(3) When Stock Code is represented by “000000”, the record of value in this field is
the time that the full market is being resumed for trading.
```
```
3.4 Transmission Status:
(1) Represented by ASCII with length of 1 BYTE. The values for the status of
transmission are defined below:
```
- C: Cyclical transmission of the data on stocks suspended/resumed for trading
    of the day.
- I: Immediate transmission of the data on stocks suspended/resumed for
    trading of the day.

```
(2) Format 19: data on stocks suspended/resumed for trading of the day will be
transmitted once every 10 minutes, which will be denoted by status C.
Whenever there are newly added data on stocks suspended/resumed for trading
of the day, data transmission will take place immediately denoted by status I.
```
4. Checksum: Calculation of the XOR value from the 2nd to the last BYTE of the
    BODY.
5. TERMINAL-CODE; the ending BYTE of each entry RECORD with fixed value of
    (HEXACODE: 0 D 0 A).


Taiwan Stock Exchange

II. Example:

1. Format 19 starts to transmit at 08:00. Assuming there is no data on stocks
    suspended/resumed for trading at the beginning of the day, and Stock 2330 and Stock
    2417 were suspended for trading at 09:12:00 and Stock 2330 resumed for trading at
    11:57:00 while Stock 2417 did not resume trading on the same day, the transmission
    of data is shown below:

```
Transmission
time
```
```
ESC-CODE HEADER BODY CHECK TERMINAL
1 2.1~2.4 2.5 3.1 3.2 3.3 3.4 4 5
09:12:00 00000001 2330 091200 999999 I
09:12:00 00000002 2417 091200 999999 I
09:12:00 00000003 000002 999999 999999 I
```
09:20:00 (^00000001 2330 091200 999999) C
09:20:00 00000002 2417 091200 999999 C
09:20:00 00000003 000002 999999 999999 C
09:30:00 00000001 2330 091200 999999 C
09:30:00 00000002 2417 091200 999999 C
09:30:00 00000003 000002 999999 999999 C

- .........
11:20:00 00000001 2330 091200 999999 C
11:20:00 00000002 2417 091200 999999 C
11:20:00 00000003 000002 999999 999999 C
11:27:00 00000001 2330 091200 115700 I
11:27:00 00000002 000001 999999 999999 I
11:30:00 00000001 2330 091200 115700 C
11:30:00 00000002 2417 091200 999999 C
11:30:00 00000003 000002 999999 999999 C
.........
17:00:00 00000001 2330 091200 115700 C
17:00:00 00000002 2417 091200 999999 C
17:00:00 00000003 000002 999999 999999 C

```
Cyclical Transmission
```
```
Cyclical
```
```
Transmission
```

Taiwan Stock Exchange

2. Format 19 starts transmission at 08:00. Assuming Stock 2330, Stock 2331, Stock 2 417 , Stock

2882 were suspended from trading at the beginning of the day and Stock 2330 resumed trading

before the beginning of transmission, Stock 2331 resumed trading at 08:12:00, Stock 2417

resumed trading at 09:43:00, and Stock 2882 did not resume trading on the day, the transmission

of data will be shown below:

```
Transmission
time
```
```
ESC-CODE HEADER BODY CHECK TERMINAL
1 2.1~2.4 2.5 3.1 3.2 3.3 3.4 4 5
08:00:00 00000001 2330 080000 080000 I
08:00:00 00000002 2331 080000 081200 I
08:00:00 00000003 2417 080000 999999 I
08:00:00 00000004 2882 080000 999999 I
08:00:00 00000005 000004 999999 999999 I
08:10:00 00000001 2330 080000 080000 C
08:10:00 00000002 2331 080000 081200 C
08:10:00 00000003 2417 080000 999999 C
08:10:00 00000004 2882 080000 999999 C
08:10:00 00000005 000004 999999 999999 C
......
09:10:00 00000001 2330 080000 080000 C
09:10:00 00000002 2331 080000 081200 C
09:10:00 00000003 2417 080000 999999 C
09:10:00 00000004 2882 080000 999999 C
09:10:00 00000005 000004 999999 999999 C
09:13:00 00000001 2417 080000 094300 I
09:13:00 00000002 000001 999999 999999 I
09:20:00 00000001 2330 080000 080000 C
09:20:00 00000002 2331 080000 081200 C
09:20:00 00000003 2417 080000 094300 C
09:20:00 00000004 2882 080000 999999 C
09:20:00 00000005 000004 999999 999999 C
......
17:00:00 00000001 2330 080000 080000 C
17:00:00 00000002 2331 080000 081200 C
17:00:00 00000003 2417 080000 094300 C
17:00:00 00000004 2882 080000 999999 C
17:00:00 00000005 000004 999999 999999 C
```
```
Cyclical Transmission
```
```
Cyclical Transmission
```

Taiwan Stock Exchange

### Format 20 Snapshot Data on Stocks for continuous trading in TWSE

## TWSE Data Transmission Format

Format 20: Snapshot Data on Stocks for continuous trading

in TWSE

Page: _1_

Length (RL): Fixed length 47 - 146 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “20”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “0 1 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 34 - 133
3.1 StockCode X (06) 6 11 - 16 ASCII
3.2 Matching Time 9(12) 6 17 - 22 PACK BCD
3.3 Disclosed Item Remarks X(01) 1 23 - 23 BIT MAP
3.4 Rise/Fall Remarks X(01) 1 24 - 24 BIT MAP
3.5 Status Remarks X(01) 1 25 - 25 BIT MAP
3.6 Opening Price 9(05)V9(4) 5 26 - 30 PACK BCD
3.7 Highest Trading Price 9(05)V9(4) 5 31 - 35 PACK BCD
3.8 Lowest Trading Price 9(05)V9(4) 5 36 - 40 PACK BCD
3.9 Accumulative Trading Volume 9(08) 4 41 - 44 PACK BCD

3. 10 Real-time Quotes 0 - 99 45 - ?? OCCURS
3. 10 .1 Price Field 9(05)V9(4) 5 ??-?? PACK BCD 0 - 11 TIMES^
3. 10 .2 Volume Field (Sheet) 9(08) 4 ??-?? PACK BCD
4 Checksum X(01) 1 ??-?? XOR VALUE
5 TERMINAL-CODE X(02) 2 ??-?? (HEXACODE: 0D
0A)

Note: A number of price/quantity data will be generated as a result of several counts of transactions

```
at the same time. The real time express news of the “latest bid price” and “accumulated
trading volume” transmitted via format 20 is not necessarily the final price/quantity of that
point in time.
```
```
For example, if a particular stock has an offer and quantity in the X file of the order record
book, and at this point in time comes a broad lot order for purchase and the bid price can
satisfy the offer of the seller, the match will instantaneously generate X transaction
price/quantity and is the identical transaction price at the nearest point in time. The
information on the most recent transaction price/quantity pop out in this format is in real time
and can be one of the transactions from the 1st to the Xth transaction price/quantity at that
specific point in time.
```
I. Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission
    formats.

2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
```

Taiwan Stock Exchange

HEADER, BODY, Checksum and TERMINAL-CODE.
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 20 in PACK BCD “ 20 ” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expressed in PACK BCD “0 1 ” for version 1 (length is 1 BYTE).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
```
3. BODY: Variable length from 34 to 133 bytes.

Market snapshots transmit the latest market data of individual stocks every five seconds. No
update transmitted if the market data remain the same.
If the market data of individual stocks do not change for too long, the latest market data will be
transmitted again within 30 seconds.
3.1 Stock Code

```
(1) Expressed in ASCII 6 BYTE. Refer to Appendix B for stock coding rules.
(2) If the Stock Code “000000” matched at time “999999999999”, it means the data on the
last entry of auction transaction of common stocks are being transmitted.
```
3.2 Matching Time
(1) Expressed in PACK BCD in the format hour: minute: second: millisecond: microsecond
(HH:MM:SS:MS:μS).
(2) In case of a held match (justifiable with bit 1-0 in Rise/Fall Remarks); the match held start
time is displayed in the Matching Time field.
(3) If the Stock Code “000000” matched at time “999999999999”, it means the data on the
last entry of auction transaction of common stocks has been transmitted.

3.3 Disclosed Item Remarks

(1) Displayed items are expressed by bit (binary):

```
Bit 7 (trading price/volume)
0: No trading price/volume, no data transmitted.
1: With trading price/volume, and data transmitted.
```
```
Bit 6-4 (purchasing price/volume)
000: No purchasing price/volume, no data transmitted.
001 - 101: Display the position of stocks by purchasing price/volume (the top five
positions are displayed in binary codes).
```
```
Bit 3-1 (selling price/volume)
000: No selling price/volume, no data transmitted.
001 - 101: Display the position of stocks by selling price/volume (the top five positions
are displayed in binary codes).
```
```
Bit 0 (Price and Volume of the best five stock transactions) –
0: Display the trading price and volume and the price and volume of the best five
stock transactions.
```

Taiwan Stock Exchange

```
1: Only display the trading price and volume, the price and volume of the best five
stock transactions are not displayed.
```
Description:

```
After the consignment trading is matched for each transaction, it may have several
trading price and volume displayed. When display the final trading price and volume,
the price and volume of the best five stock transactions are also disclosed, Bit 0 = 0.
When it is not the final trading price and volume displayed, then only display the
trading price and volume but the price and volume of the best five stock transactions
are not displayed, Bit 0 = 1.
```
(2) The data length of every price and volume field is 5 and 4 bytes respectively.

3.4 Rise/Fall Remarks

```
(1) Rise/fall remarks, held match instantaneous price trends, and delayed remarks on market
at close are expressed by bit (default: 0 x 00)
```
```
Bit 7-6: Trading rise/fall remarks
00: General trading
01: Fall stop trading
10: Rise stop trading
```
```
Bit 5-4: Optimal position purchase rise/fall remarks
00: General purchase
01: Fall stop purchase
10: Rise stop purchase
```
```
Bit 3-2: Optimal position sale rise/fall remarks
00: General sale
01: Fall stop sale
10: Rise stop sale
```
```
Bit 1-0: Instantaneous Price Trend
00: General display
01: Held match and instantaneous fall trend
10: Held match and instantaneous rise trend
11: [Reserved]
```
```
(2) Purchase (Sales) rise/fall remarks display only the purchase (selling) price of the stock at
the optimal position.
```
3.5 Status Remarks

```
(1) Trial status remarks, delayed open remarks after trial, delayed close remarks after trial and
way of matching remarks, open remarks and close remarks are expressed by individual
bit (default 0X00).
```
Bit 7 Trial status remarks


Taiwan Stock Exchange

```
0: General display
1: Trial display
```
```
Bit 6 Delayed open remarks after trial
0: Negative
1: Positive
```
```
Bit 5 Delayed close remarks after trial
0: Negative
1: Positive
```
```
Bit 4 Way of matching remarks
0: Aggregate auction
1: Continuous market method
```
```
Bit 3 Open remarks
0: Negative
1: Positive
```
```
Bit 2 Close remarks
0: Negative
1: Positive
```
Bit 1-0 Reserved

```
(2) If Bit 7=1, it means at the moment real-time quote which is in the field of 3. 10 is in the
Trial Status. If Bit 7=0, it means real-time quote is in the General Display Status, and in
the meantime Bit 5 and 6 remarks are of meaninglessness.
(3) If Bit 3=1, represents current open display data; if Bit 2=1, represents current close display
data.
```
3.6 Opening Price: Expressed in PACK BCD, length 5 bytes, to record the auction opening price

of common stocks. If Opening Price = 0 means no opening price has not come out today.
3.7 Highest Trading Price:Expressed in PACK BCD, length 5 bytes, to record the highest trading

price of the auction of common stocks after the market opens.

3.8 Lowest Trading Price:Expressed in PACK BCD, length 5 bytes, to record the lowest trading

price of the auction of common stocks after the market opens.

3.9 Accumulative Trading Volume: transmitting the latest accumulative trading volume of
individual stock by the unit of trading.

3.10 Real-time Quotes

```
Transmit the latest real-time quote information of stocks by trading price/volume, top five
positions by purchasing price/volume, and top five positions by selling price/volume. Every
trading, purchasing and selling volume represents one trading unit.
(1) The length of data expressed in PACK BCD in every price and volume field is 5 and 4
bytes respectively
(2) If Bit 7 of the Disclosed Item Remarks field is 1, this means there are trading
price/volume data.
i. If Instantaneous Price Trend of Rise/Fall remark is General Display i.e. Bit 1-0 = 00,
the item displays the current price and volume.
```

Taiwan Stock Exchange

```
ii. If Instantaneous Price Trend of Rise/Fall remark is Held Match i.e. Bit 1-0 = 01 or
10, the item will display the latest price and volume which expressed by 0; neither
the purchasing nor the selling price/volume is displayed.
```
```
(3) i. If Bit 6-4 of the Disclosed Item Remarks field is 001-101, this means there are
purchasing price/volume data, and the data at the top five positions and their
purchasing price/volume (sheet or unit) are transmitted in binary codes in ascending
order.
ii. If the “price filed” of purchasing the best position shows 0, it means purchasing at
market price; the “volume field” is the market purchasing volume.
iii. The best purchasing price/volume is shown in ascending order based on the purchasing
price. Market purchasing price/volume, if any, is listed at the top first position.
iv. For government bonds, only data of the consigned purchasing price/volume (unit) of
one bond are displayed; i.e. Disclosed Item Remarks Bit 6-4=001.
```
```
(4) i. If Bit 3-1 of the Disclosed Item Remarks field is 001-101, this means there are selling
price/volume data, and the data at the top five positions and their selling price/volume
(sheet or unit) are transmitted in binary codes in ascending order.
ii. If the “price filed” of selling the best position shows 0, it means sold at market price;
the “volume field” is the market selling volume.
iii. The best selling price/volume is shown in ascending order based on the selling price.
Market selling price/volume, if any, is listed at the top first position.
```
```
iv. For government bonds, only data of the consigned selling price/volume (unit) of one
bond are displayed; i.e. Disclosed Item Remarks Bit 3-1=001.
```
```
(5) For a trial match i.e. Status Remark Bit 7 = 1, the Real-time Quotes data is at the trial
stage.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

II. Example:

1. Let’s assume there are total 10 stocks traded in the stock market during the period of 08:30 to 13:30.
    The stock codes are, respectively, 1101, 1102, 1103...1110.
2. To optimize the transmission speed of the snapshots, the stocks will be grouped in pairs. There will be
    five groups in total. The data for each group will be transmitted at an interval of one second.
3. During the period of 09:00:01 to 09:00:05, the snapshots for ten stocks have been transmitted.
4. On 09:00:05, there is no change to the market information of stocks 1104 and 11 05. No transmission is
    made.
5. During the period of 09:00:3 1 to 09:00:3 5 the market information of stocks 1104 and 11 05 has
    remained unchanged for more than 30 seconds. The system therefore retransmitted the latest
    information, in other words, the one transmitted on 09:00:0 2 and 09:00:03.
       Transmission
          time

```
Contents of the Market Snapshot
... Transmission S/N Stock Code Matching time ...
09 :00:01 00000001 1101 090000554189
09 :00:01 00000002 1102 090000345127
09 :00:02 00000003 1103 090001543173
09 :00:02 00000004 1104 090001233189
09 :00:03 00000005 1105 090002565127
09 :00:03 00000006 1106 090001233565
09 :00:04 00000007 1107 090002554911
09 :00:04 00000008 1108 090003317345
09 :00:05 00000009 1109 090001543127
09 :00:05 00000010 1110 090003127189
09 :00:06 00000011 1101 090005233565
09 :00:06 00000012 1102 090004127189
09 :00:07 00000013 1103 090006237165
09 :00:08 00000014 1106 090005128935
09 :00:09 00000015 1107 090005549352
09 :00:09 00000016 1108 090007533145
09 :00: 10 00000017 1109 090007145523
09 :00:10 00000018 1110 090007235542
...
09 :00: 31 00000048 1101 090026549142
09 :00: 31 00000049 1102 090027514245
09 :00: 32 00000050 1104 090001233189
09 :00: 33 00000051 1105 090002565127
09 :00: 34 00000052 1107 090032543511
...
13 :33:01 00000511 1101 999999999999
13 : 33 :01 00000512 1102 999999999999
13 :33:02 00000513 1103 999999999999
13 :33:02 00000514 1104 999999999999
13 :30:03 00000515 1105 999999999999
13 :33:03 00000516 1106 999999999999
13 :33:04 00000517 1107 999999999999
13 :33:04 00000518 1108 999999999999
13 :33:05 00000519 1109 999999999999
13 :33:05 00000510 1110 999999999999
```
```
The infor
```
```
mation has remained
```
```
again.information available is transmitted unchanged for a long time. The latest
```
```
datatransmit the latest closing Market snapshots will
```
(^) of each group at an
interval of one second.
interval of one second. data of each group at an transmit the latest closing Market snapshots will
interval of one second. data of each group at an transmit the latest closing Market snapshots will
interval of one second. data of each group at an transmit the latest closing Market snapshots will


Taiwan Stock Exchange

### Format 21 Realtime Index Definition of TWSE

## TWSE Data Transmission Format

Format 21: Realtime Index Definition of TWSE Page: _1_^

Length (RL): Fixed length 116 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X (01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9 (04) 2 2 - 3 PACK BCD “0 11 6”
2.2 Business Type 9 (02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9 (02) 1 5 - 5 PACK BCD “ 21 ”
2.4 Transmission Format Version 9 (02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9 (08) 4 7 - 10 PACK BCD
3 BODY 103 11 - 113
3.1 Index code X (06) 6 11 - 16 ASCII
3.2 Index name in Chinese X ( 44 ) 44 17 - 60 ASCII
3.3 Index name in English X ( 44 ) 44 61 - 104 ASCII
3.4 Yesterday closing index 9 (0 5 )V99 4 105 - 108 PACK BCD

3. 5 Opening index time 9 (0 4 ) 2 109 - 110 PACK BCD
3. 6 Closing index time 9 (0 4 ) 2 111 - 112 PACK BCD
3. 7 Format code of transferring index 9 (0 2 ) 1 113 - 113 PACK BCD
4 checksum X (01) 1 114 - 114 XOR value
5 TERMINAL – CODE X (02) 2 115 - 116 (HEXACODE:
    0D 0A)

Note:

I. Field Description:

1. ESC-CODE: the fixed value (ASCII 27) of the starting byte of each entry record.
2. HEADER: The Header field of the information. The same HEADER field applied each
    transmission format.
    2.1 Information length:
       (1) Represented by PACK BCD (length: 2 bytes).
       (2) The entire length of the information entry (in bytes), including ESC-CODE,
          HEADER, BODY, checksum, and TERMINAL-CODE.
    2.2 Business type: PACK BCD “01” represents common stock transactions in TWSE
       (length: 1 byte).
    2.3 Transmission Format Code: PACK BCD “ 21 ” represents Format 21 (length: 1 byte).
    2.4 Transmission Format Version:
       (1) Version 1 is represented by PACK BCD “01” (length: 1 bytes).
       (2) Format version will be arranged in sequential order starting from 1 and the
          format is independently codified.
    2.5 Transmission S/N:
       (1) PACK BCD represents Transmission Code (length: 4 bytes).
       (2) Begins from 1 serially everyday, and every format is numbered individually. All
          of the realtime index definitions of TWSE are transmitted repeatedly before the
          market opens
3. BODY: Total length: 1 16 bytes
    3.1 Index Code: Represented by ASCII 6 bytes.

3.2 Index name in Chinese: Represented by ASCII 44 bytes.


Taiwan Stock Exchange

3. 3 Index name in English: Represented by ASCII 44 bytes.

3.4 Yesterday closing index: Represented by PACK BCD 4 bytes.

3. 5 Opening index time:
    (1) Represented by PACK BCD 2 bytes. Time unit is HHMM.
    ( 2 ) The value is the time that TWSE sends the opening index.
3. 6 Closing index time:
    (1) Represented by PACK BCD 2 bytes. Time unit is HHMM.
    ( 2 ) The value is the time that TWSE sends the closing index.
3. 7 Format code of transferring index:
    (1) Represented by PACK BCD 1 bytes.
    ( 2 ) The value is the format code of transferring index. If the value is “03”, it
       indicates that the realtime index is transferred by format 3: Statistics on Auction
       Indices of Common Stocks on the Stock Market. If the value is “10”, it
       indicates that the realtime index is transferred by format 10: Information of
       TWSE-compiled Indices.
4. Checksum: Calculation of the XOR value from the 2nd to the last byte of the BODY.
5. TERMINAL-CODE; the ending BYTE of each entry RECORD with fixed value of
    (HEXACODE: 0 D 0 A).


Taiwan Stock Exchange

### Format 22 Basic Data of Intraday odd lot trading Stocks

## TWSE Data Transmission Format

Format 22 : Basic Data of Intraday odd lot trading Stocks Page: _1_^

Length (RL): Fixed length 60 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD “ 60 ”
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “0 1 ”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “ 22 ”
2.4
Transmission Format
Version 9(02)^1  6 -^6 PACK BCD^ “^01 ”^
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 47 11 - 57
3 .1 Stock Code X(06) 6 11 - 16 ASCII
3.2
Stock Abbreviation in
Chinese

#### X(06) 16 17 - 32 ASCII

```
3.3 Stock Entries X(02) 2 33 - 34 ASCII
3.4 Stock Anomaly Code 9(02) 1 35 - 35 PACK BCD
3.5 Today Reference Price 9(5)V9(4) 5 36 - 40 PACK BCD
3.6 Rise Stop Price 9(5)V9(4) 5 41 - 45 PACK BCD
3.7 Fall Stop Price 9(5)V9(4) 5 46 - 50 PACK BCD
```
3. 8 Day Trading Indicator X(01) 1 51 - (^51) ASCII
3.9 Matching Cycle Seconds 9(06) 3 52 - (^54) PACK BCD
3.10 Trading Unit 9(05) 3 55 - 57 PACK BCD
4 Checksum X(01) 1 58 - 58 XOR VALUE
5 TERMINAL-CODE (^) X(02) 2 59 - 60 (HEXACODE：0D 0A)


Taiwan Stock Exchange

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

formats.

2. 1 Information Length
    (1)Expressed in PACK BCD (length: 2 bytes).
    (2)Records of the length (byte) of the entire information, including the ESC-CODE,
       HEADER, BODY, Checksum and TERMINAL-CODE

```
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD
“01” (length: 1 byte).
```
2.3 Transmission Format Code: Expresses Format 22 in PACK BCD “ 22 ” (length: 1 byte).

```
2.4 Transmission Format Version
(1)Expressed in PACK BCD “08”, where “08” means version 8 (length: 1 byte).
(2)Versions are numbered from 1, and every format has individual version numbers.
```
```
2.5 Transmission S/N
(1)Expresses Transmission S/N in PACK BCD (length: 4 bytes).
(2)The basic data of common stocks on stock market are transmitted repeatedly with Format
1 before the market opens; and the basic data of new common stocks are transmitted
repeatedly with Format 1 after the market opens. All cycles are numbered from 1.
```
3. BODY: Records general stock data, warrant(CBBC) data and foreign stock data, with a total of
    100 bytes.
3.1 Stock Code: Expressed in ASCII codes, totally 6 bytes.
Please refer to Appendix B for details of the Stock coding rules.
3.2 Stock Abbreviation: Expressed in ASCII 16 BYTEs.
3.3 Stock Entries
(1) Presented by ASCII 2 BYTE for identifying the data in the field of “Stock Code”.
(2) If the field of Stock Entries shows SPACES, the definition of “Stock Code” field
remained unchanged.
(3) Before the trading starts, the last entry of all basic information on individual stocks
being transmitted in cyclical sequence will be marked by “AL” and the “Stock Code”
field will be presented by “Total entries of Stocks”.
(4) The data transmitted in cyclical sequence during trading hours will include the stocks
added to the listing of TWSE on the same day and the last entry will be marked by “NE”
in the field of “Stock Entries” while the field of “Stock Code” will be marked as “total
new entries of stocks”.
3.4 Stock Anomaly Code
(1)Expressed in PACK BCD (length: 1 byte)
00 - Normal
01 - Attention
02 - Disposition
03 - Attention and Disposition
04 - Further Disposition
05 - Attention and Further Disposition


Taiwan Stock Exchange

```
06 - Flexible Disposition
07 - Attention and Flexible Disposition
(2)See Articles 2, 3 and 6 for attention and disposition actions in the Taiwan Stock Exchange
Corporation Directions for Announcement or Notice of Attention to Trading Information
and Dispositions
3.5 Today Reference Price
(1)Expressed in PACK BCD (length: 5 bytes)
(2)Government Bonds-0 (no rise and stop limits)
3.6 Rise Stop Price
(1)Expressed in PACK BCD (length: 5 bytes)
(2)Government Bonds-Last trading price
3.7 Fall Stop Price
(1)Expressed in PACK BCD (length: 5 bytes)
(2)Government Bonds-Last trading price
3.8 Day Trading Indicator:
Presented with ASCII 1 BYTE, its value is "A", "B" or SPACE with SPACE as the default.
A value of "A" denotes Buy first then Sell, or Sell first then Buy Day Trading Securities. A
value of "B" denotes Buy first then Sell Day Trading Securities. A value of SPACE denotes
Non-Day Trading Securities.
3.9 Matching Cycle Seconds:
6 digits number presented with PACK BCD (Length: 3 BYTE), records the matching cycle
seconds of the individual stock using aggregate auction. A value of "0" denotes the
individual stock using continuous market method.
3.10 Trading Unit:
The amount of shares for every trading unit of a stock (the amount of warrants for warrant
and beneficiary certificates for beneficiary certificate) expressed in PACK BCD (length: 3
bytes). The default value is 1000. The shares of buy/sell order is not permitted to exceed the
trade unit in the intraday odd lot trading. If the record value is 1000, this means every
trading unit is 1000 shares and maximum shares of buy/sell order is 999 in the intraday odd
lot trading. If it is 500, this means every trading unit is 500 shares and maximum shares of
buy/sell order is 499.
```
4. Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE:0D 0A.


Taiwan Stock Exchange

### Format 23 Real-time Market Data of Intraday odd lot trading stock

## TWSE Data Transmission Format

Format 23:Real-time Market Data of Intraday odd lot trading stock Page: _1_^

Length (RL): Variable Length 34 ~ 155 Bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD

2.2 (^) Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “23”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “01”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 21 - 142
3.1 StockCode X (06) 6 11 - 16 ASCII
3.2 Matching Time 9(12) 6 17 - 22 PACK BCD
3.3 Disclosed Item Remarks X(01) 1 23 - 23 BIT MAP
3.4 Rise/Fall Remarks X(01) 1 24 - 24 BIT MAP
3.5 Status Remarks X(01) 1 25 - 25 BIT MAP
3.6 Accumulative Trading Volume 9( 12 ) 6 26 - 31 PACK BCD
3.7 Real-time Quotes 0 - 121 32 - ?? OCCURS
3.7.1 Price Field 9(05)V9(4) 5 ??-?? PACK BCD 0 - 11 TIMES^
3.7.2 Volume Field (Sheet) 9( 12 ) 6 ??-?? PACK BCD
4 Checksum X(01) 1 ??-?? XOR VALUE
5
TERMINAL-CODE X(02) 2 ??-??

#### (HEXACODE: 0D

#### 0A)

Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission

```
formats.
2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”
(length: 1 byte).
2.3 Transmission Format Code: Expresses Format 23 in PACK BCD “ 23 ” (length: 1 byte).
2.4 Transmission Format Version
(1) Expressed in PACK BCD “0 1 ” for version 1 (length is 1 BYTE).
(2) Versions are numbered from 1, and every format has individual version numbers.
2.5 Transmission S/N
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
```
3. BODY: Variable length from 21 to 142 bytes.

3.1 Stock Code

```
(1) Expressed in ASCII 6 BYTE. Refer to Appendix B for stock coding rules.
(2) If the Stock Code “000000” matched at time “999999999999”, it means the data on the
last entry of auction transaction of common stocks are being transmitted.
```

Taiwan Stock Exchange

3.2 Matching Time
(1) Expressed in PACK BCD in the format hour: minute: second: millisecond: microsecond
(HHMMSSmmmμμμ).
(2) In case of a held match (justifiable with bit 1-0 in Rise/Fall Remarks); the match held start
time is displayed in the Matching Time field.
(3) If the Stock Code “000000” matched at time “999999999999”, it means the data on the
last entry of auction transaction of common stocks has been transmitted.

3.3 Disclosed Item Remarks

(1) Displayed items are expressed by bit (binary):

```
Bit 7 (trading price/volume)
0: No trading price/volume, no data transmitted.
1: With trading price/volume, and data transmitted.
```
```
Bit 6-4 (purchasing price/volume)
000: No purchasing price/volume, no data transmitted.
001 - 101: Display the position of stocks by purchasing price/volume (the top five
positions are displayed in binary codes).
```
```
Bit 3-1 (selling price/volume)
000: No selling price/volume, no data transmitted.
001 - 101: Display the position of stocks by selling price/volume (the top five positions
are displayed in binary codes).
```
Bit 0 (Reserved)

(2) The data length of every price and volume field is 5 and 6 bytes respectively.

3.4 Rise/Fall Remarks

```
(1) Rise/fall remarks, held match instantaneous price trends, and delayed remarks on market
at close are expressed by bit (default: 0 x 00)
```
```
Bit 7-6: Trading rise/fall remarks
00: General trading
01: Fall stop trading
10: Rise stop trading
```
```
Bit 5-4: Optimal position purchase rise/fall remarks
00: General purchase
01: Fall stop purchase
10: Rise stop purchase
```
```
Bit 3-2: Optimal position sale rise/fall remarks
00: General sale
01: Fall stop sale
10: Rise stop sale
```
Bit 1-0: Instantaneous Price Trend


Taiwan Stock Exchange

```
00: General display
01: Held match and instantaneous fall trend
10: Held match and instantaneous rise trend
11: (Reserved)
```
```
(2) Purchase (Sales) rise/fall remarks display only the purchase (selling) price of the stock at
the optimal position.
```
3.5 Status Remarks

```
(1) Trial status remarks, delayed open remarks after trial, delayed close remarks after trial and
way of matching remarks, open remarks and close remarks are expressed by individual
bit (default 0X00).
```
```
Bit 7 Trial status remarks
0: General display
1: Trial display
```
Bit 6 (Reserved)

Bit 5 (Reserved)

```
Bit 4 Way of matching remarks
0: Aggregate auction
1: Continuous market method
```
```
Bit 3 Open remarks
0: Negative
1: Positive
```
```
Bit 2 Close remarks
0: Negative
1: Positive
```
Bit 1- 0 (Reserved)

```
(2) If Bit 7=1, it means at the moment real-time quote which is in the field of 3.7 is in the
Trial Status.
(3) If Bit 3=1, represents current open display data; if Bit 2=1, represents current close
display data.
```
3.6 Accumulative Trading Volume: transmitting the latest accumulative trading volume of

individual stock by the shares.
3.7 Real-time Quotes

```
Transmit the latest real-time quote information of stocks by trading price/volume, top five
positions by purchasing price/volume, and top five positions by selling price/volume. Every
trading, purchasing and selling volume represents one share.
(1) The length of data expressed in PACK BCD in every price and volume field is 5 and 6
bytes respectively
(2) If Bit 7 of the Disclosed Item Remarks field is 1, this means there are trading
```

Taiwan Stock Exchange

```
price/volume data.
i. If Instantaneous Price Trend of Rise/Fall remark is General Display i.e. Bit 1-0 = 00,
the item displays the current price and volume.
ii. If Instantaneous Price Trend of Rise/Fall remark is Held Match i.e. Bit 1-0 = 01 or 10,
the item will display the latest price and volume which expressed by 0; neither the
purchasing nor the selling price/volume is displayed.
```
```
(3) i. If Bit 6-4 of the Disclosed Item Remarks field is 001-101, this means there are
purchasing price/volume data, and the data at the top five positions and their
purchasing price/volume (sheet or unit) are transmitted in binary codes in ascending
order.
ii. If the “price filed” of purchasing the best position shows 0, it means purchasing at
market price; the “volume field” is the market purchasing volume.
iii. The best purchasing price/volume is shown in ascending order based on the purchasing
price. Market purchasing price/volume, if any, is listed at the top first position.
```
```
(4) i. If Bit 3-1 of the Disclosed Item Remarks field is 001-101, this means there are selling
price/volume data, and the data at the top five positions and their selling price/volume
(sheet or unit) are transmitted in binary codes in ascending order.
ii. If the “price filed” of selling the best position shows 0, it means sold at market price;
the “volume field” is the market selling volume.
iii. The best selling price/volume is shown in ascending order based on the selling price.
Market selling price/volume, if any, is listed at the top first position.
```
```
(5) For a trial match i.e. Status Remark Bit 7 = 1, the Real-time Quotes data is at the trial
stage.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


Taiwan Stock Exchange

### Format 24 Snapshot Data on call (put) warrant for continuous trading in TWSE

## TWSE Data Transmission Format

Format 2 4 : Snapshot Data on call (put) warrant for continuous

trading in TWSE

Page: _1_

Length (RL): Fixed length 47- 146 bytes
Order Field Name/Description Attribute Length Position Storage Method Note
1 ESC-CODE X(01) 1 1 - 1 ASCII 27
2 HEADER 9 2 - 10
2.1 Information Length 9(04) 2 2 - 3 PACK BCD
2.2 Business Type 9(02) 1 4 - 4 PACK BCD “01”
2.3 Transmission Format Code 9(02) 1 5 - 5 PACK BCD “2 4 ”
2.4 Transmission Format Version 9(02) 1 6 - 6 PACK BCD “0 1 ”
2.5 Transmission S/N 9(08) 4 7 - 10 PACK BCD
3 BODY 34 - 133
3.1 StockCode X (06) 6 11 - 16 ASCII
3.2 Matching Time 9(12) 6 17 - 22 PACK BCD
3.3 Disclosed Item Remarks X(01) 1 23 - 23 BIT MAP
3.4 Rise/Fall Remarks X(01) 1 24 - 24 BIT MAP
3.5 Status Remarks X(01) 1 25 - 25 BIT MAP
3.6 Opening Price 9(05)V9(4) 5 26 - 30 PACK BCD
3.7 Highest Trading Price 9(05)V9(4) 5 31 - 35 PACK BCD
3.8 Lowest Trading Price 9(05)V9(4) 5 36 - 40 PACK BCD
3.9 Accumulative Trading Volume 9(08) 4 41 - 44 PACK BCD
3.10 Real-time Quotes 0 - 99 45 - ?? OCCURS
3.10.1 Price Field 9(05)V9(4) 5 ??-?? PACK BCD 0 - 11 TIMES^
3.10.2 Volume Field (Sheet) 9(08) 4 ??-?? PACK BCD
4 Checksum X(01) 1 ??-?? XOR VALUE
5 TERMINAL-CODE X(02) 2 ??-?? (HEXACODE: 0D
0A)

Note: A number of price/quantity data will be generated as a result of several counts of transactions

```
at the same time. The real time express news of the “latest bid price” and “accumulated
trading volume” transmitted via format 2 4 is not necessarily the final price/quantity of that
point in time.
```
```
For example, if a particular stock has an offer and quantity in the X file of the order record
book, and at this point in time comes a broad lot order for purchase and the bid price can
satisfy the offer of the seller, the match will instantaneously generate X transaction
price/quantity and is the identical transaction price at the nearest point in time. The
information on the most recent transaction price/quantity pop out in this format is in real time
and can be one of the transactions from the 1st to the Xth transaction price/quantity at that
specific point in time.
```
Field Description

1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission
    formats.

2.1 Information Length

```
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
```

Taiwan Stock Exchange

HEADER, BODY, Checksum and TERMINAL-CODE.
2.2 Business Type: Expresses common stock transactions on stock market in PACK BCD “01”

(length: 1 byte).

2.3 Transmission Format Code: Expresses Format 24 in PACK BCD “ 24 ” (length: 1 byte).

2.4 Transmission Format Version

```
(1) Expressed in PACK BCD “ 01 ” for version 1 (length is 1 BYTE).
(2) Versions are numbered from 1, and every format has individual version numbers.
```
2.5 Transmission S/N

```
(1) Expressed in PACK BCD (length: 4 bytes).
(2) Begins from 1 serially every day, and every format is numbered individually.
```
3. BODY: Variable length from 34 to 133 bytes.

```
Market snapshots transmit the latest market data of individual stocks every five seconds. No
update transmitted if the market data remain the same.
```
3.1 Stock Code

```
(1) Expressed in ASCII 6 BYTE. Refer to Appendix B for stock coding rules.
(2) If the Stock Code “000000” matched at time “999999999999”, it means the data on the
last entry of auction transaction of common stocks are being transmitted.
```
3.2 Matching Time

```
(1) Expressed in PACK BCD in the format hour: minute: second: millisecond: microsecond
(HH:MM:SS:MS:μS).
(2) In case of a held match (justifiable with bit 1-0 in Rise/Fall Remarks); the match held start
time is displayed in the Matching Time field.
(3) If the Stock Code “000000” matched at time “999999999999”, it means the data on the
last entry of auction transaction of common stocks has been transmitted.
```
3.3 Disclosed Item Remarks

(1) Displayed items are expressed by bit (binary):

```
Bit 7 (trading price/volume)
0: No trading price/volume, no data transmitted.
1: With trading price/volume, and data transmitted.
```
```
Bit 6-4 (purchasing price/volume)
000: No purchasing price/volume, no data transmitted.
001 - 101: Display the position of stocks by purchasing price/volume (the top five
positions are displayed in binary codes).
```
```
Bit 3-1 (selling price/volume)
000: No selling price/volume, no data transmitted.
001 - 101: Display the position of stocks by selling price/volume (the top five positions
are displayed in binary codes).
```
```
Bit 0 (Price and Volume of the best five stock transactions) –
0: Display the trading price and volume and the price and volume of the best five
stock transactions.
1: Only display the trading price and volume, the price and volume of the best five
stock transactions are not displayed.
```

Taiwan Stock Exchange

Description:

```
After the consignment trading is matched for each transaction, it may have several
trading price and volume displayed. When display the final trading price and volume,
the price and volume of the best five stock transactions are also disclosed, Bit 0 = 0.
When it is not the final trading price and volume displayed, then only display the
trading price and volume but the price and volume of the best five stock transactions
are not displayed, Bit 0 = 1.
```
(2) The data length of every price and volume field is 5 and 4 bytes respectively.

3.4 Rise/Fall Remarks

```
(1) Rise/fall remarks, held match instantaneous price trends, and delayed remarks on market
at close are expressed by bit (default: 0 x 00)
```
```
Bit 7-6: Trading rise/fall remarks
00: General trading
01: Fall stop trading
10: Rise stop trading
```
```
Bit 5-4: Optimal position purchase rise/fall remarks
00: General purchase
0 1: Fall stop purchase
10: Rise stop purchase
```
```
Bit 3-2: Optimal position sale rise/fall remarks
00: General sale
01: Fall stop sale
10: Rise stop sale
```
```
Bit 1-0: Instantaneous Price Trend
00: General display
01: Held match and instantaneous fall trend
10: Held match and instantaneous rise trend
11: [Reserved]
```
```
(2) Purchase (Sales) rise/fall remarks display only the purchase (selling) price of the stock at
the optimal position.
```
3.5 Status Remarks
(1) Trial status remarks, delayed open remarks after trial, delayed close remarks after trial and
way of matching remarks, open remarks and close remarks are expressed by individual
bit (default 0X00).

```
Bit 7 Trial status remarks
0: General display
1: Trial display
```

Taiwan Stock Exchange

```
Bit 6 Delayed open remarks after trial
0: Negative
1: Positive
```
```
Bit 5 Delayed close remarks after trial
0: Negative
1: Positive
```
```
Bit 4 Way of matching remarks
0: Aggregate auction
1: Continuous market method
```
```
Bit 3 Open remarks
0: Negative
1: Positive
```
```
Bit 2 Close remarks
0: Negative
1: Positive
```
Bit 1- 0 Reserved

```
(2) If Bit 7=1, it means at the moment real-time quote which is in the field of 3.10 is in the
Trial Status. If Bit 7=0, it means real-time quote is in the General Display Status, and in
the meantime Bit 5 and 6 remarks are of meaninglessness.
(3) If Bit 3=1, represents current open display data; if Bit 2=1, represents current close display
data.
```
3.6 Opening Price: Expressed in PACK BCD, length 5 bytes, to record the auction opening price

of common stocks. If Opening Price = 0 means no opening price has not come out today.

3.7 Highest Trading Price:Expressed in PACK BCD, length 5 bytes, to record the highest trading

price of the auction of common stocks after the market opens.
3.8 Lowest Trading Price:Expressed in PACK BCD, length 5 bytes, to record the lowest trading

price of the auction of common stocks after the market opens.

3.9 Accumulative Trading Volume: transmitting the latest accumulative trading volume of

individual stock by the unit of trading.

3.10 Real-time Quotes
Transmit the latest real-time quote information of stocks by trading price/volume, top five
positions by purchasing price/volume, and top five positions by selling price/volume. Every
trading, purchasing and selling volume represents one trading unit.
(1) The length of data expressed in PACK BCD in every price and volume field is 5 and 4
bytes respectively
(2) If Bit 7 of the Disclosed Item Remarks field is 1, this means there are trading
price/volume data.
i. If Instantaneous Price Trend of Rise/Fall remark is General Display i.e. Bit 1-0 = 00,
the item displays the current price and volume.
ii. If Instantaneous Price Trend of Rise/Fall remark is Held Match i.e. Bit 1-0 = 01 or
10, the item will display the latest price and volume which expressed by 0; neither
the purchasing nor the selling price/volume is displayed.


Taiwan Stock Exchange

```
(3) i. If Bit 6-4 of the Disclosed Item Remarks field is 001-101, this means there are
purchasing price/volume data, and the data at the top five positions and their
purchasing price/volume (sheet or unit) are transmitted in binary codes in ascending
order.
ii. If the “price filed” of purchasing the best position shows 0, it means purchasing at
market price; the “volume field” is the market purchasing volume.
iii. The best purchasing price/volume is shown in ascending order based on the purchasing
price. Market purchasing price/volume, if any, is listed at the top first position.
iv. For government bonds, only data of the consigned purchasing price/volume (unit) of
one bond are displayed; i.e. Disclosed Item Remarks Bit 6-4=001.
```
```
(4) i. If Bit 3-1 of the Disclosed Item Remarks field is 001-101, this means there are selling
price/volume data, and the data at the top five positions and their selling price/volume
(sheet or unit) are transmitted in binary codes in ascending order.
ii. If the “price filed” of selling the best position shows 0, it means sold at market price;
the “volume field” is the market selling volume.
iii. The best selling price/volume is shown in ascending order based on the selling price.
Market selling price/volume, if any, is listed at the top first position.
```
```
iv. For government bonds, only data of the consigned selling price/volume (unit) of one
bond are displayed; i.e. Disclosed Item Remarks Bit 3-1=001.
```
```
(5) For a trial match i.e. Status Remark Bit 7 = 1, the Real-time Quotes data is at the trial
stage.
```
4. Checksum: Checksum: Calculates the XOR VALUE the second to the last bytes of the BODY.
5. TERMINAL-CODE: The ending byte of every record, default is HEXACODE: 0D 0A.


```
Taiwan Stock Exchange
```
### Format 25 Available Balance for Securities borrowing and lending

## TWSE Data Transmission Format

Format 25: Available Balance for Securities borrowing and lending^ Page: 1_

Length (RL): Fixed length 32 bytes

Order Field Name/Description Attribute Length Position Storage Method Note

1 ESC-CODE X (01) 1 1 - 1 ASCII 27

2 HEADER 9 2 - 10

2.1 Information Length 9 (04) 2 2 - 3 PACK BCD

2.2 Business Type 9 (02) 1 4 - 4 PACK BCD “01”

2.3

```
Transmission Format
Code
```
9 (02) 1 5 - 5 PACK BCD “25”

2.4

```
Transmission Format
Version
9 (02) 1 6 - 6 PACK BCD “01”
```
2.5

```
Transmission S/N
9 (08) 4 7 - 10 PACK BCD
```
3 BODY 19 11 - 29

3.1 Update Time 9 (12) 6 11 - 16 PACK BCD

3.2 Stock Code X (06) 6 17 - 22 ASCII

3.3

```
Available balance for
Securities borrowing
and lending
```
9 (14) 7 23 - 29 PACK BCD

4 Checksum X (01) 1 30 - 30 XOR Value

5 TERMINAL-CODE X (02) 2 31 - 32 (HEXACODE: 0D 0A)

```
Description
I. Field Description:
```
1. ESC-CODE: Initial byte of every record, fixed value (ASCII 27).
2. HEADER: Information header field. The same header field is applied to all transmission
formats.

```
2.1 Information Length
(1) Expressed in PACK BCD (length: 2 bytes).
(2) Records of the length (byte) of the entire information, including the ESC-CODE,
HEADER, BODY, Checksum and TERMINAL-CODE.
```
2.2 Business Type: Common stock transactions on stock market is expressed in PACK BCD


Taiwan Stock Exchange

“01” (length: 1 byte).

2.3 Transmission Format Code: Format 25 is expressed in PACK BCD “25” (length: 1 byte).

```
2.4 Transmission Format Version:
(1) Expressed in PACK BCD “01”, where “01” means version 1 (length: 1 byte).
(2) Versions are numbered from 1, and every format has an individual number.
```
```
2.5 Transmission S/N:
(1) The transmission S/N is expressed in PACK BCD (length: 4 bytes).
(2) Serial numbers are assigned in sequence from 1 every day, and each format is
numbered independently.
```
3. BODY: A total length of 19 bytes.
    The available balance for Securities borrowing and lending(SBL) will be transmitted every
five seconds, but will not be transmitted if there is no change in the information. If the
individual stock information has not changed for an extended time, then the latest
information will be retransmitted within 30 seconds.

```
3.1 Update time: 12 digits, expressed in PACK BCD (length: 6 bytes).
(1) The update time is the system time for checking the available balance for SBL, in the
format of hour, minute, second, millisecond and microsecond(HHMMSSmmmμμμ).
(2) The "available balance for securities borrowing" is transmitted in cycle before the
transaction, and the update time is "000000000000". Field 3.3 indicates the individual
stock’s available balance for securities borrowing; if the stock code is "000000" and the
update time is "000000000000", then field 3.3 indicates the total number of stocks
transmitted in cycle.
(3) If the stock code is "000000" and the update time is " 9 99999999999", it means that the
last quotation data of the available balance for SBL has been sent out.
```
```
3.2 Stock Code:
(1) Expressed in ASCII 6 bytes. Please refer to Appendix II for securities coding rules.
(2) The "available balance for SBL" is transmitted in cycle before the transaction, and the
update time is " 000000000000 ". Field 3.3 indicates the individual stock’s available
balance for SBL; if the stock code is "000000" and the update time is "000000000000",
then field 3.3 indicates the total number of stocks transmitted in cycle.
```

Taiwan Stock Exchange

```
(3) If the stock code is "000000" and the update time is " 9 99999999999", it means that the
last quotation data of the available balance for SBL has been sent out.
```
3.3 Available balance for SBL: 14 digits, expressed in PACK BCD (length: 7 bytes).

4. Checksum: Calculates the XOR VALUE from the second to the last byte of the BODY.

```
5. TERMINAL-CODE: The ending byte of every record, with the fixed value of
HEXACODE:0D 0A.
```

Taiwan Stock Exchange

II. Example:

1. Assume that there are 10 stocks in the stock market from 07:30 to 17:00, with stock codes of
    1101, 1102, 1103......1110 respectively.
2. Before 08:00, the available balance for SBL is transmitted in cycle, and the update time is
    "000000000000". If the stock code is "000000", it indicates the total number of stocks
    transmitted in cycle.
3 During the period of Market Data transmission, the available balance for SBL will be
    transmitted every five seconds, but will not be transmitted if there is no change in the
    information. If the individual stock information is not changed for a long time, then the
    latest information will be retransmitted within 30 seconds.
4. From 08:00:01 to 08:00:05, the information about the available balance for SBL of stocks
    1101, 1102, 1103, 1104, 1105, 1107, 1108 and 1109 after the change is transmitted.
5. If the stock information of 1106 and 1110 transmitted at 07:59:33 and 07:59:35 has not
    changed for more than 30 seconds, then the latest available balance for SBL will be
    retransmitted at 08:00:03 and 08:00:05.
6. At 17:00:00, the last quotation data of the stock "000000" is transmitted, and the update time
    is "9999999999 99 ".


Taiwan Stock Exchange

```
Transmission
time
```
```
Market snapshot information content
```
```
...
Transmission
S/N
Stock code Update time
Available balance for
SBL
07:30:01 00000001 1101 000000000000 6350000
07:30:01 00000002 1102 000000000000 2200000
07:30:02 00000003 1103 000000000000 600000
07:30:02 00000004 1104 000000000000 1250000
07:30:03 00000005 1105 000000000000 50000
07:30:03 00000006 1106 000000000000 350000
07:30:04 00000007 1107 000000000000 650000
07:30:04 00000008 1108 000000000000 850000
07:30:05 00000009 1109 000000000000 4750000
07:30:05 00000010 1110 000000000000 1650000
07:30:05 00000011 000000 000000000000 10
...
07:59:31 00000650 1101 000000000000 6350000
07:59:31 00000651 1102 000000000000 2200000
07:59:32 00000652 1103 000000000000 600000
07:59:32 00000653 1104 000000000000 1250000
07:59:33 00000654 1105 000000000000 50000
07:59:33 00000655 1106 000000000000 350000
07:59:34 00000656 1107 000000000000 650000
07:59:34 00000657 1108 000000000000 850000
07:59:35 00000658 1109 000000000000 4750000
07:59:35 00000659 1110 000000000000 1650000
07:59:35 00000660 000000 000000000000 10
...
08:00:01 00000661 1101 080001011234 6325000
08:00:01 00000662 1102 080001102256 2189000
08:00:02 00000663 1103 080002031245 587000
08:00:02 00000664 1104 080002126575 1114000
08:00:03 00000665 1105 080003023325 30000
08:00:03 00000666 1106 000000000000 350000
08:00:04 00000667 1107 080004111125 623000
08:00:04 00000668 1108 080004201147 810000
08:00:05 00000669 1109 080005211005 4723000
08:00:05 00000670 1110 000000000000 1650000
08:00:07 00000672 1103 080007232239 586500
08:00:09 00000672 1108 080009319875 725000
...
17:00:00 00012359 000000 999999999999 0
```
```
balance for SBLBefore the start, the available
```
(^) will be transmitted
in cycle every 30 seconds.
The available balance for SBL
second.of one group is transmitted per
will be retransmitted.for 30 seconds, the latest one If the data has not changed
transferred Total number of stocks
in cycle
quotation dataTransmission of the last
The latest available balance for SBL
of ea
ch stock is transmitted every
five seconds.


Taiwan Stock Exchange

## 4. Version Update Log

## Data Format Version Update Log

Data/Version/Data Format Format 1 Format 2 Format 3 Format 4 Format 5 Format 6 Format 7 Format 8 Format 9

```
2002/7/1 1 1 1 1 1 1 1 1 1
```
```
2003/5/5 2
```
```
2004/1/3 2
```
```
2008/12/8 3
```
```
2009/07 4
```
```
2010/07 2 2
```
```
20 11/03 5 2 2
```
```
2011/10 6
```
```
2015/06 3
```
```
2015/07 7
```
```
2 020/03 8 4 3
```
```
2 021/06 9 3 3
```
Data/Version/Data Format
Format
10

```
Format
11
```
```
Format
12
```
```
Format
13
```
```
Format
14
```
```
Format
15
```
```
Format
16
```
```
Format
17
```
```
Format
18
```
```
2002/7/1 1
```
```
2003/ 6 1
```
```
2003/12 1
```
```
2005/09 1
```
```
2008/07 1 1 1
2009/07 1 1
```
```
2010/07
```
```
2011/03 2 2
```
```
2015/06 2 3 2
```
```
2016/06 2
```
```
2 020/03 3 3 4 3
```
Data/Version/Data Format
Format
19

```
Format
20
```
```
Format
21
```
```
Format
22
```
```
Format
23
```
```
Format
24
```
```
Format
25
2011/03 1
```
```
2019/12 1
```
2 020/03 (^1  1 1)


Taiwan Stock Exchange

2 022/06 (^1 1)


Taiwan Stock Exchange

## Information Transmission Manual Update Log

```
Version Date Update Contents
21/02/99 Changed index calculation cycle from 5 minute to 1 minute once.
B.03 19/08/99 Format 5: In addition to general announcements, the transmission of
emergency announcements was added, using S/N to classify general and
emergency announcements: general announcement S/N began from “1” and
emergency announcement S/N from “9001”.
B.04 28/01/00 Defines the transmission rules of after-hour fixed-price transaction
information. No format was changed, except the addition of display time and
description of meanings of information in different fields in all formats.
B.04.01 18/05/00 (1) Format 1: Added Stock Disposition Code (using the reserved bit)
(2) Format 4: Produced statistics on the intraday consignment volume
every minute.
(3) Added government bond transaction information (originally Formats 1
and 6).
(4) No format was changed; except the addition of display time and
description of meanings of information in different fields in all formats.
B.04.02 24/07/00 Revised part of Format 6 to ensure every entry of data transmitted in Format
6 contain at the same time the purchasing, selling and trading prices at the
same time to enhance information transformation efficiency.
B.04.03 12/10/00 Revised part of Format 7 to display the weighted index of non-electronic
industry stocks in terms of 20 index items.
B.04.04 06/08/01 Revised Format 6 on 17 April 2001) to display purchasing, selling and
trading prices and volume (sheet). The revised version was implemented
officially on 6 August 2001.
B.05 01/07/02 1. Coordinating with the instantaneous price stabilizing policy, Format 6
was revised to display held match information and the best
purchasing/selling price/volume. The revised Format 6 was implemented
officially on 1 July 2002.
```
2. Coping with the needs of displaying the top five positions and
    high-speed information transmission, in addition to adding the HEADER
    field, the original formats 1-8 were combined to formats 1-6. The policy
    was implemented in the beginning of 2003.
B.05.01 02/06/03 1. The Stock Industry Category Cross-reference Table was revised to cope
with the new stock coding rules.
2. As some stocks contain meanings that cannot be identified directly from
    the stock code (e.g. call/Callable Bear Contracts which were classified
    into proportional and un-proportional call/Callable Bear Contracts based
    on the original conversion ratio), the Stock Category field was added to
    identify such stocks.
3. The Strike Ratio field in Format 1 was enlarged.
4. Description of Statistics Time=999999 in formats 2 and 3 was revised.
5. The ETF Net Value field was added to Format 11.


Taiwan Stock Exchange

```
B.05.02 12/03 Added Format 12 Information of Auction Opening (Closing) Price of
Common Stocks on Stock Market.
B.05.03 20/10/04 1. Revised the government bond description in formats 1 and 6 and
Appendix B and Appendix C: Stock coding rules to cope with the
transaction rule change and exchange bond issue of government bonds.
```
2. Corrected text description in Format 10.
B.05.04 Revised on 03/01/05
Implemented on
01/03/05
1. Revised relevant descriptions in Format 3 after the issue of the
Non-Financial and Non-Electronic Stocks Issuing Volume Weighted
Index.
2. Revised the stock coding rules of Call/Callable Bear Contracts.
3. Revised the stock coding rules and descriptions of Government Bonds.
B.05.05 Revised on 29/04/05
Implemented in June
2004

```
Revised the description of Stock Anomaly Code and added flexible
disposition judgment code.
```
```
B.05.06 Revised on 5/9/05
Implemented on
2/1/06
```
```
Revised the description in Format 3 for the termination of producing and
transmitting Integrated Stock Price Average and Industrial Stock price
Average.
B.06 Revised on 23/9/05
Implemented on
19/12/05
```
```
Added Format 13 Odd Lot Transaction Information on Stock Market.
```
```
B.06.01 Revised on 15/9/06
Implemented in 1/07
```
```
Updated Index Code Table, Appendix 5; added TWEI (Taiwan Eight
Industries Index) and TWDP (Taiwan Dividend Plus Index).
B.07 Revised on 12/01/07
Implemented in 7/07
```
```
Adjusted the sequence of transmission index items and fields in Format 3 to
cope with the re-allocation and adjustment of the industry categorization of
listed companies.
B.08 Revised on 07/07/08
Implemented in
October 2008
```
1. Revised Format 1 and added Stock Category field and definitions to cope
    with the listing of foreign enterprises in the TWSE.
2. Added the Full Name of Callable Bull(Bear) Contract data on stock
    market in Format 14.
3. Added Format 15 Information of Suspended Stocks on the Stock Market.
4. Added Format 16 Data Transmission System HeartBeat Data on Stock
    Market.
5. Cancelled the transmission of Format 11 and removed the format from
    the specification since the implementation of ETF net value calculation
    by investment trust.
B.08.01 Revised in December
2008
Implemented in
March 2009

```
Revise the message length, format version, strike price and exercise rate
fields in Format I for indexed warrants.
```
```
B.08.02 Revised in April 2009
Implemented in July
2009
```
```
Revise the definition of the offshore stock trading volume in Format I as the
units in each stock transaction in the centralized market in supporting the
trading of overseas ETF in less than 1,000 units as a trading lot in the future.
The definitions of trading and trade order volume from “lot (1,000 shares) to
“quantity” in Format II, IV, VI, VII, VIII, IX, and XII. The quantity of trade
is the trading units of stock.
```

Taiwan Stock Exchange

B. 09 Revised on 98/07 1. Revision of Appendix B on the Rules for Stock Code for the addition of
subscription warrants.

2. Revision of Format 1, with the addition of Format 17 and Format 18 for
    the transaction of subscription warrants by transaction.
    (1) Revision of the message length, format version field of market
       information transmission Format 1, with the addition of the field for
       notes on market information line.
    (2) Addition of Format 17, 2nd IP on real time market information on
       trading of common stocks in the centralized market though bidding.
    (3) Addition of Format 18, 2nd IP on information on price at opening
       (close) on the trading of common stocks in the centralized market
       through bidding.
    (4) Addition of Attachment 6 in the table of Online Transmission Format.
B.09. 01
Revised in November
2009
Implemented in
December 2009

```
Revise Appendix B on the rules of stock code in supporting the addition of
product code for beneficiary certificates, ETF and TDR products.
```
```
B.09.02 Implemented in April
2010
```
```
In the field “Corresponding Stock Code Sequential Number” of format 1, 6,
9, and 17, the method of using the sequential number is changed. When a
specific stock was delisted, its code will be used by another stock when
listing on the TSE.
B.09.03 Effective May 2010 Amendment to the attached Securities Codification Rules and notes to the
fields of Format XIV in line with the newly added “Callable Bull(Bear)
Contracts with foreign underlying assets” products and “extension of the
codification of warrants”.
B.09.04 Implemented in
January 2011
```
```
Transmission rate of newly added format II and IV statistics and revised
format II, III, and IV is adjusted to 15 seconds in responding to the
“disclosure of index and consigned trade statistical information at 15
seconds” and “ type of transaction and statistical information “.
B.09.05 Revised on January
2011
Implemented in
March 2011
```
```
Updated version of Appendix E “TWSE-Compiled Index Table” with the
addition of “EMP99, Taiwan Employment 99 Index”
```
```
B.10 Revised in March
2011
Implemented in
August 2011
```
1. The field of “Stock Order Number” in Format 1, Format 6, Format 9 ,
    Format 13, and Format 17 has been adjusted, and replaced by “Stock
    Code”.
2. Format 6 has been altered with the addition of trial calculation on
    matching and disclosure of delayed close of market.
3. Addition of Format 19- “Data on stocks suspended for trading” and
    adjustment of Appendix F in supporting the function of
    suspended/resumed trade during the trading hours.
4. Adjustment of Appendix B – “Stock coding rules” with the addition of
    Callable Bull/Bear Contracts”.
B.10.01 Revised in October
2011
Implemented in
January 2012
1. In order to accommodate the disclosures of “Stocks under the influences
of abnormal recommendations on cable television”, “Abnormal
securities”, and “Foreign companies’ first listings on TSE with face
values other than $10 per share”, three new fields were added to Format
1, namely 3.1.10 – “Non-$10 face value indicator”, 3.1.11 – “Abnormal
recommendation indicator”, and 3.1.12 – “Abnormal securities
indicator”. The version of the revised format was adjusted to 06, with
record length adjusted to 76 Bytes.
2. Adjusted Appendix B: Stock Coding Rules
B.10.02 Revised in October
2012
Implemented in
December 2012

```
Adjusted Appendix E: TWSE-Compiled Index Table increasing Taiwan
RAFI(r) CO101 Index
```

Taiwan Stock Exchange

```
B.10.03 Revised in January
2013
```
```
Following stock codification rules change, amending
convertible bonds, exchangeable corporate bonds, and
exchangeable financial bonds codes.
```
```
B.10.04 Revised in January
2014
Implemented in
February 2014
```
```
Format 2, 3, 4, and 10 have been amended for transmitting
frequence change from 15 seconds to 10 seconds.
```
```
B.10.05 Revised in March
2014
Implemented in May
2014
```
```
Adjusted Appendix E: TWSE-Compiled Index Table adding FORMOSA
Index
```
```
B.10.06 Revised in June
2014
Implemented in
August 2014
```
```
Adjusted Appendix E: TWSE-Compiled Index Table adding Taiwan HC
100 Index
```
```
B.10.07 Revised in July
2014
Implemented in July
2014
```
```
With the revision of “Principles for Assignment of Ticker Symbols in the
Republic of China Securities Markets,” Open-end Callable Bull and Bear
Contracts, leveraged ETFs, inverse ETFs, and futures trust ETFs are added
to Appendix II: Principles for Assignment of Ticker Symbols.
B.10.08 Revised in December
2014
Implemented in
December 2014
```
```
Format 2, 3, 4, and 10 have been amended for transmitting
frequence change from 10 seconds to 5 seconds.
```
B.10.09 (^) Revised in January
2015
Implemented in
April 2015
Revised in April
2015
Implemented in
June 2015

1. The definition in the fields of Foreign Stock ID and Trading
Currency Code in format 1 has been adjusted.(implemented in
April 2015)
2. To remove the item of Foreign Stocks from Appendix B.
(implemented in April 2015)
3. To increase Appendis G – Trading Currency Code.
(implemented in April 2015)
4. To revise Appendix E-TWSE-Compiled Index Table, adding
2 kinds of index of “TWSE Electronics Daily Return Leveraged
2X Index” and “TWSE Electronics Daily Return Inverse -1X
Index”
B.1 1 Revised in January
2015
Revised in April
2015
Implemented in June
2015
1. Formats 6,12,17,and 18 have been amended, focusing on
disclosing trial trading price/volume and 5 best bid/ask
price/volume data before market open and close.
2. Format 19 has been amended with adding the definition of the
Full Market Suspension.
3. To revise Appendix E-TWSE-Compiled Index Table, adding
an index of “TWSE Corporate Governance 100 Index” and
deleting “TWSE RAFI® Taiwan Corporate Operation 101
Index”.


Taiwan Stock Exchange

```
B.11.01 Revised in July 2015
```
```
Implemented in
```
```
September 2015
```
```
Implemented in
```
```
October 2015
```
1. Updated Appendix E "TWSE Compiled Index Table", added "TAIEX

```
Leveraged 2X Index" and "TAIEX Inversed - 1X Index". (Implemented in
```
```
September 2015)
```
2. Revised Format 1, Expanded the field length of the stock abbreviation in

```
Chinese, added fields of " Day Trading Indicator", "Exemption of
```
```
Unchanged Market Margin Sale Indicator", "Exemption of Unchanged
```
```
Market Securities Lending Sale Indicator", "Matching Cycle Seconds",
```
```
"Upper Limit Price", "Lower Limit Price" and "Maturity Date".
```
```
(Implemented in October 2015)
```
3. Adjusted Format 6, 17 "Status Remarks" Field Definition, added

```
Disclosure Open Close Remarks. (Implemented October 2015).
```
```
B.11.02 Revised in November
```
```
2015.
```
```
Implemented in
```
```
December 2015.
```
## Updated Appendix E "TWSE Compiled Index Table", added " TWSE

```
TAIEX Small-Cap 300 Sub-Index ". (Implemented in December 2015)
```
```
B.11.03 Revised in December
```
```
2015.
```
```
Implemented in
```
```
January 2016.
```
```
Updated Appendix E "TWSE Compiled Index Table", added "Finance
```
```
Leveraged 2X Index " and “Finance Inverse-1X Index”. (Implemented in
```
```
January 2016 )
```
```
B.11.04 Revised in March
```
```
2016, Implemented in
```
```
June 2016.
```
Coordinating with the securities abbreviation bytes expansion

program, update the Field Description of the "Full-name

Information of Callable Bull (Bear) Contracts on the Stock

Market" (Format 14). (Implemented in June 2016)

```
B.11.05 Revised in July 2016,
```
```
Implemented in July
```
```
2016.
```
Due to adding a new Index of “TIP TAIEX+ Dividend

Appreciation 150 Index”, appendix 5 of TWSE-Compiled

Index Table will be updated accordingly. (Implemented in

July 2016)

```
B.11.06 Revised in August
```
```
2016, Implemented in
```
```
August 2016.
```
Due to adding a new Index of “TIP TAIEX+ Dividend

Appreciation 100 Index”, appendix 5 of TWSE-Compiled

Index Table will be updated accordingly. (Implemented in

August 2016)


Taiwan Stock Exchange

```
B.11.07 Revised in December
```
```
2016, Implemented in
```
```
January 2017.
```
```
Due to adding 6 new Indices of “TIP TAIEX+ Blue Chip 30
Index”, ” TIP TAIEX+ Industry Elite 30 Index”, ” TIP
TAIEX+ IT Elite 30 Index”, ” TIP TAIEX+ Low Volatility
Select 30 Index”, ” TIP TAIEX+ Low Beta 100 Index”, and ”
TIP TAIEX+ Blue Chip 30 Index Daily Return Inverse -1X
Index”, appendix 5 of TWSE-Compiled Index Table will be
updated accordingly. (Implemented in January 2017)
```
```
B.11.08 Revised in March
```
```
2017, Implemented in
```
```
March 2017.
```
```
Due to adding 2 new Indices of “TIP TAIEX+
Small/Mid-Cap Select 50 Index”, and ” TIP TAIEX+
Small/Mid-Cap Alpha Momentum 50 Index”, appendix 5 of
TWSE-Compiled Index Table are updated accordingly.
(Implemented in March 2017)
```
```
B.11.09 Revised in June 2017,
```
```
Implemented in July
```
```
2017.
```
```
1.Due to adding 2 new Indices of “TIP TAIEX+ Customized
High Dividend Minimum Variance Index”, and ” TIP Taiwan
Market Biotechnology and Medical Care Index”, appendix E
of TWSE-Compiled Index Table are updated accordingly.
(Implemented in July 10, 2017)
2.Due to ETF product code expansion, revising appendix B of
Stock Coding Rules (Implemented in July 31, 2017).
```
```
B.11.10 Revised in August
```
```
2017, Implemented in
```
```
September 2017.
```
```
Due to adding a new Index of “TIP TAIEX+ Industry Elite 30
Index Daily Return Inverse -1X Index” in th file spec, appendix
E of TWSE-Compiled Index Table is updated accordingly.
(Implemented in September 11, 2017)
```
```
B.11.11 Revised in November
```
```
2017, Implemented in
```
```
December 2017.
```
```
Due to adding 2 new Indices of “TIP Customized Domestic
Demand High Yield Price Index”, and ” FTSE4Good TIP
Taiwan ESG Index” in the file spec, appendix E of
TWSE-Compiled Index Table is updated accordingly.
(Implemented in December 11 and 18, 2017, respectively)
```
```
B.11. 12 Revised in January
```
```
2018 , Implemented in
```
```
March 2018.
```
```
Due to “Principles for Assignment of Ticker Symbols in the
Republic of China Securities Markets” revised, the appendix B
was updated accordingly.
1.Certificates of payment for shares, certificates of entitlement
to new shares, certificates of entitlement to new shares from
convertible bonds: A single English letter, from X to Z, is
added following the four-digit ticker symbol.
2.Preferred stocks: An English letter, from A to W, is added
following the four-digit ticker symbol.
```

Taiwan Stock Exchange

```
B.11.13 Revised in April 2018,
```
```
Implemented in May
```
```
2018.
```
```
Due to adding 3 new Indices of the “TIP Taiwan Market
Small/Mid cap Corporate Governance Index,” “TIP Taiwan
Market IPO Index”and ”TIP Value Investing 30 Index,”
appendix E of TWSE-Compiled Index Table in the file spec is
updated accordingly. (Implemented in May, 201 8 )
```
```
B.11.14 Revised in October,
```
```
2018, Implemented in
```
```
April 201 9.
```
```
Due to adding “exchange-traded note (ETN)”, the appendix B :
Stock coding rules was updated accordingly.
```
```
B.11.15 Revised in November
```
```
2018,
```
```
Implemented in
```
```
December 2018.
```
```
1.Due to adding 5 new Indices of “DVA 150 Total Return
Index”, “TIP H20 EWTR Index”,” TIP APL TR Index”,” TIP
TWSM 300 Index”and ” TIP TWSM 300 TR Index”, appendix
E of TWSE-Compiled Index Table in the file spec is updated
accordingly. (Implemented in December, 2018 )
```
```
B.11.16 Revised in January
```
```
2019,
```
```
Implemented in
```
```
February 2019.
```
```
1.Due to adding 2 new Indices of “ TIP PSHD 20 TR Index”
and ” TIP Cap.Top 500 TR Index”, appendix E of
TWSE-Compiled Index Table in the file spec is updated
accordingly. (Implemented in February, 2019 )
```
```
B.11.17 Revised in February
```
```
2019,
```
```
Implemented in
```
```
March 2019.
```
```
1.Due to adding 2 new Indices of “TIP FS 50 TR Index” and ”
ITE 30 Total Return Index”, appendix E of TWSE-Compiled
Index Table in the file spec is updated accordingly.
(Implemented in March, 2019)
```
2. Due to some of indices names change, the appendix E of
TWSE-Compiled Index Table updated accordingly.

```
B.11.18 Revised in September
```
```
2019,
```
```
Implemented in
```
```
December 2019.
```
```
1.Add a new format 21: Realtime Index Definition of TWSE
2.Appendix E: TWSE-Compiled Index Table is removed. The
index information can be retrieved from the website of TWSE
or TIP.
3.Due to adding the new Indices of “Taiwan Strategy Series
Indices”, the transmission Sequence for the format 10 of the
appendix A is updated accordingly.
```

Taiwan Stock Exchange

```
B.12.00 Revised in October
```
```
2018 ,
```
```
Revised in January
```
```
2019 ,
```
```
Implemented in
```
```
March 2020.
```
```
Revised in October, 2018
Implemented according to the expansion of each transaction
and price field
```
1. Revised Formats 1, 6, 9, 12, 13, 17 and 18, expanding the
    price fields as 9(5)V9(4)
2. Added the description of market order price in Formats 6 and
    17
3. Added the snapshot of stock market in Format 20

```
Revised in January, 2019
```
1. Revised Format 20, add 3 fields of opening price, the highest
    trading price and lowest trading price.

```
B.12.01 Revised in April, 2020
```
```
Implemented in
```
```
October 2020.
```
According to the Intraday odd lot trading

1. Added format 22 : Basic Data of Intraday odd lot trading

Stocks.

2.Added format 23 : Real-time Market Data of Intraday odd lot

trading stock.

Added appendix: Table of Transmission Setting.

Removed appendix: TWSE-Compiled Index Table.

```
B.12.0 2 Revised in March
```
```
2021 ,
```
```
Implemented in June,
```
```
2021.
```
According to the Taiwan Innovation Board

1. Added new field “Board Code” in format 1.
2. Added statistical information of Taiwan Innovation Board in
    format 2 and format 4.

```
B.12.0 3 Revised in March
```
```
2022 ,
```
```
Implemented in April
```
```
2022.
```
Due to “Principles for Assignment of Ticker Symbols in the

Republic of China Securities Markets” revised, the appendix B

was updated for the addition of Strategy ETN.


Taiwan Stock Exchange

```
B.12.0 4 Revised in May
```
```
2022 ,
```
```
Implemented in June
```
```
2022.
```
```
Due to “Principles for Assignment of Ticker Symbols in the
Republic of China Securities Markets” revised, the appendix B
was updated accordingly.
```
1. Remove the item of New Share Entitlement Certificates,
New Stock Right Certificates, Stock Share Payment
Certificates.
2. Update the item of General Preferred Stocks: An English

letter, from A to Y, is added following the four-digit ticker

symbol.

3. Add the item of Exchangeable Preferred Stocks.

```
B12.0 5
```
```
Revision in February
```
```
2022 ,
```
```
Implemented in June
```
```
2022.
```
Formats added

1. Format 24: Snapshot information of call (put) warrant
auction trading in the centralized market.
2. Format 25: Available balance for SBL.

Revised the format name.

1. Format 17: Real-time Auction Quotes of call (put) warrant on
TWSE Market.
2. Format 18: Information of Auction Opening (Closing) Price
of call (put) warrants on TWSE Market.

```
B.12.0 6 Revised in October
```
```
2022 ,
```
```
Implemented in
```
```
November 2022.
```
```
.Due to “Principles for Assignment of Ticker Symbols in the
Republic of China Securities Markets” revised, the appendix B
was updated accordingly.
Update the item of Callable Bear Contract with domestic
securities or index as underlying assets.
```

Taiwan Stock Exchange

```
B.12.0 7 Revised in March
```
```
2023 ,
```
```
Implemented in July
```
```
2023.
```
```
Due to "Taiwan Stock Exchange Corporation Key Points
for Classifying and Adjusting Categories of Industries of
Listed Companies " is amended and takes effect on July 3,
2023.
Appendix C:
ADD:
35.Green Energy and Environmental Services
36.Digital and Cloud Services
37.Sports and Leisure
38.Household
UPDATE:
```
16. Tourism Industry to Tourism and Hospitality

```
Format 10: Add transmission of Format 3 indices (including
the newly added industry category index)
Format 3 :maintains the current format
Format 21 :the value of format code of transferring index will
be fixed 10
```
```
B.12.08 Revised in March
```
```
2024 ,
```
```
Implemented in July
```
```
2024.
```
```
Format 3:" Statistics on Auction Indices of Common Stocks on
the Stock Market" information have been transmitted in Format
```
10. The transmission of Format 3 has been cancelled, and
removed the format from the specification

```
B.12.0 9 Revised in September
```
```
2024 ,
```
```
Implemented in
```
```
November 2024.
```
```
Due to “Principles for Assignment of Ticker Symbols in the
Republic of China Securities Markets” revised, the appendix B
was updated accordingly.
Update the item of ETF(exchange traded fund): The ETF codes
have been adjusted from five digits to six digits.
```
```
B.12. 10 Revised in October
```
```
2024 ,
```
```
Implemented in
```
```
November 2024.
```
```
Due to “Principles for Assignment of Ticker Symbols in the
Republic of China Securities Markets” revised, the appendix B
was updated accordingly.
Update the item of ETF(exchange traded fund): Adjust the
starting code range for ETF codes.
```

Taiwan Stock Exchange

```
B.12. 11 Revised in January
```
```
2025 ,
```
```
Implemented in
```
```
March 2025.
```
```
Due to “Principles for Assignment of Ticker Symbols in the
Republic of China Securities Markets” revised, the appendix B
was updated accordingly.
Adding stock type：Multi-Asset ETFs、Active ETFs and Bond
Active ETFs.
```

Taiwan Stock Exchange

## Appendix A: Description of Transmission Sequence................................................

Transmission of Before-hour Information (approx. 08:00-08:59)
Format 1: Information of individual stocks is transmitted once every minute.(Approx.07:40～08: 50 )
Format 2: System Time is transmitted once every 5 seconds.
Format 10 : Yesterday closing index is transmitted once every 5 seconds.
Format 5: Announcements of the day are transmitted once every 5 minutes.
Format 15: Information of suspended stocks of the day is transmitted every 5 minutes.
Format 22 : Basic Data of Intraday odd lot trading Stocks is transmitted once every minute.(Approx.07:40～
08: 50 )

Transmission of Common Stocks Intraday Information (09:00-13:45)
Format 1: Information of individual stocks is transmitted once every minute; new stocks only. （Approx. 09 : 00 ～
13:30）
Format 2: Trading information is transmitted once every 5 seconds.
Format 10 : The latest index information is transmitted once every 5 seconds.
Format 4: One-minute consigned trade information is transmitted once every 5 seconds.
Format 5: General announcements are transmitted once every 60 minutes; all new emergency announcements are
transmitted immediately; and sent emergency announcements of the day are transmitted in cycles every
5 minutes.
Format 6: The purchasing, selling and trading price/volume of individual stocks are transmitted in real time.
Format 12: Information of auction opening (closing) price of common stocks on stock market is transmitted at
every 10 minutes intraday and every 5 minutes after hour (until 14:00).
Format 17: The purchasing, selling and trading price/volume of Call (put) warrant are transmitted in real time.
Format 18: Transmission of information on the prices during trading hours once every 10 minutes, and once every
5 minutes at close, on the trading of Call (put) warrant in the TWSE market at opening (close)
(transmission ended at 14:00).
Format 20: Latest market display of the purchasing, selling and trading price/volume of individual stock is
transmitted every 5 seconds.
Format 22 : Basic Data of Intraday odd lot trading Stocks is transmitted once every minute; new stocks only.
(Approx. 09 : 00 ～13:30)
Format 23 : The purchasing, selling and trading price/volume of Intraday odd lot trading Stocks are transmitted in
real time.
Format 24: The latest market information of the purchase, sale and transaction price of the call (put) warrant is
transmitted every five seconds.

Transmission of After-hour Fixed-price Consignment Information (14:00-14:30)
Format 5: General announcements are transmitted once every 5 minutes; all new emergency announcements are
transmitted immediately; and sent emergency announcements of the day are transmitted in cycles every
5 minutes.
Format 8: One-minute consigned trade information is transmitted once every 30 seconds.
Transmission of After-hour Fixed-price Trading Information (approx. 14:30-15:00)
Format 7: Statistics on the trading volume (except the accumulative trading volume of auction) of the after-hour
fixed-price transaction market is transmitted after the match transactions are completed by the
after-hour fixed-price transaction system; and the last trading information of the fixed-price transaction
market is transmitted repeatedly at every 30 minutes afterwards.
Format 8: Statistics on the one-minute consignments in the last trading information of the fixed-price transaction
market is transmitted repeatedly at every 30 minutes.
Format 9: Information of after-hour fixed-price trading of individual stocks (except the accumulative trading
volume of auction) is transmitted once every minute.
Transmission of Miscellaneous Information
Format 5: General announcements are transmitted once at every 5 minutes from 14:30-17:00; all new
emergency announcements are transmitted immediately; and sent emergency announcements of the
day are transmitted in cycles every 5 minutes.
Format 10: New Taiwan indices are transmitted once every 5 seconds from 08:00- 14 : 3 0.
Format 13: Real-time transaction information of odd lots on stock market is transmitted during 14:25-14:50. Trial


Taiwan Stock Exchange

```
calculation information is transmitted before matching at 14:30. Match trading information of odd lots
is transmitted after 14:30 every 30 seconds repeatedly until 14:50.
Format 14: Data of the full name of Callable Bull(Bear) Contracts on stock market are transmitted during
14:30-16:00. Data of the full name of Callable Bull(Bear) Contracts on stock market are transmitted
once every 10 minutes after 14:30, until 16:00.
Format 16: The HeartBeat data of the stock market quote transmission system are transmitted during 08:00-17:20.
Startup information is transmitted immediately after the startup of the daily quote transmission system.
HeartBeat data are then transmitted once every 30 seconds; and the last entry of data is transmitted
about every 5 minutes after 17:15.
Format 19: Data on stocks suspended/resumed for trading in TWSE will be transmitted during 08:00-17:00. Data
on stocks newly suspended/resumed for trading will be transmitted immediately. Data on stocks
suspended/resumed for trading will be transmitted once every 10 minutes.
Format 2 1: Realtime index definition are transmitted once at every 1 minutes from 08:00-14:30.
Format 25: The latest available balance for SBL will be transmitted every five seconds between 07:40 to 17:10,
but will not be transmitted if there is no change in the information. If the individual stock information
has not changed for a long time, then the latest information will be retransmitted within 30 seconds.
```

Taiwan Stock Exchange

## Appendix B: Stock coding rules

(1) A stock code is expressed in ASCII code, 6 bytes

Stock Code

Stock Type (^) byte
1
Byte
2
Byte
3
byte
4
byte
5
byte
6
Beneficiary Certificate (note 1) 0 0

#### 0

#### |

#### 3

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

ETF(exchange traded fund) (note 1)

#### 0 0

#### 4

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

ETF(denominated in foreign currencies) K
Bond ETF B
Bond ETF(denominated in foreign currencies) C
Multi-Asset ETFs T
Leveraged ETFs L
Leveraged ETFs (denominated in foreign currencies) M
Inverse ETFs R
Inverse ETFs (denominated in foreign currencies) S
Futures Trust ETFs U
Futures Trust ETFs (denominated in foreign currencies) V
Active ETFs A
Bond Active ETFs D
Real Estate Asset Trust Beneficiary Securities
0 1

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### P

Financial Asset Securitization Beneficiary Securities S
Real Estate Investment Trust Beneficiary Securities T

ETN 0 2

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

Bond ETN

#### 0 2

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### B

Leveraged ETN L

Inverse ETN R

Strategy ETN S

Callable Bull Contract with domestic securities or index as
underlying assets.

#### 0

#### 3

#### |

#### 8

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

Callable Bear Contract with domestic securities or index
as underlying assets.

#### 0

#### 3

#### |

#### 8

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### P

#### U

#### T

Callable Bull Contract with foreign securities or indexes
as underlying assets F^
Callable Bear Contract with foreign securities or indexes
as underlying assets

#### Q

“Lower Limit Callable Bull Contract” (Bull Contract)
with domestic securities or indexes as underlying assets

#### C

“Upper Limit Callable Bear Contract” (Bear Contract)
with domestic securities or indexes as underlying assets

#### B

“Open-End Callable Bull Contract” whose underlying
assets are domestic securities or indices (Open-End
Callable Bull Contract)

#### X

“Open-End Callable Bear Contract” whose underlying
assets are domestic securities or indices (Open-End
Callable Bear Contract)

#### Y

Common Stocks 1 0 0 0


Taiwan Stock Exchange

#### |

#### 9

#### |

#### 9

#### |

#### 9

#### |

#### 9

Depository Receipt (note 1)

#### 9 1

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

Corporate Bond Convertible into Depository Receipts C

#### 1

#### |

#### 9

Corporate Bond with Warrants on Depository Receipts G

#### D

#### |

#### L

Remaining Corporate Bond with Warrants Exercised for
Depository Receipts

#### F

#### 1

#### |

#### 9

Warrant on Depository Receipts G

#### 1

#### |

#### 9

General Preferred Stocks Original Code

#### A

#### |

#### Y

Preferred Stocks with Warrants Original Code G

#### A

#### |

#### C

Debentures with Warrants Original Code G

#### D

#### |

#### L

Subscription warrants Original Code G

#### 1

#### |

#### 9

Cooperate Bonds of Performed Debentures with Warrants Original Code F

#### 1

#### |

#### 9

Exchangeable Preferred Stocks Original Code Z

#### 1

#### |

#### 9

Convertible Bonds Original Code

1

|

9

Void,

0

|

9

Exchangeable Corporate Bonds, Exchangeable Financial
Bonds Original Code^0

1

|

9

Government Bonds

#### A

#### C

#### D

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

#### 0

#### |

#### 9

Foreign Securities F － － － － －

Note 1: The previous 6-digit principle of codification for close-end fund certificates, ETF, TDR is

applicable to securities listed in the exchange after December 15 2009. Stock code previously


Taiwan Stock Exchange

assigned is still in 4-digit. Stock code defined as 9201~9299 under TDR remains unchanged.

The ETF coding rule is applicable only to securities listed after November 18, 2024. The previously

assigned security codes, including five-digit numbers (applicable to securities listed after June 24,

2014), as well as four-digit or six-digit numbers (applicable to securities listed before June 24,

2014), will remain their original coding rules without reassigning new codes.

(2) Government Bond Coding Rules (6 codes)
□□□□□□
1 2 3 4 5 6

1. Code 1 is a letter: A- Central Government Bond; C-Taipei City Government Bond;
    D-Kaohsiung City Government Bond
2. Codes 2-3 represent year, e.g. 93, 94, 95 etc.
3. Code 4 represents bond type.
4. Codes 5-6 represent period, e.g. 01, 02, 03 etc.

Note 2: The stock code starts 09 is preserved.


Taiwan Stock Exchange

## Appendix C: Stock Industry Category (Cf Format 1 3.1.3)

(1) Codes 1 and 4 of government bonds are the identity code of the bond’s industry category.

(2) The industry category of common stocks is identified with the following codes:

```
Industry
Category
Code
```
```
Industry
Category
```
```
Industry
Category
Code
```
```
Industry
Category
```
```
Industry
Category
Code
```
```
Industry
Category
```
```
Industry
Category
Code
```
```
Industry
Category
```
```
01
Cement
Industry
12
Automobile
Industry
23
```
```
Oil, Gas and
Electricity
Industry
```
```
36
```
```
Digital
and Cloud
Services
```
```
02
Food
Industry
```
```
14
```
```
Building
Materials and
Construction
Industry
```
```
24
Semiconductor
Industry
```
```
37
Sports and
Leisure
```
```
03
Plastic
Industry
15
```
```
Shipping and
Transportation
Industry
```
```
25
```
```
Computer and
Peripheral
Equipment
Industry
```
```
38 Household
```
```
04
Textiles
Industry
16
Tourism and
Hospitality
26
Optoelectronic
Industry
```
```
05
```
```
Electric
Machinery
Industry
```
```
17
```
```
Finance and
Insurance
27
```
```
Communications
and Internet
Industry
```
```
06
```
```
Electrical and
Cable
Industry
```
```
18
```
```
Trading and
Consumers
Goods Industry
```
```
28
```
```
Electronic Parts/
Components
Industry
```
```
08
```
```
Glass and
Ceramics
Industry
```
```
19
```
```
General
Industry
29
```
```
Electronic
Products
Distribution
Industry
```
```
09
```
```
Paper and
Pulp Industry
20 Other Industry 30
```
```
Information
Service Industry
```
```
10
Iron and
Steel Industry
21
Chemical
Industry
31
Other Electronic
Industry
```
```
11
```
```
Rubber
Industry
22
```
```
Biotechnology
and Medical
Care
```
```
35
```
```
Green Energy and
Environmental
Services
```

Taiwan Stock Exchange

## Appendix D: Stock Category Code Table (Cf Format 1 3.1.4)

```
Code Meaning
W1 Callable Bull Contract, proportionally issued (the amount of original conversion target shares is
1000 upon issue)
W2 Callable Bull Contract, un-proportionally issued (the amount of original conversion target
shares is not 1000 upon issue)
W3 Callable Bear Contract, proportionally issued (the amount of original conversion target shares
is 1000 upon issue)
W4 Callable Bear Contract, un-proportionally issued (the amount of original conversion target
shares is not 1000 upon issue)
BS Securities stocks of domestic listed companies
FB Bank stocks of domestic listed companies
Blank Listed securities of other domestic companies
RR Listed securities of other foreign companies
RS Securities stocks of foreign listed companies
RB Bank stocks of foreign listed companies
```
## Appendix E: Table of Transmission Setting

Line Transmission Category Multicast Group Port

1 st IP

Real-Time Market Data of

stock and basic data

224.0.100.100 10000

224.0.200.200 20000

2 nd IP

Real-Time Market Data of

Call (put) warrant and

basic data

22 4.2.100.100 10002

224.2.200.200 20002

3 rd IP

Snapshot Data of stock、

Call (put) warrant and

basic data

224.4.100.100 10004

224.4.200.200 20004

4 th IP

Other Data

(basic data、statistics、

announcement、available

balance for SBL...etc)

224.6.100.100 10006

224.6.200.200 20006

5 th IP

Real-Time Market Data

and basic data of Intraday

odd lot trading stock

224.8.100.100 10008

224.8.200.200 20008


Taiwan Stock Exchange

## Appendix F: Table of Online Transmission Formats

```
Line
Market
information format
```
```
1st IP 2nd IP 3rd IP 4th IP 5th IP
```
```
Format 1    
Format 2

Format 4

Format 5

Format 6 
Format 7

Format 8

Format 9

Format 10

```
Format (^12) 

Format 13

Format 14

Format 15

Format 16 
Format 17

Format 18


Format 19

Format 20

Format 21 
Format (^22) 
Format (^23) 
Format (^24) 
Format (^25) 

## Appendix G: Trading Currency Code

Represented by ASCII Code in length of 3 bytes

Currency

code

CNY JPY KRW USD CAD GBP EUR SEK AUD

Country China Japan S.Korea US Canada UK Europe

Zone

Sweden Australia

Currency

code

HKD SGD


Taiwan Stock Exchange

Country HK^ Singapore^


