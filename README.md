const { Client } = require('discord.js');
const fetch = require('node-fetch');

const client = new Client();
const prefix = '!'; // Change this to your preferred command prefix

client.once('ready', () => {
    console.log('Bot is ready!');
});

client.on('message', async (message) => {
    if (message.author.bot) return; // Ignore messages from bots

    if (message.content.startsWith(prefix)) {
        const [command] = message.content.slice(prefix.length).split(' ');

        if (command === 'joke') {
            const joke = await fetchJoke();
            message.channel.send(joke);
        } else if (command === 'quote') {
            const quote = await fetchQuote();
            message.channel.send(quote);
        }
        // Add more commands as needed
    }
});

async function fetchJoke() {
    const response = await fetch('https://official-joke-api.appspot.com/random_joke');
    const joke = await response.json();
    return `${joke.setup}\n${joke.punchline}`;
}

async function fetchQuote() {
    const response = await fetch('https://api.quotable.io/random');
    const quote = await response.json();
    return `"${quote.content}" - ${quote.author}`;
}

// Replace 'YOUR_BOT_TOKEN' with your actual bot token
client.login('YOUR_BOT_TOKEN');
