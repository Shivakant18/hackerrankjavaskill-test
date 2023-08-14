# hackerrankjavaskill-test

java script basic 

#node anagramStrings.js



const readline = require('readline');
const fs = require('fs');

const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout
});

function stringAnagram(dictionary, query) {
  const frequencyMap = new Map();
  
  for (const word of dictionary) {
    const chars = word.split('');
    chars.sort();
    const sorted = chars.join('');
    frequencyMap.set(sorted, (frequencyMap.get(sorted) || 0) + 1);
  }
  
  const result = [];
  
  for (const word of query) {
    const chars = word.split('');
    chars.sort();
    const sorted = chars.join('');
    result.push(frequencyMap.get(sorted) || 0);
  }
  
  return result;
}

const dictionary = [];
const query = [];

let dictionaryCount = 0;
let queryCount = 0;

let inputState = 0;

rl.on('line', (input) => {
  if (inputState === 0) {
    dictionaryCount = parseInt(input);
    inputState = 1;
  } else if (inputState === 1) {
    dictionary.push(input);
    if (dictionary.length === dictionaryCount) {
      inputState = 2;
    }
  } else if (inputState === 2) {
    queryCount = parseInt(input);
    inputState = 3;
  } else if (inputState === 3) {
    query.push(input);
    if (query.length === queryCount) {
      rl.close();
    }
  }
});

rl.on('close', () => {
  const result = stringAnagram(dictionary, query);

  const ws = fs.createWriteStream(process.env.OUTPUT_PATH);

  result.forEach((value) => {
    ws.write(value + '\n');
  });

  ws.end();
});
