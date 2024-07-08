import openai
import requests
import json
import tkinter as tk
from tkinter import messagebox
from requests.auth import HTTPBasicAuth

# Set up the OpenAI API key
openai.api_key = 'your-openai-api-key'

# Function to generate blog content
def generate_content(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a helpful assistant."},
            {"role": "user", "content": prompt},
        ],
        max_tokens=500
    )
    return response.choices[0].message['content'].strip()

# Function to post content to WordPress
def post_to_wordpress(title, content):
    wordpress_site = 'https://your-wordpress-site.com/wp-json/wp/v2/posts'
    username = 'your-username'
    password = 'your-application-password'

    data = {
        'title': title,
        'content': content,
        'status': 'publish'
    }

    response = requests.post(
        wordpress_site,
        auth=HTTPBasicAuth(username, password),
        headers={'Content-Type': 'application/json'},
        data=json.dumps(data)
    )

    if response.status_code == 201:
        messagebox.showinfo("Success", "Post created successfully.")
    else:
        messagebox.showerror("Error", f"Failed to create post. Response: {response.text}")

# Function to handle content generation
def generate():
    prompt = prompt_entry.get("1.0", tk.END).strip()
    if not prompt:
        messagebox.showerror("Error", "Prompt cannot be empty.")
        return

    content = generate_content(prompt)
    content_text.delete("1.0", tk.END)
    content_text.insert(tk.END, content)

# Function to handle content posting
def post():
    title = title_entry.get().strip()
    if not title:
        messagebox.showerror("Error", "Title cannot be empty.")
        return

    content = content_text.get("1.0", tk.END).strip()
    if not content:
        messagebox.showerror("Error", "Content cannot be empty.")
        return

    post_to_wordpress(title, content)

# Set up the GUI
root = tk.Tk()
root.title("Blog Post Generator and Poster")

tk.Label(root, text="Prompt:").pack(pady=5)
prompt_entry = tk.Text(root, height=5, width=50)
prompt_entry.pack(pady=5)

tk.Label(root, text="Title:").pack(pady=5)
title_entry = tk.Entry(root, width=50)
title_entry.pack(pady=5)

tk.Button(root, text="Generate", command=generate).pack(pady=5)
tk.Button(root, text="Post", command=post).pack(pady=5)

tk.Label(root, text="Generated Content:").pack(pady=5)
content_text = tk.Text(root, height=15, width=50)
content_text.pack(pady=5)

root.mainloop()
