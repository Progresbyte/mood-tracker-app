# AI-Powered Daily Mood Tracker Web App

A simple web app for tracking your daily mood, journaling, and receiving motivational quotes or tips based on your mood.

---

## Features

- Log your mood (Happy, Sad, Neutral, etc.)
- Write a short journal entry
- Get motivational quotes based on your mood (using a public API)
- Simple, responsive design

---

## Demo

![Mood Tracker Screenshot](screenshot.png)

---

## How to Use

1. Open `index.html` in your browser.
2. Select your mood.
3. Write your journal entry.
4. Click "Save Entry" to log your mood and get a motivational quote.

---

## Code

### `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Daily Mood Tracker</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>Daily Mood Tracker</h1>
        <form id="moodForm">
            <label for="mood">How are you feeling today?</label>
            <select id="mood" required>
                <option value="">Select mood</option>
                <option value="happy">😊 Happy</option>
                <option value="sad">😢 Sad</option>
                <option value="neutral">😐 Neutral</option>
                <option value="angry">😠 Angry</option>
                <option value="anxious">😰 Anxious</option>
            </select>
            <label for="journal">Journal Entry</label>
            <textarea id="journal" rows="4" placeholder="Write something..."></textarea>
            <button type="submit">Save Entry</button>
        </form>
        <div id="quoteSection" class="hidden">
            <h2>Motivational Quote</h2>
            <p id="quote"></p>
        </div>
    </div>
    <script src="app.js"></script>
</body>
</html>
```

---

### `style.css`

```css
body {
    font-family: Arial, sans-serif;
    background: #f0f4f8;
    margin: 0;
    padding: 0;
}
.container {
    max-width: 400px;
    margin: 40px auto;
    background: #fff;
    padding: 2em;
    border-radius: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
}
h1, h2 {
    text-align: center;
}
label {
    display: block;
    margin-top: 1em;
}
select, textarea, button {
    width: 100%;
    margin-top: 0.5em;
    padding: 0.5em;
    border-radius: 5px;
    border: 1px solid #ccc;
}
button {
    background: #0078d4;
    color: #fff;
    border: none;
    margin-top: 1em;
    cursor: pointer;
}
button:hover {
    background: #005fa3;
}
.hidden {
    display: none;
}
#quoteSection {
    margin-top: 2em;
    background: #e6f7ff;
    padding: 1em;
    border-radius: 8px;
}
```

---

### `app.js`

```javascript
document.getElementById('moodForm').addEventListener('submit', async function(e) {
    e.preventDefault();
    const mood = document.getElementById('mood').value;
    const journal = document.getElementById('journal').value;
    if (!mood) return;

    // Fetch a motivational quote from a public API
    let quote = '';
    try {
        const res = await fetch('https://api.quotable.io/random?tags=motivational|inspirational');
        const data = await res.json();
        quote = data.content;
    } catch {
        quote = "Keep going! You're doing great.";
    }

    // Optionally, customize the quote based on mood
    if (mood === 'sad') quote = "Every day may not be good, but there's something good in every day.";
    if (mood === 'anxious') quote = "Take a deep breath. You are stronger than you think.";
    if (mood === 'angry') quote = "Let go of anger. Embrace peace.";
    if (mood === 'happy') quote = "Keep spreading your happiness!";
    if (mood === 'neutral') quote = "Stay balanced and keep moving forward.";

    document.getElementById('quote').textContent = quote;
    document.getElementById('quoteSection').classList.remove('hidden');

    // Optionally, save entry to localStorage
    const entry = { date: new Date().toLocaleDateString(), mood, journal };
    let entries = JSON.parse(localStorage.getItem('moodEntries') || '[]');
    entries.push(entry);
    localStorage.setItem('moodEntries', JSON.stringify(entries));
});
```

---

## Public API Used

- [Quotable API](https://github.com/lukePeavey/quotable) for motivational quotes

---

## License

MIT
