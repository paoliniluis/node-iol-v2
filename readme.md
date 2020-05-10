# Node.Js ultra-light wrapper for InvertirOnLine REST API V2

Disclaimer: I'm not responsible for any damage, of any way, when using this module, if you find any kind of problems, create an issue! Also: I'm not giving any investing advise here, all you see here is personal work and does not have any relationship with the company I work for or any company I worked or I will work in the future!.

[Link to Github repo](https://github.com/paoliniluis/node-iol-v2)

## How to start using this package

1) Ask the guys of InvertirOnline to enable you the API (at the time this was published it was still free and we want them to keep it this way to foster the argentinian capital markets!)
2) Install Node.Js
3) Clone/Download this repo OR create a new folder with any name and do `npm install node-iol-v2`
4) Complete the `auth.json` file with your username and password (if you did the npm install, the file will be inside your node-modules folder)
5) Enjoy!

## Usage

First we create a file (can be index.js) where we require this package if we did the `npm install node-iol-v2` thing
`const iol = require('node-iol');`
or, if we downloaded from github, we require the file directly 
`const iol = require('./node-iol');`
Then we write some code like this one:

~~~~javascript
let iol = require ("./node-iol")

async function getMEPDollarQuote() {
    let token = await iol.auth();
    let ay24 = await iol.getTickerValue(token, 'bcba', 'ay24');
    console.log(ay24.ultimoPrecio);
    let ay24d = await iol.getTickerValue(token, 'bcba', 'ay24d');
    console.log(ay24d.ultimoPrecio);
    console.log('Purchasing dollars in the stock market costs:', ay24.ultimoPrecio/ay24d.ultimoPrecio);
}

getMEPDollarQuote()
~~~~

or maybe something like a CCL (contado con liqui) finder:

~~~~javascript

const iol = require('./node-iol');

async function cclGGAL() {
    let token = await iol.auth();
    let ggal = await iol.getTickerValue(token, 'bCBA', 'GGAL');
    let ggald = await iol.getTickerValue(token, 'nYSE', 'GGAL');

    let ccl = (ggal.ultimoPrecio/ggald.ultimoPrecio) * 10; // multiplied by 10 due to conversion factor to calculate CCL;

    console.log('GGAL price in ARG', ggal.ultimoPrecio);
    console.log('GGAL price in USA', ggald.ultimoPrecio);

    console.log('so...');

    console.log(`CCL price for GGAL is ${ccl}`);
}

cclGGAL()
~~~~

## Notes

- Be extremely careful when calling methods, as I've done this in a couple of hours and it might not cover all cases (like get values that accepts a term but I haven't covered that and might be useful to calculate intrinsic rates!!!)

## To Do

- Tests Tests Tests (Mocha)
- Review The getTickerValuesBetweenDates function, haven't tried it, specially dates formats
- Polish the username/password authentication (hey! this was done in just one day!)
