# Ai-copilot
Building the custom AI copilot involved several steps, from setting up the development environment to implementing and testing the server. Below is a detailed guide on how we built both the Node.js and Python versions of the project.

code:

How We Built the Custom AI Copilot
Building the custom AI copilot involved several steps, from setting up the development environment to implementing and testing the server. Below is a detailed guide on how we built both the Node.js and Python versions of the project.

Steps
mkdir ai-copilot
cd ai-copilot
npm init -y
npm install express openai dotenv

OPENAI_API_KEY=your_openai_api_key_here

Create an index.js file and set up an Express server:


const express = require('express');
const { Configuration, OpenAIApi } = require('openai');
require('dotenv').config();

const app = express();
const port = 3000;

app.use(express.json());

const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);

app.post('/chat', async (req, res) => {
  const { prompt } = req.body;
  if (!prompt) {
    return res.status(400).send({ error: 'Prompt is required' });
  }

  try {
    const response = await openai.createCompletion({
      model: 'text-davinci-003',
      prompt: prompt,
      max_tokens: 150,
    });

    res.send(response.data.choices[0].text);
  } catch (error) {
    res.status(500).send({ error: 'Something went wrong' });
  }
});

app.listen(port, () => {
  console.log(`AI Copilot app listening at http://localhost:${port}`);
});
Run Your Server
Test Your API

Use a tool like Postman or curl to send a POST request to http://localhost:3000/chat with a JSON body containing a prompt:
json
{
  "prompt": "Tell me a joke."
}

mkdir ai_copilot
cd ai_copilot
python -m venv venv
source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
pip install flask openai python-dotenv
Create Environment Variables

Create a .env file in your project root and add your OpenAI API key.

OPENAI_API_KEY=your_openai_api_key_here


Create an app.py file and set up a Flask server:

from flask import Flask, request, jsonify
import openai
from dotenv import load_dotenv
import os

load_dotenv()
openai.api_key = os.getenv('OPENAI_API_KEY')

app = Flask(__name__)

@app.route('/chat', methods=['POST'])
def chat():
    data = request.get_json()
    prompt = data.get('prompt')
    if not prompt:
        return jsonify({'error': 'Prompt is required'}), 400

    try:
        response = openai.Completion.create(
            model="text-davinci-003",
            prompt=prompt,
            max_tokens=150
        )
        return jsonify({'response': response.choices[0].text})
    except Exception as e:
        return jsonify({'error': str(e)}), 500

if __name__ == '__main__':
    app.run(port=5000)
Run Your Server


python app.py
Test Your API

Use a tool like Postman or curl to send a POST request to http://localhost:5000/chat with a JSON body containing a prompt:
json
Copy code
{
  "prompt": "Tell me a joke."
}
