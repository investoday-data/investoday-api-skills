# InvestToday Financial Data

InvestToday provides 180+ financial data APIs covering China A-shares, Hong Kong stocks, funds, indices, macro data, and research-oriented datasets.

> 中文版说明请见：[SKILL.md](./SKILL.md)

## API Key

- [Get an API Key](https://data-api.investoday.net/login)
- Create a `.env` file in the root of this skill directory, at the same level as `SKILL.md`, for example:
  - Cursor: `.cursor/skills/investoday-finance-data/.env`
  - Claude Code: `.claude/skills/investoday-finance-data/.env`
```bash
INVESTODAY_API_KEY=<your_key>
```
- You can also configure the key through an environment variable. Avoid leaving the plaintext key in shell history.

**API Key security rules (strictly required):**

1. **Never print the plaintext API Key in terminal output, logs, or chat messages.**
2. When the user provides an API Key:
   - write it directly into `.env`; do **not** echo, print, or reveal the key
   - make sure `.env` is included in `.gitignore`
   - after saving it, **must** respond with the following message:

> ✅ API Key has been configured. Your API Key is the unique credential for accessing InvestToday data. Store it securely and never expose it in chats, screenshots, or code commits.

3. When calling the API, **do not** place the API Key in command-line arguments, logs, or error output.
4. When checking whether the key is configured, only say `configured` or `not configured`. Do **not** reveal any part of the key.

## Calling the API

```bash
# GET (default)
node scripts/call_api.js <endpoint> [key=value ...]

# POST (parameters are sent as JSON body)
node scripts/call_api.js <endpoint> --method POST [key=value ...]

# Array parameters: repeat the same key
node scripts/call_api.js <endpoint> --method POST codes=000001 codes=000002
```

Check the `references/` docs for whether an endpoint uses GET or POST. Responses are returned as JSON. Failures are printed as error messages.

**Examples**

```bash
node scripts/call_api.js search key=600519 type=11
node scripts/call_api.js stock/basic-info stockCode=600519
node scripts/call_api.js stock/adjusted-quotes stockCode=600519 beginDate=2024-01-01 endDate=2024-12-31
node scripts/call_api.js fund/daily-quotes --method POST fundCode=000001 beginDate=2024-01-01 endDate=2024-12-31
```

## Reference Index

Find endpoint paths and request parameters in the corresponding files under `references/`. The reference filenames remain in Chinese because the source documents are currently maintained in Chinese.

| Category | Subcategory | Document |
|------|--------|------|
| Core Data | — | [Core Data (基础数据)](references/基础数据.md) |
| Market Data | — | [Market Data (市场数据)](references/市场数据.md) |
| Mainland China Equities | Basic Info | [Basic Info (基础信息)](references/沪深京数据/基础信息.md) |
|  | Stock Quotes | [Stock Quotes (股票行情)](references/沪深京数据/股票行情.md) |
|  | Financial Data | [Financial Data (财务数据)](references/沪深京数据/财务数据.md) |
|  | Special Data | [Special Data (特色数据)](references/沪深京数据/特色数据.md) |
|  | Corporate Actions | [Corporate Actions (公司行为)](references/沪深京数据/公司行为.md) |
| Sectors | Base Quotes | [Base Quotes (基础行情)](references/板块/基础行情.md) |
|  | Derived Quotes | [Derived Quotes (衍生行情)](references/板块/衍生行情.md) |
|  | Base Data | [Base Data (基础数据)](references/板块/基础数据.md) |
|  | Financial Data | [Financial Data (财务数据)](references/板块/财务数据.md) |
|  | Special Data | [Special Data (特色数据)](references/板块/特色数据.md) |
|  | Analysis & Forecast | [Analysis & Forecast (分析与预测)](references/板块/分析与预测.md) |
| Indices | Base Quotes | [Base Quotes (基础行情)](references/指数/基础行情.md) |
|  | Technical Indicators | [Technical Indicators (技术指标)](references/指数/技术指标.md) |
|  | Derived Quote Data | [Derived Quote Data (行情衍生数据)](references/指数/行情衍生数据.md) |
|  | Index Profile | [Index Profile (指数资料)](references/指数/指数资料.md) |
| News & Views | Base Data | [Base Data (基础数据)](references/新闻与观点/基础数据.md) |
| Research Reports | Base Data | [Base Data (基础数据)](references/研报/基础数据.md) |
|  | Special Data | [Special Data (特色数据)](references/研报/特色数据.md) |
|  | Analyst Ratings | [Analyst Ratings (投资评级)](references/研报/投资评级.md) |
| Announcements | — | [Announcements (公告)](references/公告.md) |
| Hong Kong Stocks | Financial Data | [Financial Data (财务数据)](references/港股/财务数据.md) |
|  | Base Data | [Base Data (基础数据)](references/港股/基础数据.md) |
|  | HK Market Quotes | [HK Market Quotes (港股行情)](references/港股/港股行情.md) |
|  | Corporate Actions | [Corporate Actions (公司行为)](references/港股/公司行为.md) |
| Tools | Icons | [Icons (图标)](references/工具/图标.md) |
| Macro Economy | China Macro | [China Macro (国内宏观)](references/宏观经济/国内宏观.md) |
| LLM Corpora | — | [LLM Corpora (大模型语料)](references/大模型语料.md) |
| Funds | Fund Quotes | [Fund Quotes (基金行情)](references/基金/基金行情.md) |
|  | Fund Profiles | [Fund Profiles (基金资料)](references/基金/基金资料.md) |
|  | Fund Performance | [Fund Performance (基金业绩表现)](references/基金/基金业绩表现.md) |
|  | Fund Portfolio | [Fund Portfolio (基金投资组合)](references/基金/基金投资组合.md) |
|  | Fund Holders | [Fund Holders (基金持有人)](references/基金/基金持有人.md) |
|  | Special Data | [Special Data (特色数据)](references/基金/特色数据.md) |
|  | ETF Funds | [ETF Funds (ETF基金)](references/基金/ETF基金.md) |
|  | Fund Financial Data | [Fund Financial Data (基金财务数据)](references/基金/基金财务数据.md) |

## Security & Privacy

- **Data sent off the local machine:** endpoint path, query parameters, and `INVESTODAY_API_KEY` (sent over HTTPS to `data-api.investoday.net`)
- **Data that stays local:** local files, other environment variables, and chat content
- The API Key is used only for authentication and is not forwarded to third parties

## External Endpoint

| Endpoint | Purpose | Data Sent |
|------|------|-----------|
| `https://data-api.investoday.net/data/cloud/*` | Financial data queries | API Key (header), query parameters |

> **Trust notice:** This skill sends requests to the InvestToday data platform (`data-api.investoday.net`). Only install and use it if you trust this service.

## Related Links

[API Docs](https://data-api.investoday.net/hub?url=%2Fapidocs%2Fai-native-financial-data) · [FAQ](https://data-api.investoday.net/hub?url=%2Fapidocs%2Ffaq) · [Contact](https://data-api.investoday.net/hub?url=%2Fapidocs%2Fcontact-me)
