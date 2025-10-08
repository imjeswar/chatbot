import streamlit as st
import requests

# === Your Gemini API Key ===
API_KEY = "AIzaSyAwjWaL1EwFuqEn5UJu7ioCQW2htzXyC3o"

# === Streamlit Page Setup ===
st.set_page_config(page_title=" Chatbot", page_icon="🤖")
st.title("🤖 TALK WITH KYRIOS")

# === Initialize Chat History ===
if "messages" not in st.session_state:
    st.session_state["messages"] = []  # list of {"role": "user"/"assistant", "text": str}

# === Display Previous Messages ===
for msg in st.session_state["messages"]:
    if msg["role"] == "user":
        st.chat_message("user").write(msg["text"])
    else:
        st.chat_message("assistant").write(msg["text"])

# === User Input ===
if prompt := st.chat_input("Type your message..."):
    # Show user input immediately
    st.chat_message("user").write(prompt)
    st.session_state["messages"].append({"role": "user", "text": prompt})

    # === Gemini Chat Endpoint ===
    url = "https://generativelanguage.googleapis.com/v1beta/chat/completions"
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {API_KEY}"
    }

    # Build request body
    body = {
        "model": "gemini-2.0-flash",  # Change if ListModels shows another name
        "messages": [
            {"role": "user", "content": prompt}
        ]
    }

    try:
        # ✅ NO params here
        resp = requests.post(url, headers=headers, json=body)

        if resp.status_code == 200:
            j = resp.json()
            # Extract assistant reply (OpenAI-style response)
            answer = j["choices"][0]["message"]["content"]
        else:
            answer = f"Error {resp.status_code}: {resp.text}"

    except Exception as e:
        answer = f"Request failed: {e}"

    # Show assistant reply
    st.chat_message("assistant").write(answer)
    st.session_state["messages"].append({"role": "assistant", "text": answer})
