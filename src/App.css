import React, { useState, useEffect } from "react";
import { HubConnectionBuilder } from "@microsoft/signalr";

function App() {
    const [connection, setConnection] = useState(null);
    const [messages, setMessages] = useState([]);
    const [message, setMessage] = useState("");
    const [user, setUser] = useState("");

    useEffect(() => {
        const connect = new HubConnectionBuilder()
            .withUrl("http://localhost:5000/chathub")
            .withAutomaticReconnect()
            .build();

        connect.start()
            .then(() => {
                console.log("SignalR connected!");

                connect.on("ReceiveMessage", (user, message) => {
                    setMessages((prev) => [...prev, { user, message }]);
                });
            })
            .catch((error) => console.error("Connection failed: ", error));

        setConnection(connect);

        return () => {
            connect.stop();
        };
    }, []);

    const sendMessage = async () => {
        if (message && user) {
            const newMessage = { user, content: message };

            // Save to backend
            await fetch("http://localhost:5000/api/message", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(newMessage),
            });

            setMessage("");
        }
    };

    return (
        <div style={{ padding: "20px" }}>
            <h1>Realtime Chat</h1>
            <input
                type="text"
                placeholder="Kullanıcı Adı"
                value={user}
                onChange={(e) => setUser(e.target.value)}
                style={{ marginBottom: "10px", display: "block" }}
            />
            <div>
                <input
                    type="text"
                    placeholder="Mesaj yaz..."
                    value={message}
                    onChange={(e) => setMessage(e.target.value)}
                />
                <button onClick={sendMessage}>Gönder</button>
            </div>
            <div style={{ marginTop: "20px" }}>
                <h3>Mesajlar:</h3>
                <ul>
                    {messages.map((msg, index) => (
                        <li key={index}>
                            <strong>{msg.user}:</strong> {msg.message}
                        </li>
                    ))}
                </ul>
            </div>
        </div>
    );
}

export default App;
