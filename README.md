# WolfBot
This is the source of the Crypto currency trading bot running on: https://wolfbot.org

It is written in TypeScript for NodeJS + MongoDB.

## Features
* **Trading**: buying + selling, portfolio management (sync balances with exchanges)
* **Margin Trading**: leveraged trading including short selling and futures trading
* **Arbitrage**: profit from price differences between 2 exchanges (done "on the books" with balances on both exchanges, no withdrawals from exchanges required)
* **Lending**: lend your coins on the lending market of supported crypto exchanges for the highest possible interest rates
* **Backtesting**: test your trade strategies in simulation on historical data
* **Web Plugins**: [Access social media data](https://wolfbot.org/social-sentiment/) from Twitter, Reddit, Telegram Channels, RSS Feeds,... to trade based on news and real world events (not part of open source version yet)

Screenshots of the Trading UI:

[Trading Chart](https://wolfbot.org/wp-content/uploads/2018/06/trading-1024x605.png)

[Live Strategy data](https://wolfbot.org/wp-content/uploads/2018/06/trading_strat-1024x564.png)

[Strategy configuration](https://wolfbot.org/wp-content/uploads/2018/06/config_strat-1024x606.png)

A more detailed list of all features: https://wolfbot.org/features/

The list of supported exchanges: https://wolfbot.org/features/exchanges/

The full strategy documentation: https://wolfbot.org/strategy-docs/

## Getting Started

### Requirements
```
NodeJS >= 10
MongoDB >= 3.4
TypeScript >= 2.8
yarn >= 1.9.4 (npm should work too, but no support given if you run into errors)
Webpack >= 4 (only for UI modifications)
```


### Installation
```
git clone https://github.com/Ekliptor/WolfBot
yarn install
```
You can use the `--production` flag if you only want to run the bot and not make any code changes.


### Start trading
Rename the `configLocal-sample.ts` file in the project root directory to `configLocal.ts` and add at least `mongoUrl` (plus some exchange API keys if you want to trade).


After running TypeScript (automatically in your IDE or run the `tsc` command in the project root dir) you will see a file:
```
build/app.js
```
`tsc` will show you some errors (due to shared code with missing types between server and client side, I will refactor this later). Just ignore these errors and make sure `noEmitOnError` is not set (the default) and that you have a `build/` dir in the project root as well as for all packages under `node_modules/@ekliptor` which contain a `tsconfig.json` file. You Use the `build` directory as the working directory and run:
```
node app.js --debug --config=Noop --trader=RealTimeTrader --noUpdate --noBrowser
```
The `config` parameter must be a JSON file from the `config` directory. For a list of all parameters look at the top of the `app.ts` file.


### Writing your own trading strategies
There is documentation available here: https://forum.wolfbot.org/forums/strategy-development.17/

Look into the `/src/Strategies` folder for more examples.


### Modifying the UI
In the project root directory, run:
```
webpack --progress --colors --watch
```

You should also run the bot with the `--uiDev` flag so that changes to HTML template files are reloaded from disk.

The UI uses a single persistent WebSocket connection. UI related code is in the following directories:
```
├ project root
├─── public <- all code shipped to the browser
├────── js <- all TypeScript code to be compiled with WebPack
├─── src
├────── WebSocket <- all server-side logic to push updates to the browser
├─── views <- all HTML templates
```


## REST and WebSocket JSON APIs
If you want to connect WolfBot to your trading terminal, please take a look at the [API docs](https://github.com/Ekliptor/WolfBot/blob/master/tests/docs/WolfBot%20API.pdf).

This let's you:
* create new cloud bot instances and earn commission for every referral (not applicable to open source version)
* import the trades book with all trades WolfBot made via REST API call
* subscribe to live trades via WebSocket to display them in your trading terminal
* subscribe to live strategy data and indicator values via WebSocket

