<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Nysa Chatbot</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
:root { 
    --primary-color: #FFD700; 
    --secondary-color: #FDB931; 
    --background-dark: #1a1a1f; 
    --text-light: #ffffff; 
    --text-dark: #000000; 
    --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color)); 
    --error-color: #ff4444; 
    --success-color: #00C851; 
}

* { 
    box-sizing: border-box; 
    margin: 0; 
    padding: 0; 
}

body { 
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif; 
    background-color: var(--background-dark); 
    color: var(--text-light); 
    line-height: 1.6; 
    overflow-x: hidden; 
}

.header { 
    padding: 8px 16px; 
    background: rgba(26, 26, 31, 0.95); 
    position: fixed; 
    width: 100%; 
    top: 0; 
    left: 0; 
    z-index: 30; 
    border-bottom: 2px solid var(--primary-color); 
    backdrop-filter: blur(10px); 
    height: 60px; 
}

.header-content { 
    max-width: 1400px; 
    margin: 0 auto; 
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
    height: 100%; 
}

.logo { 
    font-size: 1.8em; 
    font-weight: 900; 
    background: var(--accent-gradient); 
    -webkit-background-clip: text; 
    color: transparent; 
    letter-spacing: 1.2px; 
}

.menu-button { 
    padding: 10px 20px; 
    background: var(--accent-gradient); 
    border: none; 
    border-radius: 50px; 
    color: var(--text-dark); 
    font-weight: 600; 
    cursor: pointer; 
    transition: all 0.3s ease; 
    font-size: 0.95em; 
}

.chatbot-container { 
    position: fixed; 
    top: 60px; 
    left: 0; 
    right: 0; 
    bottom: 60px; 
    background: rgba(26, 26, 31, 0.95); 
    display: flex; 
    flex-direction: column; 
}

.chat-header { 
    padding: 10px; 
    background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1)); 
    border-bottom: 1px solid rgba(255, 215, 0, 0.2); 
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
}

.chat-status { 
    display: flex; 
    align-items: center; 
    color: var(--primary-color); 
    font-weight: 600; 
}

.status-dot { 
    width: 8px; 
    height: 8px; 
    background: var(--primary-color); 
    border-radius: 50%; 
    margin-right: 8px; 
    animation: pulse 2s infinite; 
}

.chat-messages { 
    flex: 1; 
    overflow-y: auto; 
    padding: 20px; 
    scroll-behavior: smooth; 
}

.message {
    font-family: 'Poppins', sans-serif;
    display: flex;
    gap: 10px;
    margin-bottom: 16px;
    animation: messageSlide 0.3s ease-out;
    width: 100%;
    max-width: 100%;
}

/* Bot message styles */
.bot-message {
    align-self: flex-start;
    width: 100%;
    max-width: 100%;
}

.bot-message .message-content {
    background: transparent;
    padding: 16px;
    border-radius: 0;
    color: var(--text-light);
    font-size: 16px;
    line-height: 1.6;
    position: relative;
    width: 100%;
    max-width: 100%;
}

/* User message styles */
.user-message {
    align-self: flex-end;
    flex-direction: row-reverse;
    width: 100%;
    max-width: 100%;
}

.user-message .message-content {
    background: rgba(255, 215, 0, 0.1);
    border-radius: 16px;
    width: auto;
    max-width: 80%;
}


/* Welcome message styles */
.welcome-message {
    display: flex;
    align-items: flex-start;
    gap: 12px;
    margin-bottom: 16px;
    width: 20%;
    max-width: 100%;
}

.welcome-avatar {
  width: 40px;
  height: 40px;
  min-width: 40px;
  min-height: 40px;
  background: rgba(255, 215, 0, 0.1);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--primary-color);
  flex-shrink: 0;
}

.welcome-content {
    background: rgba(255, 255, 255, 0.05);
    padding: 12px 16px;
    border-radius: 16px;
    color: var(--text-light);
    max-width: 100%;
    width: 100%;
}

.welcome-content p {
  margin: 0;
  font-size: 16px;
  line-height: 1.5;
}

/* Animation for welcome message */
.welcome-message {
    animation: messageSlide 0.3s ease-out;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .welcome-message {
        width: 80%;
    }
}

/* Fade out animation */
.welcome-message.fade-out {
    opacity: 0;
    transition: opacity 0.3s ease-out;
}

/* Add markdown styling for bot messages */
.bot-message .message-content pre {
    background: rgba(255, 255, 255, 0.05);
    border-radius: 6px;
    padding: 12px;
    margin: 8px 0;
    overflow-x: auto;
}

.bot-message .message-content code {
    background: rgba(255, 255, 255, 0.1);
    padding: 2px 6px;
    border-radius: 4px;
    font-family: 'Menlo', 'Monaco', 'Courier New', monospace;
}

.bot-message .message-content p {
    margin-bottom: 16px;
}

.bot-message .message-content ul,
.bot-message .message-content ol {
    margin: 16px 0;
    padding-left: 24px;
}

.bot-message .message-content li {
    margin-bottom: 8px;
}

/* Add a subtle separator between messages */
.bot-message:not(:last-child) {
    border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    padding-bottom: 16px;
}

/* Remove any padding that might be causing the issue */
.chat-messages {
    padding: 20px 10px !important;
}

/* Responsive adjustments */
@media (max-width: 768px) {
    .bot-message .message-content,
    .user-message .message-content,
    .welcome-content {
        padding: 12px;
    }
    
    .user-message .message-content {
        max-width: 90%;
    }
}

.message-content { 
    background: rgba(255, 255, 255, 0.05); 
    padding: 12px 16px; 
    border-radius: 16px; 
    color: var(--text-light); 
}

.user-message .message-content { 
    background: rgba(255, 215, 0, 0.1); 
}

.chat-input-container {
  padding: 20px;
  background: rgba(26, 26, 31, 0.98);
  border-top: 1px solid rgba(255, 215, 0, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
}

.input-with-voice {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#chatInput {
  width: 100%;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 20px 50px 20px 20px;
  color: var(--text-light);
  font-size: 0.95em;
}

.voice-button {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-button:hover {
  color: var(--secondary-color);
}

.send-message {
  padding: 20px;
  background: var(--accent-gradient);
  border: none;
  border-radius: 8px;
  color: #ffffff;
  cursor: pointer;
  transition: all 0.3s ease;
}

/* Desktop styles */
@media (min-width: 769px) {
  .chat-input-container {
    width: 60%;
    margin-left: auto;
    margin-right: auto;
  }
  
  #chatInput {
    width: 100%;
    height: 70px;
    font-size: 1.05rem;
    padding: 0 70px 0 24px;  /* Increased right padding for larger voice button */
    border-radius: 16px;
  }
  
  .voice-button {
    width: 60px;  /* Increased width */
    height: 60px; /* Increased height */
    right: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .voice-button i {
    font-size: 1.7em;  /* Increased font size */
  }
  
  .send-message {
    padding: 20px;
    height: 70px;
    width: 70px;  /* Made slightly wider */
    display: flex;
    align-items: center;
    justify-content: center;
  }
  
  .send-message i {
    font-size: 1.7em;  /* Increased font size */
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-input-container {
    width: 100%;
    padding: 10px;
  }
  
  #chatInput {
    width: 100%;
  }
  
  .voice-button i {
    font-size: 1.5em;
  }
  
  .send-message i {
    font-size: 1.5em;
  }
}

.alert { 
    position: fixed; 
    top: 20px; 
    right: 20px; 
    padding: 15px 20px; 
    border-radius: 8px; 
    color: var(--text-light); 
    z-index: 1000; 
    animation: slideIn 0.3s ease-out; 
}

.alert-success { 
    background: var(--success-color); 
}

.alert-error { 
    background: var(--error-color); 
}

.bottom-nav { 
    position: fixed; 
    bottom: 0; 
    left: 0; 
    right: 0; 
    height: 60px; 
    background: rgba(26, 26, 31, 0.98); 
    padding: 8px 0; 
    border-top: 1px solid rgba(255, 215, 0, 0.2); 
}

.nav-container { 
    display: flex; 
    justify-content: space-around; 
    align-items: center; 
    height: 100%; 
    max-width: 600px; 
    margin: 0 auto; 
}

.nav-item { 
    display: flex; 
    flex-direction: column; 
    align-items: center; 
    text-decoration: none; 
    color: #fff; 
    transition: all 0.3s ease; 
    padding: 5px; 
    cursor: pointer; 
    font-family: 'Poppins', sans-serif; 
}

.nav-item i { 
    color: #ffffff; 
    font-size: 20px; 
    margin-bottom: 4px; 
}

.nav-item span { 
    font-size: 12px; 
    font-family: 'Poppins', sans-serif; 
    font-weight: 600; 
    letter-spacing: 0.6px; 
    text-align: center; 
}

@keyframes pulse { 
    0% { opacity: 1; } 
    50% { opacity: 0.5; } 
    100% { opacity: 1; } 
}

@keyframes messageSlide { 
    from { opacity: 0; transform: translateY(10px); } 
    to { opacity: 1; transform: translateY(0); } 
}

@keyframes slideIn { 
    from { opacity: 0; transform: translateX(100px); } 
    to { opacity: 1; transform: translateX(0); } 
}

@media (max-width: 768px) { 
    .message { 
        max-width: 90%; 
    }
}

body > h1:first-of-type:not(.heading) { 
    display: none !important; 
}

.markdown-body h1:first-child { 
    display: none !important; 
}

.position-relative h1:first-child { 
    display: none !important; 
}
      
      .modal-overlay { 
          position: absolute; 
          top: 0; 
          left: 0; 
          width: 100%; 
          height: 100%; 
          background-color: rgba(0, 0, 0, 0.5); 
          display: flex; 
          justify-content: center; 
          align-items: center; 
          padding: 20px; 
      }
      
      .modal-content { 
          background-color: #1a1a1f; 
          border-radius: 12px; 
          width: 100%; 
          max-width: 600px; 
          max-height: 90vh; 
          overflow-y: auto; 
          box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15); 
      }
      
      .modal-header { 
          padding: 20px; 
          border-bottom: 1px solid #eee; 
          display: flex; 
          justify-content: space-between; 
          align-items: center; 
      }
      
      .modal-header h2 { 
          margin: 0; 
          font-size: 1.5rem; 
          color: #fffdfd; 
      }
            
      .modal-body { 
          padding: 20px; 
      }
      
      .menu-overlay { 
          display: none; 
          position: fixed; 
          top: 60px; 
          left: 0; 
          right: 0; 
          bottom: 60px; 
          background: rgba(26, 26, 31, 0.95); 
          z-index: 25; 
          padding: 0; 
          animation: fadeIn 0.3s ease-out; 
          overflow-y: auto; 
      }
      
      .menu-sections { 
          background: rgba(26, 26, 31, 0.95); 
          padding: 20px; 
      }
      
      .menu-section { 
        background: rgba(26, 26, 31, 0.95); 
          margin-bottom: 24px; 
      }
      
      .menu-section-title { 
          color: var(--primary-color); 
          font-size: 1.2em; 
          font-weight: 600; 
          margin-bottom: 12px; 
          padding-bottom: 8px; 
          border-bottom: 1px solid rgba(255, 215, 0, 0.2); 
      }
      
      .menu-items { 
          display: flex; 
          flex-direction: column; 
          gap: 8px; 
      }
      
      .menu-item { 
          padding: 12px 16px; 
          background: rgba(255, 255, 255, 0.05); 
          border-radius: 8px; 
          color: var(--text-light); 
          cursor: pointer; 
          transition: all 0.3s ease; 
          display: flex; 
          justify-content: space-between; 
          align-items: center; 
      }
      
      .menu-item:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .menu-item-price { 
          color: var(--primary-color); 
          font-weight: 600; 
      }
      
      .social-links { 
          display: flex; 
          gap: 12px; 
      }
      
      .social-link { 
          width: 40px; 
          height: 40px; 
          border-radius: 50%; 
          background: rgba(255, 255, 255, 0.05); 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          color: var(--text-light); 
          text-decoration: none; 
          transition: all 0.3s ease; 
      }
      
      .social-link:hover { 
          background: var(--primary-color); 
          color: var(--text-dark); 
      }
      
      .about-content { 
          line-height: 1.6; 
          color: #cccccc; 
      }
      
      .menu-close-button { 
          background: none; 
          border: none; 
          font-size: 1.5rem; 
          cursor: pointer; 
          color: #FFD700; 
          padding: 5px; 
      }

      .menu-item { 
          animation: slideIn 0.3s ease-out; 
          animation-fill-mode: both; 
      }
      
      .menu-item:nth-child(1) { 
          animation-delay: 0.1s; 
      }
      
      .menu-item:nth-child(2) { 
          animation-delay: 0.2s; 
      }
      
      .menu-item:nth-child(3) { 
          animation-delay: 0.3s; 
      }
      
      .menu-item:nth-child(4) { 
          animation-delay: 0.4s; 
      }
      
      .menu-item:nth-child(5) { 
          animation-delay: 0.5s; 
      }
      
      @keyframes slideIn { 
          from { 
              opacity: 0; 
              transform: translateX(-20px); 
          } 
          to { 
              opacity: 1; 
              transform: translateX(0); 
          } 
      }
      
      .deepchat-item { 
          background-color: #ff0000; 
          color: rgb(255, 255, 255); 
          border-radius: 8px; 
          padding: 10px; 
          text-align: center; 
      }
      
      .deepchat-item i { 
          color: rgb(255, 255, 5); 
      }
      
      .deepchat-item:hover { 
          background-color: #ff0606; 
          color: rgb(255, 255, 255); 
      }
      
      .deepchat-item span { 
          font-weight: 600; 
          font-family: 'Poppins', -apple-system, BlinkMacSystemFont, Roboto, sans-serif; 
          letter-spacing: 0.3px; 
      }
      
      .confirmation-icon { 
          font-size: 80px; 
          color: #4CAF50; 
          margin-bottom: 20px; 
      }
      
      .confirmation-icon i { 
          display: block; 
      }
      
      .alert { 
          position: fixed; 
          top: 20px; 
          left: 50%; 
          transform: translateX(-50%); 
          padding: 10px 20px; 
          border-radius: 5px; 
          z-index: 1100; 
      }
      
      .alert-success { 
          background-color: #4CAF50; 
          color: white; 
      }
      
      .alert-error { 
          background-color: #f44336; 
          color: white; 
      }
      
      .location-links, .partner-links { 
          display: flex; 
          flex-direction: column; 
      } 
      
      .location-link, .partner-link { 
          text-decoration: none; 
          color: #ffffff; 
          padding: 8px 10px; 
          transition: all 0.3s ease; 
          border-radius: 4px; 
      } 
      
      .location-link:hover, .partner-link:hover { 
          background-color: #f0f0f0; 
          color: #333; 
      } 
      
      .location-link i, .partner-link i { 
          margin-right: 10px; 
          color: #fff; 
      } 
      
      .location-link:hover i, .partner-link:hover i { 
          color: #2c7cd1; 
      }
      
      .header-button-group { 
          display: flex; 
          align-items: center; 
          gap: 5px; 
      }
      
      .town-button, 
      .nysa-feed-button, 
      .news-button { 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          padding: 15px; 
          background: none; 
          border: none; 
          color: var(--primary-color); 
          cursor: pointer; 
          transition: background-color 0.3s ease; 
          border-radius: 4px; 
          width: 40px; 
          height: 40px; 
      }
      
      .town-button:hover, 
      .nysa-feed-button:hover, 
      .news-button:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .town-button i, 
      .nysa-feed-button i, 
      .news-button i { 
          font-size: 18px; 
      }
      
      </style>        
        </head>
        <body>
            <header class="header">
                <div class="header-content">
                    <a href="https://nysaabhi.github.io/A2" class="logo">Nysa</a>
                    <button class="menu-button" id="menuButton">
                        <i class="fas fa-bars"></i>
                     
                    </button>
            </div>
            </header>
            
            <div class="chatbot-container">
                <div class="chat-header">
                    <div class="chat-status">
                        <span class="status-dot"></span>
                        Nysa AI Assistant
                    </div>
                    <div class="header-button-group">
                        <button class="town-button" id="townButton">
                            <i class="fas fa-map-marker-alt"></i>
                        </button>
                        <button class="nysa-feed-button" id="nysaFeedButton">
                          <i class="fas fa-image"></i>
                        </button>
                                  <button class="news-button" id="newsButton">
                          <i class="fas fa-newspaper"></i>
                        </button>
                                          </div>
                </div>
        
                <div class="chat-messages" id="chatMessages">
                    <div class="category-grid" id="categoryGrid">
                        <!-- Categories will be dynamically populated -->
                    </div>
                             
                    <div class="woohoo" id="woohoo">
                        <div class="hahaha">
                    </div>
                </div>
            </div>

            
            <!-- Bottom Navigation -->
            <nav class="bottom-nav">
                <div class="nav-container">
                    <div class="nav-item" data-page="services">
                           <i class="fas fa-user-tie"></i>
                        <span>Service</span>
                    </div>
                    <div class="nav-item" data-page="brand">
                        <i class="fas fa-store"></i>
                        <span>Stores</span>
                    </div>
                    <div class="nav-item deepchat-item" data-page="chat">
                            <i class="fas fa-message"></i>
                            <span>Nysa Chat</span>
                        </div>
                        <div class="nav-item" data-page="places">
                            <i class="fas fa-landmark"></i>
                            <span>Places</span>
                       </div>
                       <div class="nav-item" data-page="real-estate" onclick="showRealEstateOverlay()">
                        <i class="fas fa-home"></i>
                        <span>Real Estate</span>
                    </div>
                    </div>
                </nav>
    
            <div class="menu-overlay" id="menuOverlay">
                <div class="menu-sections">
                    <div class="menu-section">
                        <div class="menu-section-title">About Us</div>
                        <div class="about-content">
                  Nysa is a revolutionary digital platform that connects users with a diverse range of stores, services, opportunities, and innovations—all in one place. From groceries, fashion, and restaurants to real estate, jobs, healthcare, and education, Nysa makes everyday life simpler, smarter, and more interactive. Users can explore live feeds, place orders, interact with posts, access exclusive offers, and engage with businesses like never before. With features like VR views, gamified actions, and seamless chat, Nysa is reshaping how people discover and interact with the world around them. Nysa isn’t just an platform—it’s your gateway to a smarter lifestyle.
                        </div>   
                    </div>
                         
                    <div class="menu-section">
                        <div class="menu-section-title">Connect With Us</div>
                        <div class="social-links">
                            <a href="https://nysaabhi.github.io/chat" class="social-link">
                                <i class="fab fa-facebook-f"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-twitter"></i>
                            </a>
                            <a href="https://www.instagram.com/nysa.world?igsh=MWNkMWN5cDBvNHhzdQ==" class="social-link">
                                <i class="fab fa-instagram"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-linkedin-in"></i>
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Our Locations</div>
                        <div class="location-links">
                            <a href="location.html" class="location-link">
                                <i class="fas fa-map-marker-alt"></i> India
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">List On Nysa</div>
                        <div class="partner-links">
                            <a href="https://www.example1.com" class="partner-link">
                                <i class="fas fa-tools"></i> Service Provider
                            </a>
                            <a href="https://www.example2.com" class="partner-link">
                                <i class="fas fa-store"></i> Stores
                            </a>
                            <a href="https://www.example3.com" class="partner-link">
                                <i class="fas fa-map-marker-alt"></i> Public Places
                            </a>
                            <a href="https://www.example4.com" class="partner-link">
                                <i class="fas fa-building"></i> Real Estate
                            </a>
                        </div>
                    </div>
                </div>
            </div> 

            <div class="chat-input-container">
                <div class="input-with-voice">
                    <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
                    <button class="voice-button" id="voiceButton">
                        <i class="fas fa-microphone"></i>
                    </button>
                </div>
                <button class="send-message" id="sendButton">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </div>
            </div>
      
    
        
            <script src="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.js"></script>
            <script>                        
// Complete Chat System Implementation
const searchConfig = {
  promptDatabase: {
  'where can i recharge my phone': {
    answer: 'You can recharge your phone using Paytm, PhonePe, or Google Pay.',
    clickButtons: [
      {
        label: 'Recharge',
        url: 'https://paytm.com/recharge',
        icon: 'fa-mobile'
      }
    ],
    elaborateInfo: {
      'Payment Methods': [
        'UPI payments through digital wallets',
        'Credit/Debit cards',
        'Net banking',
        'Wallet balance'
      ],
      'Process': [
        'Select operator and circle',
        'Enter mobile number',
        'Choose recharge plan',
        'Select payment method',
        'Complete payment'
      ],
      'Security': [
        'Encrypted transactions',
        'OTP verification',
        'Payment protection guarantee'
      ]
    }
  },
  
  'book cab': {
    answer: 'You can book a cab using Uber or Ola apps.',
    clickButtons: [
      {
        label: 'Uber',
        url: '/redirect-uber',
        icon: 'fa-car'
      },
      {
        label: 'Ola',
        url: '/redirect-ola',
        icon: 'fa-taxi'
      }
    ],
    elaborateInfo: {
      'Available Services': [
        'Mini/Sedan/SUV options',
        'Shared rides',
        'Rentals',
        'Outstation'
      ],
      'Booking Steps': [
        'Enter pickup location',
        'Choose destination',
        'Select car type',
        'Confirm booking'
      ],
      'Payment Options': [
        'Cash payment',
        'Digital wallets',
        'Cards',
        'UPI'
      ]
    }
  },

  'book train': {
      answer: 'You can book a train ticket. Please provide the source station, destination station, and travel date.',
      clickButtons: [
        {
          label: 'Check Trains',
          url: 'https://www.confirmtkt.com/rbooking/trains/from/{from}/to/{to}/{date}',
          icon: 'fa-train'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter your source station code (e.g., BPL for Bhopal)',
          'Enter your destination station code (e.g., MMCT for Mumbai)',
          'Enter your travel date in DD-MM-YYYY format'
        ]
      }
    },

    'book flight': {
      answer: 'You can book a flight ticket. Please provide the source airport, destination airport, and travel dates.',
      clickButtons: [
        {
          label: 'Check Flights',
          url: 'https://www.skyscanner.co.in/transport/flights/{from}/{to}/{departureDate}/{returnDate}/?adultsv2=1&cabinclass=economy&childrenv2=&ref=home&rtn=1&preferdirects=false&outboundaltsenabled=false&inboundaltsenabled=false',
          icon: 'fa-plane'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter your source airport code (e.g., BHO for Bhopal)',
          'Enter your destination airport code (e.g., BOM for Mumbai)',
          'Enter your departure date in DDMMYY format',
          'Enter your return date in DDMMYY format (if round trip)'
        ]
      }
    },

    'register domain': {
      answer: 'You can register a domain. Please provide the domain name you want to check.',
      clickButtons: [
        {
          label: 'Check Domain',
          url: 'https://www.godaddy.com/en-in/domainsearch/find?domainToCheck={domain}',
          icon: 'fa-globe'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter the domain name you want to register (e.g., RadhaKrishna.com)'
        ]
      }
    },

    'check stocks': {
      answer: 'You can check stock prices. Please provide the stock name or ticker symbol.',
      clickButtons: [
        {
          label: 'Check Stock',
          url: 'https://upstox.com/stocks/{stock}',
          icon: 'fa-chart-line'
        }
      ],
      elaborateInfo: {
        'Instructions': [
          'Enter the stock name or ticker symbol (e.g., TITAN for Titan Company Limited)'
        ]
      }
    },


  'apply for aadhar card': {
    answer: 'You can apply for Aadhar card online or visit nearest enrollment center.',
    clickButtons: [
      {
        label: 'Apply',
        url: 'https://uidai.gov.in/my-aadhaar/appointment-booking.html',
        icon: 'fa-id-card'
      },
      {
        label: 'Find',
        url: 'https://appointments.uidai.gov.in/centerSearch',
        icon: 'fa-location-dot'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity (Passport/DL/Voter ID)',
        'Proof of Address (Utility bill/Rent agreement)',
        'Proof of Birth (Birth certificate/School certificate)',
        'Recent passport size photograph'
      ],
      'Application Steps': [
        'Book appointment online or visit center',
        'Submit required documents',
        'Biometric capture (fingerprints, iris, photograph)',
        'Receive acknowledgment slip',
        'Track application status online'
      ],
      'Important Notes': [
        'Free of cost for first time applicants',
        'Mandatory biometric update for children at 5 and 15 years',
        'SMS notifications for status updates',
        'E-Aadhaar available for download after generation'
      ]
    }
  },

  'apply for pan card': {
    answer: 'You can apply for PAN card online through NSDL or UTITSL portal.',
    clickButtons: [
      {
        label: 'Apply Online',
        url: 'https://www.onlineservices.nsdl.com/paam/endUserRegisterContact.html',
        icon: 'fa-credit-card'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity',
        'Proof of Address',
        'Proof of Date of Birth',
        'Recent passport size photograph',
        'Digital signature (for online application)'
      ],
      'Application Process': [
        'Fill online application form',
        'Upload required documents',
        'Pay application fee',
        'Schedule in-person verification if required',
        'Track application status'
      ],
      'Fee Structure': [
        'Regular application: ₹107',
        'Express delivery: Additional charges apply',
        'Digital signature charges if applicable'
      ]
    }
  },

  'open bank account': {
    answer: 'You can open a bank account online or visit nearest branch of preferred bank.',
    clickButtons: [
      {
        label: 'Compare Banks',
        url: '/compare-banks',
        icon: 'fas fa-building'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Valid ID proof (Aadhar/PAN/Passport)',
        'Address proof',
        'Passport size photographs',
        'Income proof (for certain accounts)'
      ],
      'Account Types': [
        'Savings Account',
        'Current Account',
        'Salary Account',
        'Fixed Deposit Account'
      ],
      'Process Steps': [
        'Choose account type and bank',
        'Fill application form',
        'Submit KYC documents',
        'Initial deposit (if required)',
        'Receive account kit'
      ],
      'Features to Compare': [
        'Minimum balance requirement',
        'Interest rates',
        'Online banking facilities',
        'Debit card features',
        'Banking charges'
      ]
    }
  },

  'file income tax returns': {
    answer: 'You can file your Income Tax Returns (ITR) online through the Income Tax portal.',
    clickButtons: [
      {
        label: 'File ITR',
        url: 'https://www.incometax.gov.in/iec/foportal',
        icon: 'fa-file-invoice-dollar'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Form 16 from employer',
        'Bank statements',
        'Investment proofs',
        'Property tax receipts',
        'Loan statements'
      ],
      'Filing Process': [
        'Register on Income Tax portal',
        'Choose correct ITR form',
        'Fill in tax details',
        'Upload required documents',
        'Verify using Aadhaar OTP/Digital Signature'
      ],
      'Important Dates': [
        'Financial Year: April 1 to March 31',
        'Due date for individuals: July 31',
        'Late filing fees applicable after due date'
      ],
      'Additional Information': [
        'Tax saving investments under 80C',
        'Deductions available',
        'Penalty for non-filing',
        'Tax refund process'
      ]
    }
  },

  'apply for passport': {
    answer: 'You can apply for passport online through Passport Seva portal.',
    clickButtons: [
      {
        label: 'Apply Now',
        url: 'https://www.passportindia.gov.in',
        icon: 'fa-passport'
      }
    ],
    elaborateInfo: {
      'Required Documents': [
        'Proof of Identity',
        'Proof of Address',
        'Birth Certificate',
        'Educational certificates',
        'Aadhaar card'
      ],
      'Application Steps': [
        'Register on Passport Seva portal',
        'Fill online application form',
        'Schedule appointment',
        'Pay fees online',
        'Visit PSK for verification'
      ],
      'Fee Structure': [
        'Normal Application: ₹1500',
        'Tatkal Application: ₹3500',
        'Additional charges for SMS updates'
      ],
      'Processing Time': [
        'Normal: 30-45 days',
        'Tatkal: 1-3 days',
        'Subject to police verification'
      ]
    }
  }
},

  couponDatabase: {
    keywords: ['coupon', 'coupons', 'discount', 'offer', 'code', 'deal', 'deals', 'promo'],
    brands: ['amazon', 'flipkart', 'swiggy', 'myntra', 'zomato'],
    items: {
      'amazon': [
        {
          code: 'SAVE20',
          discount: '20% OFF',
          validity: '31 Jan 2025',
          maxDiscount: '₹2000',
          brand: 'Amazon',
          category: 'E-commerce',
          description: 'Get 20% off on all products',
          icon: 'fa-shopping-cart',
          eligibility: {
            'Minimum Purchase': '₹1000',
            'Valid Categories': [
              'Electronics',
              'Fashion',
              'Home & Kitchen'
            ],
            'Payment Methods': [
              'All payment methods accepted',
              'No cash on delivery'
            ],
            'User Eligibility': [
              'All users',
              'One time per user'
            ],
            'Exclusions': [
              'Not valid on lightning deals',
              'Cannot be combined with other offers'
            ]
          }
        },
        {
          code: 'NEWUSER50',
          discount: '50% OFF',
          validity: '31 Dec 2024',
          maxDiscount: '₹500',
          brand: 'Amazon',
          category: 'E-commerce',
          description: 'First order discount for new users',
          icon: 'fa-shopping-cart',
          eligibility: {
            'Minimum Purchase': '₹500',
            'Valid Categories': ['All categories'],
            'Payment Methods': ['All payment methods'],
            'User Eligibility': ['New users only'],
            'Exclusions': ['Some premium brands excluded']
          }
        }
      ],
      'flipkart': [
        {
          code: 'FLIP30',
          discount: '30% OFF',
          validity: '30 Jan 2025',
          maxDiscount: '₹1500',
          brand: 'Flipkart',
          category: 'E-commerce',
          description: 'Special discount on electronics',
          icon: 'fa-shopping-cart',
          eligibility: {
            'Minimum Purchase': '₹3000',
            'Valid Categories': ['Electronics only'],
            'Payment Methods': ['All except COD'],
            'User Eligibility': ['All users'],
            'Exclusions': ['Not valid on Apple products']
          }
        }
      ],
      'swiggy': [
        {
          code: 'TASTE40',
          discount: '40% OFF',
          validity: '15 Jan 2025',
          maxDiscount: '₹100',
          brand: 'Swiggy',
          category: 'Food Delivery',
          description: 'Food delivery discount',
          icon: 'fa-utensils',
          eligibility: {
            'Minimum Order': '₹200',
            'Valid Time': ['All day'],
            'Payment Methods': ['All digital payments'],
            'User Eligibility': ['All users'],
            'Exclusions': ['Not valid on delivery charges']
          }
        }
      ],
      'myntra': [
        {
          code: 'FASHION25',
          discount: '25% OFF',
          validity: '28 Feb 2025',
          maxDiscount: '₹1000',
          brand: 'Myntra',
          category: 'Fashion',
          description: 'Fashion & lifestyle discount',
          icon: 'fa-tshirt',
          eligibility: {
            'Minimum Purchase': '₹1499',
            'Valid Categories': ['All Fashion Categories'],
            'Payment Methods': ['All payment methods'],
            'User Eligibility': ['All users'],
            'Exclusions': ['Not valid on luxury brands']
          }
        }
      ],
      'zomato': [
        {
          code: 'ZFIRST',
          discount: '60% OFF',
          validity: '31 Jan 2025',
          maxDiscount: '₹150',
          brand: 'Zomato',
          category: 'Food Delivery',
          description: 'New user special discount',
          icon: 'fa-hamburger',
          eligibility: {
            'Minimum Order': '₹159',
            'Valid Time': ['11 AM to 11 PM'],
            'Payment Methods': ['Online payments only'],
            'User Eligibility': ['First-time users'],
            'Exclusions': ['Not valid on dine-in']
          }
        }
      ]
    }
  },

  realEstateDatabase: {
    properties: [
      {
        id: 1,
        title: "2 BHK Apartment in Bangalore",
        type: "Apartment",
        location: "Bangalore",
        price: "₹1.2 Cr",
        area: "1200 sq.ft",
        bedrooms: 2,
        bathrooms: 2,
        description: "A spacious 2 BHK apartment in the heart of Bangalore, close to IT hubs and amenities.",
        postedDate: "2023-10-01",
        contact: "+91 9876543210",
        bookingLink: "https://example.com/book/1", // Add a booking link
        whatsappLink: "https://wa.me/919876543210", // Add a WhatsApp link
        images: [
          "https://via.placeholder.com/800x600?text=Living+Room",
          "https://via.placeholder.com/800x600?text=Bedroom",
          "https://via.placeholder.com/800x600?text=Kitchen"
        ],
        amenities: ["Swimming Pool", "Gym", "Parking", "24/7 Security"],
        propertyInfo: {
          'Overview': [
            '2 BHK Apartment',
            '1200 sq.ft',
            'Ready to Move In'
          ],
          'Amenities': [
            'Swimming Pool',
            'Gym',
            'Parking',
            '24/7 Security'
          ],
          'Location Advantages': [
            'Close to IT Hubs',
            'Nearby Schools and Hospitals',
            'Easy Access to Public Transport'
          ],
          'Contact Details': [
            'Agent: John Doe',
            'Phone: +91 9876543210',
            'Email: john.doe@example.com'
          ]
        }
      },
      {
        id: 2,
        title: "3 BHK Villa in Hyderabad",
        type: "Villa",
        location: "Hyderabad",
        price: "₹2.5 Cr",
        area: "2500 sq.ft",
        bedrooms: 3,
        bathrooms: 3,
        description: "A luxurious 3 BHK villa with modern amenities and a private garden.",
        postedDate: "2023-09-25",
        contact: "+91 9876543211",
        bookingLink: "https://example.com/book/2", // Add a booking link
        whatsappLink: "https://wa.me/919876543211", // Add a WhatsApp link
        images: [
          "https://www.ferienhaus-ibiza.de/wp-content/uploads/2020/05/47-Kopie.jpg",
          "https://via.placeholder.com/800x600?text=Bedroom",
          "https://via.placeholder.com/800x600?text=Kitchen",
          "https://via.placeholder.com/800x600?text=Garden"
        ],
        amenities: ["Private Garden", "Swimming Pool", "Gym", "Parking", "24/7 Security"],
        propertyInfo: {
          'Overview': [
            '3 BHK Villa',
            '2500 sq.ft',
            'Ready to Move In'
          ],
          'Amenities': [
            'Private Garden',
            'Swimming Pool',
            'Gym',
            'Parking',
            '24/7 Security'
          ],
          'Location Advantages': [
            'Close to IT Hubs',
            'Nearby Schools and Hospitals',
            'Easy Access to Public Transport'
          ],
          'Contact Details': [
            'Agent: Jane Doe',
            'Phone: +91 9876543211',
            'Email: jane.doe@example.com'
          ]
        }
      }
      // Add more properties as needed
    ]
  },

  automotiveDatabase: {
    vehicles: [
      {
        id: 1,
        title: "Tata Tiago",
        type: "car",
        brand: "Tata",
        price: "₹5.60 Lakh",
        fuelType: "Petrol",
        transmission: "Manual",
        seatingCapacity: 5,
        description: "The Tata Tiago is a compact hatchback known for its stylish design, fuel efficiency, and advanced features.",
        postedDate: "2023-10-01",
        contact: "+91 9876543210",
        bookingLink: "https://www.cardekho.com/tata/tiago",
        whatsappLink: "https://wa.me/919876543210",
        images: [
        "https://imgd.aeplcdn.com/0X0/n/cw/ec/39345/tiago-exterior-right-front-three-quarter-6.jpeg?q=85",
    "https://tse2.mm.bing.net/th?id=OIP.h8kPvP94IIn-1XhXNJcoHgHaE8&pid=Api",
    "https://cars.tatamotors.com/images/tiago/ice/gallery/exterior/02.jpg"
        ],
        features: ["Touchscreen Infotainment", "ABS with EBD", "Dual Airbags", "Rear Parking Camera"],
        vehicleInfo: {
          'Overview': [
            'Compact Hatchback',
            'Petrol Engine',
            '5 Seater'
          ],
          'Specifications': [
            'Engine: 1.2L Revotron',
            'Mileage: 19.8 km/l',
            'Power: 85 bhp',
            'Torque: 113 Nm'
          ],
          'Features': [
            '7-inch Touchscreen',
            'Android Auto & Apple CarPlay',
            'Rear Parking Camera',
            'Dual Airbags'
          ],
          'Contact Details': [
            'Dealer: Tata Motors',
            'Phone: +91 9876543210',
            'Email: tata.motors@example.com'
          ]
        }
      },
      {
        id: 2,
        title: "Royal Enfield Classic 350",
        type: "bike",
        brand: "Royal Enfield",
        price: "₹1.93 Lakh",
        fuelType: "Petrol",
        transmission: "Manual",
        seatingCapacity: 2,
        description: "The Royal Enfield Classic 350 is a retro-styled motorcycle known for its thumping engine and timeless design.",
        postedDate: "2023-09-25",
        contact: "+91 9876543211",
        bookingLink: "https://www.bikewale.com/royalenfield-bikes/classic-350/",
        whatsappLink: "https://wa.me/919876543211",
        images: [
        "https://images-stag.jazelc.com/uploads/theautopian-m2en/2023-royal-enfield-hunter-350-fi-1.jpg",
    "https://tse3.mm.bing.net/th?id=OIP.JpkDwBrcvzjFjmW1rSm05gHaE8&pid=Api",
    "https://akm-img-a-in.tosshub.com/indiatoday/images/bodyeditor/202009/8_Dual_Channel_Airborne_Blue-x600.png?ILg3qU6tOsAJ2XaOzGfRlZitPC2SgI.c"
        ],
        features: ["Retro Design", "Thumping Engine", "ABS", "LED Lighting"],
        vehicleInfo: {
          'Overview': [
            'Retro-styled Motorcycle',
            'Petrol Engine',
            '2 Seater'
          ],
          'Specifications': [
            'Engine: 349cc Single Cylinder',
            'Mileage: 35 km/l',
            'Power: 20.2 bhp',
            'Torque: 27 Nm'
          ],
          'Features': [
            'Retro Design',
            'ABS',
            'LED Headlamp',
            'Tripper Navigation'
          ],
          'Contact Details': [
            'Dealer: Royal Enfield',
            'Phone: +91 9876543211',
            'Email: royal.enfield@example.com'
            ]
     }
    }
  ]
},

providerDatabase: {
    'wifi': [
      {
        id: 1,
        name: "Airtel Broadband",
        location: "Delhi",
        rating: 4.5,
        contact: "+91 1234567890",
        services: ["WiFi", "Broadband", "Fiber"],
        description: "Airtel offers high-speed broadband and WiFi services in Delhi.",
        metadata: {
          'Pricing': [
            'Basic Plan: ₹499/month (100 Mbps)',
            'Premium Plan: ₹799/month (300 Mbps)'
          ],
          'Coverage': [
            'Delhi NCR',
            'Gurgaon',
            'Noida'
          ],
          'Installation': [
            'Free installation',
            '24-hour setup'
          ]
        },
        actionButtons: [
          {
            label: 'Call',
            url: 'tel:+911234567890',
            icon: 'fa-phone'
          },
          {
            label: 'Website',
            url: 'https://www.airtel.in',
            icon: 'fa-globe'
          }
        ]
      },
      // Add more WiFi providers...
    ],
    'solar energy': [
      {
        id: 2,
        name: "Tata Power Solar",
        location: "Bhopal",
        rating: 4.7,
        contact: "+91 9876543210",
        services: ["Solar Panels", "Inverters", "Installation"],
        description: "Tata Power Solar provides solar energy solutions in Bhopal.",
        metadata: {
          'Products': [
            'Solar Panels: 300W to 500W',
            'Inverters: 1KW to 10KW'
          ],
          'Services': [
            'Installation',
            'Maintenance',
            'Consultation'
          ],
          'Warranty': [
            '25 years on panels',
            '5 years on inverters'
          ]
        },
        actionButtons: [
          {
            label: 'Call',
            url: 'tel:+919876543210',
            icon: 'fa-phone'
          },
          {
            label: 'Email',
            url: 'mailto:support@tatapowersolar.com',
            icon: 'fa-envelope'
          }
        ]
      }
      // Add more solar energy providers...
    ]
    // Add more categories...
  },

  diseaseDatabase: {
    diseases: [
      {
        id: 1,
        name: "Asthma",
        description: "A condition in which a person's airways become inflamed, narrow, and produce extra mucus, making breathing difficult.",
        symptoms: ["Shortness of breath", "Chest tightness", "Wheezing", "Coughing"],
        treatments: ["Inhalers", "Bronchodilators", "Anti-inflammatory medications"],
        prevention: ["Avoid allergens", "Quit smoking", "Regular exercise"],
        metadata: {
          'Overview': [
            'Chronic respiratory condition',
            'Affects airways and breathing',
            'Can be managed with proper treatment'
          ],
          'Symptoms': [
            'Shortness of breath',
            'Chest tightness',
            'Wheezing',
            'Coughing'
          ],
          'Treatments': [
            'Inhalers',
            'Bronchodilators',
            'Anti-inflammatory medications'
          ],
          'Prevention': [
            'Avoid allergens',
            'Quit smoking',
            'Regular exercise'
          ],
          'Emergency Signs': [
            'Severe shortness of breath',
            'Bluish lips or face',
            'Rapid worsening of symptoms'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.mayoclinic.org/diseases-conditions/asthma/symptoms-causes/syc-20369653',
            icon: 'fa-info-circle'
          },
          {
            label: 'Find a Doctor',
            url: 'https://www.mayoclinic.org/appointments',
            icon: 'fa-user-md'
          }
        ]
      },
      {
        id: 2,
        name: "Diabetes",
        description: "A chronic condition that affects how your body turns food into energy, leading to high blood sugar levels.",
        symptoms: ["Increased thirst", "Frequent urination", "Extreme fatigue", "Blurred vision"],
        treatments: ["Insulin therapy", "Oral medications", "Diet and exercise"],
        prevention: ["Healthy diet", "Regular exercise", "Weight management"],
        metadata: {
          'Overview': [
            'Chronic metabolic disorder',
            'Affects blood sugar regulation',
            'Type 1 and Type 2 diabetes'
          ],
          'Symptoms': [
            'Increased thirst',
            'Frequent urination',
            'Extreme fatigue',
            'Blurred vision'
          ],
          'Treatments': [
            'Insulin therapy',
            'Oral medications',
            'Diet and exercise'
          ],
          'Prevention': [
            'Healthy diet',
            'Regular exercise',
            'Weight management'
          ],
          'Complications': [
            'Heart disease',
            'Kidney damage',
            'Nerve damage'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.mayoclinic.org/diseases-conditions/diabetes/symptoms-causes/syc-20371444',
            icon: 'fa-info-circle'
          },
          {
            label: 'Find a Doctor',
            url: 'https://www.mayoclinic.org/appointments',
            icon: 'fa-user-md'
          }
        ]
      },
      // Add more diseases alphabetically...
    ]
  },
  
  coursesDatabase: {
    courses: [
      {
        id: 1,
        name: "Full Stack Web Development",
        description: "Learn full-stack web development with HTML, CSS, JavaScript, Node.js, and React.",
        category: "Web Development",
        duration: "6 months",
        fees: "$500",
        metadata: {
          'Overview': [
            'Comprehensive course covering front-end and back-end development',
            'Hands-on projects and real-world applications',
            'Suitable for beginners and intermediate learners'
          ],
          'Curriculum': [
            'HTML, CSS, JavaScript Basics',
            'Advanced JavaScript and ES6',
            'React for Front-End Development',
            'Node.js and Express for Back-End',
            'Database Management with MongoDB'
          ],
          'Instructors': [
            'John Doe - Senior Web Developer',
            'Jane Smith - Full Stack Engineer'
          ],
          'Certification': [
            'Certificate of Completion',
            'Project-based assessments'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://example.com/full-stack-course',
            icon: 'fa-info-circle'
          },
          {
            label: 'Enroll Now',
            url: 'https://example.com/enroll/full-stack-course',
            icon: 'fa-user-graduate'
          }
        ]
      },
      {
        id: 2,
        name: "Data Science with Python",
        description: "Master data science concepts with Python, including data analysis, machine learning, and visualization.",
        category: "Data Science",
        duration: "4 months",
        fees: "$400",
        metadata: {
          'Overview': [
            'Learn data analysis, machine learning, and data visualization',
            'Hands-on projects with real datasets',
            'Suitable for beginners and professionals'
          ],
          'Curriculum': [
            'Python Basics for Data Science',
            'Data Analysis with Pandas and NumPy',
            'Machine Learning with Scikit-Learn',
            'Data Visualization with Matplotlib and Seaborn',
            'Capstone Project'
          ],
          'Instructors': [
            'Dr. Emily Johnson - Data Scientist',
            'Michael Brown - Machine Learning Engineer'
          ],
          'Certification': [
            'Certificate of Completion',
            'Capstone Project Certification'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://example.com/data-science-course',
            icon: 'fa-info-circle'
          },
          {
            label: 'Enroll Now',
            url: 'https://example.com/enroll/data-science-course',
            icon: 'fa-user-graduate'
          }
        ]
      },
      // Add more courses...
    ]
  },

  franchiseDatabase: {
    franchises: [
      {
        id: 1,
        name: "McDonald's",
        description: "McDonald's is the world's largest chain of hamburger fast food restaurants, serving around 68 million customers daily in 119 countries.",
        category: "Fast Food",
        investmentRange: "$1,000,000 - $2,300,000",
        initialFranchiseFee: "$45,000",
        royaltyFee: "4%",
        metadata: {
          'Overview': [
            'Global fast food chain',
            'Specializes in hamburgers, fries, and beverages',
            'Over 38,000 locations worldwide'
          ],
          'Requirements': [
            'Minimum net worth: $500,000',
            'Liquid assets: $250,000',
            'Experience in restaurant management preferred'
          ],
          'Support': [
            'Comprehensive training programs',
            'Ongoing operational support',
            'Marketing and advertising assistance'
          ],
          'Benefits': [
            'Strong brand recognition',
            'Proven business model',
            'Access to global supply chain'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.mcdonalds.com/us/en-us/franchising.html',
            icon: 'fa-info-circle'
          },
          {
            label: 'Apply Now',
            url: 'https://www.mcdonalds.com/us/en-us/franchising/apply.html',
            icon: 'fa-file-alt'
          }
        ]
      },
      {
        id: 2,
        name: "Domino's Pizza",
        description: "Domino's Pizza is an American multinational pizza restaurant chain founded in 1960. It is the second-largest pizza chain in the United States and the largest worldwide.",
        category: "Pizza",
        investmentRange: "$145,000 - $500,000",
        initialFranchiseFee: "$10,000",
        royaltyFee: "5.5%",
        metadata: {
          'Overview': [
            'Global pizza delivery chain',
            'Specializes in pizza, pasta, and sides',
            'Over 17,000 locations worldwide'
          ],
          'Requirements': [
            'Minimum net worth: $250,000',
            'Liquid assets: $75,000',
            'Experience in food service preferred'
          ],
          'Support': [
            'Comprehensive training programs',
            'Ongoing operational support',
            'Marketing and advertising assistance'
          ],
          'Benefits': [
            'Strong brand recognition',
            'Proven business model',
            'Access to global supply chain'
          ]
        },
        actionButtons: [
          {
            label: 'Learn More',
            url: 'https://www.dominos.com/franchising',
            icon: 'fa-info-circle'
          },
          {
            label: 'Apply Now',
            url: 'https://www.dominos.com/franchising/apply',
            icon: 'fa-file-alt'
          }
        ]
      },
      // Add more franchises...
    ]
  },

  loanProvidersDatabase: {
        providers: [
            {
                id: 1,
                name: "HDFC Bank",
                loanTypes: ["Personal", "Home", "Business"],
                loanAmount: { min: 10000, max: 5000000 },
                interestRate: { min: 8.5, max: 15 },
                processingFees: "1% of loan amount or ₹2,000 (whichever is higher)",
                tenure: { min: 1, max: 20 },
                emiOptions: "Flexible EMI options available",
                collateralRequired: false,
                eligibilityCriteria: [
                    "Minimum salary: ₹25,000 per month",
                    "Credit score: 750 or above",
                    "Stable employment history"
                ],
                documentsRequired: [
                    "PAN Card",
                    "Aadhaar Card",
                    "Salary slips (last 3 months)",
                    "Bank statements (last 6 months)"
                ],
                topLenders: ["HDFC Bank", "ICICI Bank", "Axis Bank"],
                governmentSchemes: false,
                processingTime: "3-5 working days",
                prepaymentCharges: "2% of outstanding amount",
                applyNowLink: "https://www.hdfcbank.com/personal/loans",
                metadata: {
                    'Overview': [
                        'HDFC Bank offers a wide range of loan products with competitive interest rates.',
                        'Flexible repayment options and quick processing.'
                    ],
                    'Benefits': [
                        'No collateral required for personal loans',
                        'Low processing fees',
                        'Quick approval and disbursal'
                    ],
                    'Contact Details': [
                        'Customer Care: 1800 202 6161',
                        'Email: loans@hdfcbank.com'
                    ]
                }
            },
            {
                id: 2,
                name: "ICICI Bank",
                loanTypes: ["Personal", "Home", "Business"],
                loanAmount: { min: 50000, max: 10000000 },
                interestRate: { min: 9, max: 16 },
                processingFees: "1.5% of loan amount or ₹3,000 (whichever is higher)",
                tenure: { min: 1, max: 25 },
                emiOptions: "Customizable EMI plans",
                collateralRequired: false,
                eligibilityCriteria: [
                    "Minimum salary: ₹30,000 per month",
                    "Credit score: 700 or above",
                    "Stable employment history"
                ],
                documentsRequired: [
                    "PAN Card",
                    "Aadhaar Card",
                    "Salary slips (last 3 months)",
                    "Bank statements (last 6 months)"
                ],
                topLenders: ["ICICI Bank", "HDFC Bank", "Axis Bank"],
                governmentSchemes: false,
                processingTime: "5-7 working days",
                prepaymentCharges: "3% of outstanding amount",
                applyNowLink: "https://www.icicibank.com/loans",
                metadata: {
                    'Overview': [
                        'ICICI Bank provides a variety of loan options with flexible repayment terms.',
                        'Competitive interest rates and quick processing.'
                    ],
                    'Benefits': [
                        'No collateral required for personal loans',
                        'Low processing fees',
                        'Quick approval and disbursal'
                    ],
                    'Contact Details': [
                        'Customer Care: 1800 1080',
                        'Email: loans@icicibank.com'
                    ]
                }
            },
            // Add more loan providers as needed
        ]
    },

  movieDatabase: {
    movies: [
      {
        id: 1,
        title: "Machante Malakha",
        releaseDate: "Feb 27, 2025",
        status: "upcoming",
        genre: ["Drama", "Romance"],
        cast: ["Dileesh Pothan", "Soubin Shahir", "Dhyan Sreenivasan", "Namitha Pramod"],
        description: "A heartwarming tale of love and relationships.",
        metadata: {
          'Director': ['Dileesh Pothan'],
          'Producer': ['Soubin Shahir'],
          'Music': ['Dhyan Sreenivasan'],
          'Runtime': ['2h 15m']
        },
        actionButtons: [
          {
            label: 'Watch Trailer',
            url: 'https://www.youtube.com/watch?v=example',
            icon: 'fa-play-circle'
          },
          {
            label: 'Book Tickets',
            url: 'https://www.bookmyshow.com/machante-malakha',
            icon: 'fa-ticket-alt'
          }
        ]
      },
      {
        id: 2,
        title: "The Brutalist",
        releaseDate: "Feb 28, 2025",
        status: "upcoming",
        genre: ["Drama"],
        cast: ["Adrien Brody", "Felicity Jones", "Guy Pearce", "Joe Alwyn"],
        description: "A gripping drama about the struggles of an architect.",
        metadata: {
          'Director': ['Adrien Brody'],
          'Producer': ['Felicity Jones'],
          'Music': ['Guy Pearce'],
          'Runtime': ['2h 30m']
        },
        actionButtons: [
          {
            label: 'Watch Trailer',
            url: 'https://www.youtube.com/watch?v=example',
            icon: 'fa-play-circle'
          },
          {
            label: 'Book Tickets',
            url: 'https://www.bookmyshow.com/the-brutalist',
            icon: 'fa-ticket-alt'
          }
        ]
      },
      // Add more movies...
    ]
  },

  jobsDatabase: {
  jobs: [
    {
      id: 1,
      title: "Software Developer",
      company: "Tech Corp",
      location: "Bangalore",
      salary: "₹120,000 per month",
      type: "Full-time",
      experience: "2-5 years",
      skills: ["JavaScript", "React", "Node.js"],
      description: "We are looking for a skilled Software Developer to join our team...",
      postedDate: "2023-10-01",
      applyLink: "https://techcorp.com/careers",
      jobInfo: {
        'Responsibilities': [
          'Develop and maintain web applications using JavaScript, React, and Node.js',
          'Collaborate with cross-functional teams to define, design, and ship new features',
          'Write clean, maintainable, and efficient code'
        ],
        'Requirements': [
          '2-5 years of experience in software development',
          'Proficient in JavaScript, React, and Node.js',
          'Strong understanding of web technologies and frameworks'
        ],
        'Benefits': [
          'Competitive salary and benefits',
          'Flexible working hours',
          'Opportunities for professional growth'
        ],
        'Application Process': [
          'Submit your application online',
          'Initial screening interview',
          'Technical assessment',
          'Final interview with the hiring manager'
        ]
      }
    },
    {
      id: 2,
      title: "Data Scientist",
      company: "Data Insights",
      location: "Hyderabad",
      salary: "₹150,000 per month",
      type: "Full-time",
      experience: "3-6 years",
      skills: ["Python", "Machine Learning", "Data Analysis"],
      description: "Join our team as a Data Scientist and work on cutting-edge AI projects...",
      postedDate: "2023-09-25",
      applyLink: "https://datainsights.com/careers",
      jobInfo: {
        'Responsibilities': [
          'Analyze large datasets to extract actionable insights',
          'Develop machine learning models for predictive analytics',
          'Collaborate with stakeholders to define business problems and solutions'
        ],
        'Requirements': [
          '3-6 years of experience in data science or related fields',
          'Proficient in Python and machine learning libraries (e.g., TensorFlow, Scikit-learn)',
          'Strong understanding of statistical analysis and data visualization'
        ],
        'Benefits': [
          'Competitive salary with performance bonuses',
          'Health insurance and wellness programs',
          'Access to cutting-edge tools and technologies'
        ],
        'Application Process': [
          'Online application submission',
          'Technical screening and coding test',
          'Case study presentation',
          'Final interview with the data science team'
        ]
      }
    },
    {
      id: 3,
      title: "Bank Manager",
      company: "National Bank",
      location: "Bhopal",
      salary: "₹100,000 per month",
      type: "Full-time",
      experience: "5-10 years",
      skills: ["Banking", "Finance", "Management"],
      description: "We are hiring a Bank Manager to oversee our Bhopal branch...",
      postedDate: "2023-10-05",
      applyLink: "https://nationalbank.com/careers",
      jobInfo: {
        'Responsibilities': [
          'Oversee daily operations of the bank branch',
          'Manage a team of banking professionals',
          'Ensure compliance with banking regulations and policies'
        ],
        'Requirements': [
          '5-10 years of experience in banking or financial services',
          'Strong leadership and team management skills',
          'In-depth knowledge of banking products and services'
        ],
        'Benefits': [
          'Attractive salary with performance incentives',
          'Retirement savings plan',
          'Paid time off and holidays'
        ],
        'Application Process': [
          'Submit your resume and cover letter',
          'Initial interview with HR',
          'Assessment center evaluation',
          'Final interview with regional management'
        ]
      }
    },
    {
      id: 4,
      title: "Police Officer",
      company: "Madhya Pradesh Police",
      location: "Madhya Pradesh",
      salary: "₹80,000 per month",
      type: "Government",
      experience: "0-2 years",
      skills: ["Law Enforcement", "Physical Fitness", "Public Safety"],
      description: "Join the Madhya Pradesh Police force and serve the community...",
      postedDate: "2023-09-30",
      applyLink: "https://mppolice.gov.in/careers",
      jobInfo: {
        'Responsibilities': [
          'Maintain public order and safety',
          'Enforce laws and regulations',
          'Respond to emergencies and incidents'
        ],
        'Requirements': [
          'Physical fitness and endurance',
          'Strong sense of integrity and ethics',
          'Willingness to work in challenging environments'
        ],
        'Benefits': [
          'Job security and pension benefits',
          'Health and life insurance',
          'Opportunities for career advancement'
        ],
        'Application Process': [
          'Physical fitness test',
          'Written examination',
          'Interview with the selection board',
          'Background check and medical examination'
        ]
      }
    },
    {
      id: 5,
      title: "Product Manager",
      company: "Google",
      location: "Mumbai",
      salary: "₹200,000 per month",
      type: "Full-time",
      experience: "5-8 years",
      skills: ["Product Management", "Agile", "Leadership"],
      description: "Google is looking for a Product Manager to lead our next big project...",
      postedDate: "2023-10-10",
      applyLink: "https://google.com/careers",
      jobInfo: {
        'Responsibilities': [
          'Define product vision and strategy',
          'Collaborate with engineering, design, and marketing teams',
          'Prioritize features and manage the product roadmap'
        ],
        'Requirements': [
          '5-8 years of experience in product management',
          'Strong analytical and problem-solving skills',
          'Excellent communication and leadership abilities'
        ],
        'Benefits': [
          'Competitive salary with stock options',
          'Comprehensive health and wellness benefits',
          'Flexible work hours and remote work options'
        ],
        'Application Process': [
          'Online application submission',
          'Initial phone screening',
          'Product case study presentation',
          'Final interview with senior leadership'
        ]
      }
    },
    {
      id: 6,
      title: "Graphic Designer",
      company: "Creative Studio",
      location: "Delhi",
      salary: "₹60,000 per month",
      type: "Full-time",
      experience: "1-3 years",
      skills: ["Adobe Photoshop", "Illustrator", "UI/UX Design"],
      description: "We are seeking a creative Graphic Designer to join our team...",
      postedDate: "2023-10-15",
      applyLink: "https://creativestudio.com/careers",
      jobInfo: {
        'Responsibilities': [
          'Create visual content for digital and print media',
          'Collaborate with the marketing team to design campaigns',
          'Develop UI/UX designs for web and mobile applications'
        ],
        'Requirements': [
          '1-3 years of experience in graphic design',
          'Proficient in Adobe Creative Suite (Photoshop, Illustrator, InDesign)',
          'Strong portfolio showcasing design skills'
        ],
        'Benefits': [
          'Creative and collaborative work environment',
          'Opportunities for skill development',
          'Performance-based bonuses'
        ],
        'Application Process': [
          'Submit your portfolio and resume',
          'Design assessment task',
          'Interview with the creative team',
          'Final decision and offer'
        ]
      }
    },
    {
      id: 7,
      title: "Marketing Manager",
      company: "Global Brands Inc.",
      location: "Chennai",
      salary: "₹90,000 per month",
      type: "Full-time",
      experience: "4-7 years",
      skills: ["Digital Marketing", "SEO", "Content Strategy"],
      description: "We are hiring a Marketing Manager to lead our digital marketing efforts...",
      postedDate: "2023-10-20",
      applyLink: "https://globalbrands.com/careers",
      jobInfo: {
        'Responsibilities': [
          'Develop and execute digital marketing strategies',
          'Manage SEO, SEM, and social media campaigns',
          'Analyze campaign performance and optimize ROI'
        ],
        'Requirements': [
          '4-7 years of experience in digital marketing',
          'Proficient in SEO tools and analytics platforms',
          'Strong understanding of content marketing and branding'
        ],
        'Benefits': [
          'Competitive salary with performance bonuses',
          'Opportunities for professional development',
          'Flexible work arrangements'
        ],
        'Application Process': [
          'Online application submission',
          'Initial interview with HR',
          'Marketing strategy presentation',
          'Final interview with the marketing director'
        ]
      }
    },
    {
      id: 8,
      title: "Customer Support Executive",
      company: "Tech Solutions",
      location: "Pune",
      salary: "₹35,000 per month",
      type: "Full-time",
      experience: "0-1 years",
      skills: ["Customer Service", "Communication", "Problem Solving"],
      description: "We are looking for a Customer Support Executive to assist our clients...",
      postedDate: "2023-10-25",
      applyLink: "https://techsolutions.com/careers",
      jobInfo: {
        'Responsibilities': [
          'Respond to customer inquiries via phone, email, and chat',
          'Resolve customer issues and escalate when necessary',
          'Maintain accurate records of customer interactions'
        ],
        'Requirements': [
          'Excellent communication and interpersonal skills',
          'Basic knowledge of customer support tools (e.g., Zendesk)',
          'Ability to work in a fast-paced environment'
        ],
        'Benefits': [
          'On-the-job training and support',
          'Performance-based incentives',
          'Opportunities for career growth'
        ],
        'Application Process': [
          'Online application submission',
          'Initial phone screening',
          'Interview with the support team',
          'Final decision and offer'
        ]
      }
    },
    {
  id: 9,
  title: "Software Engineer",
  company: "Google",
  location: "Bangalore",
  salary: "₹180,000 per month",
  type: "Full-time",
  experience: "3-7 years",
  skills: ["Java", "C++", "Distributed Systems"],
  description: "Google is seeking a Software Engineer to work on scalable and distributed systems...",
  postedDate: "2023-10-12",
  applyLink: "https://google.com/careers",
  jobInfo: {
    'Responsibilities': [
      'Design, develop, and maintain scalable software systems',
      'Collaborate with cross-functional teams to solve complex problems',
      'Optimize system performance and reliability'
    ],
    'Requirements': [
      '3-7 years of experience in software engineering',
      'Proficient in Java, C++, or similar programming languages',
      'Strong understanding of distributed systems and algorithms'
    ],
    'Benefits': [
      'Competitive salary with stock options',
      'Comprehensive health and wellness benefits',
      'Flexible work hours and remote work options'
    ],
    'Application Process': [
      'Online application submission',
      'Initial phone screening',
      'Technical coding interview',
      'Final interview with the engineering team'
    ]
  }
},
{
  id: 10,
  title: "UX Designer",
  company: "Google",
  location: "Hyderabad",
  salary: "₹150,000 per month",
  type: "Full-time",
  experience: "4-8 years",
  skills: ["User Research", "Wireframing", "Prototyping"],
  description: "Google is looking for a UX Designer to create intuitive and user-friendly interfaces...",
  postedDate: "2023-10-18",
  applyLink: "https://google.com/careers",
  jobInfo: {
    'Responsibilities': [
      'Conduct user research and usability testing',
      'Create wireframes, prototypes, and high-fidelity designs',
      'Collaborate with product managers and engineers to implement designs'
    ],
    'Requirements': [
      '4-8 years of experience in UX design',
      'Proficient in design tools like Sketch, Figma, or Adobe XD',
      'Strong portfolio showcasing UX design projects'
    ],
    'Benefits': [
      'Competitive salary with stock options',
      'Comprehensive health and wellness benefits',
      'Flexible work hours and remote work options'
    ],
    'Application Process': [
      'Online application submission',
      'Initial portfolio review',
      'Design challenge and presentation',
      'Final interview with the design team'
    ]
  }
},
{
  id: 11,
  title: "Data Engineer",
  company: "Google",
  location: "Gurgaon",
  salary: "₹170,000 per month",
  type: "Full-time",
  experience: "5-9 years",
  skills: ["Big Data", "SQL", "Data Pipelines"],
  description: "Google is hiring a Data Engineer to build and maintain data pipelines...",
  postedDate: "2023-10-22",
  applyLink: "https://google.com/careers",
  jobInfo: {
    'Responsibilities': [
      'Design and implement scalable data pipelines',
      'Optimize data storage and retrieval processes',
      'Collaborate with data scientists and analysts to support data needs'
    ],
    'Requirements': [
      '5-9 years of experience in data engineering',
      'Proficient in SQL, Python, and big data technologies (e.g., Hadoop, Spark)',
      'Strong understanding of data modeling and ETL processes'
    ],
    'Benefits': [
      'Competitive salary with stock options',
      'Comprehensive health and wellness benefits',
      'Flexible work hours and remote work options'
    ],
    'Application Process': [
      'Online application submission',
      'Initial phone screening',
      'Technical coding interview',
      'Final interview with the data engineering team'
    ]
  }
},
{
  id: 12,
  title: "Cloud Solutions Architect",
  company: "Google",
  location: "Pune",
  salary: "₹200,000 per month",
  type: "Full-time",
  experience: "6-10 years",
  skills: ["Cloud Computing", "Google Cloud Platform", "DevOps"],
  description: "Google is seeking a Cloud Solutions Architect to design and implement cloud solutions...",
  postedDate: "2023-10-28",
  applyLink: "https://google.com/careers",
  jobInfo: {
    'Responsibilities': [
      'Design and implement cloud-based solutions for clients',
      'Provide technical guidance on Google Cloud Platform (GCP)',
      'Collaborate with DevOps teams to ensure smooth deployment'
    ],
    'Requirements': [
      '6-10 years of experience in cloud computing',
      'Proficient in Google Cloud Platform (GCP) and related technologies',
      'Strong understanding of DevOps practices and tools'
    ],
    'Benefits': [
      'Competitive salary with stock options',
      'Comprehensive health and wellness benefits',
      'Flexible work hours and remote work options'
    ],
    'Application Process': [
      'Online application submission',
      'Initial phone screening',
      'Technical architecture interview',
      'Final interview with the cloud solutions team'
    ]
     }
    }
  ]
},

  qaDatabase: {
    'how do i book a service': {
      answer: 'To book a service, click on the category, select a service, choose a package, and fill out the booking form. You can also call us directly at +91 9669181789 for immediate assistance.',
      category: 'Booking',
      keywords: ['book', 'service', 'booking', 'schedule', 'appointment']
    },
    "come posso prenotare un servizio": {
  "answer": "Per prenotare un servizio, fai clic sulla categoria, seleziona un servizio, scegli un pacchetto e compila il modulo di prenotazione. Puoi anche chiamarci direttamente al +91 9669181789 per assistenza immediata.",
  "category": "Prenotazione",
  "keywords": ["prenotare", "servizio", "prenotazione", "programmare", "appuntamento"]
},
"मैं सेवा कैसे बुक करूं": {
  "answer": "सेवा बुक करने के लिए, श्रेणी पर क्लिक करें, एक सेवा चुनें, एक पैकेज चुनें और बुकिंग फॉर्म भरें। आप तत्काल सहायता के लिए हमें सीधे +91 9669181789 पर कॉल भी कर सकते हैं।",
  "category": "बुकिंग",
  "keywords": ["बुक", "सेवा", "बुकिंग", "शेड्यूल", "अपॉइंटमेंट"]
},
    'what services do you offer': {
      answer: 'We offer a wide range of home and occasion services including Plumbing, Electrical, Carpentry, Painting, Appliance Repair, AC/Heating Repair, Cleaning Services, and Skills Training.',
      category: 'Services',
      keywords: ['services', 'offer', 'available', 'provide', 'types']
    },
    'hey': {
      answer: 'Hello! How can I assist you today?',
      category: 'Greeting',
      keywords: ['hey', 'hi', 'hello', 'greetings']
    },
    'hello': {
      answer: 'Hi there! What can I help you with?',
      category: 'Greeting',
      keywords: ['hello', 'hi', 'hey']
    },
    'how are you': {
      answer: 'I am just a program, but I am here to help! How about you?',
      category: 'General',
      keywords: ['how', 'are', 'you', 'doing']
    },
    'good morning': {
      answer: 'Good morning! How can I assist you today?',
      category: 'Greeting',
      keywords: ['good', 'morning']
    },
    'good afternoon': {
      answer: 'Good afternoon! How may I help you?',
      category: 'Greeting',
      keywords: ['good', 'afternoon']
    },
    'good evening': {
      answer: 'Good evening! What can I do for you today?',
      category: 'Greeting',
      keywords: ['good', 'evening']
    },
    'what is your name': {
      answer: 'I am your assistant. How can I help you?',
      category: 'General',
      keywords: ['your', 'name']
    },
    'who are you': {
      answer: 'I am your assistant, here to assist you with your queries.',
      category: 'General',
      keywords: ['who', 'are', 'you']
    },
    'what can you do': {
      answer: 'I can help you book services, answer your queries, and assist with information about our offerings.',
      category: 'General',
      keywords: ['what', 'can', 'do']
    },
    'thank you': {
      answer: 'You’re welcome! Let me know if you need any further assistance.',
      category: "Polite",
      keywords: ['thank', 'you', 'thanks']
    },
    'bye': {
      answer: 'Goodbye! Have a great day!',
      category: 'Polite',
      keywords: ['bye', 'goodbye', 'see', 'later']
    },
    'do you offer emergency services': {
      answer: 'Yes, we do offer emergency services. Please call us at +91 9669181789 for immediate assistance.',
      category: 'Services',
      keywords: ['emergency', 'services', 'urgent', 'immediate']
    },
    'how do i cancel a booking': {
      answer: 'To cancel a booking, please log into your account, go to your bookings, and select the cancel option. You can also contact our support team.',
      category: 'Booking',
      keywords: ['cancel', 'booking', 'remove', 'appointment']
    },
    'do you have a mobile app': {
      answer: 'Yes, we have a mobile app available for download on Android and iOS platforms. Search for our app in your store.',
      category: 'General',
      keywords: ['mobile', 'app', 'application', 'download']
    },
    'how can i give feedback': {
      answer: 'You can provide feedback through our website or mobile app under the feedback section. We value your input!',
      category: 'Feedback',
      keywords: ['feedback', 'review', 'suggestion', 'input']
    },
    'do you offer discounts': {
      answer: 'Yes, we have seasonal and promotional discounts. Please check our website or subscribe to our newsletter for updates.',
      category: 'Services',
      keywords: ['discounts', 'offers', 'promotions', 'deals']
    },
    'where are you located': {
      answer: 'We are an online platform but operate across multiple cities. Contact us for location-specific services.',
      category: 'General',
      keywords: ['location', 'where', 'address', 'based']
    },
    'can i reschedule a booking': {
      answer: 'Yes, you can reschedule your booking through your account or by contacting our support team.',
      category: 'Booking',
      keywords: ['reschedule', 'change', 'booking', 'appointment']
    },
    'do you provide 24/7 support': {
      answer: 'Yes, our support team is available 24/7 to assist you.',
      category: 'Greeting',
      keywords: ['24/7', 'support', 'available', 'hours']
    },
    "who is narendra modi": {
      "answer": "Narendra Modi is the current Prime Minister of India, serving since May 2014. He is a leader of the Bharatiya Janata Party (BJP) and was previously the Chief Minister of Gujarat from 2001 to 2014.",
      "category": "Politics",
      "keywords": ["narendra", "modi", "prime minister", "india", "bjp"]
    },
    "who was the first prime minister of india": {
      "answer": "Jawaharlal Nehru was the first Prime Minister of India, serving from 1947 to 1964.",
      "category": "Politics",
      "keywords": ["first", "prime minister", "india", "jawaharlal", "nehru"]
    },
    "who is the president of the united states": {
      "answer": "As of 2025, the current President of the United States is [Check latest information], serving since [year].",
      "category": "Politics",
      "keywords": ["president", "usa", "united states", "america"]
    },
    "who was the first american prime minister": {
      "answer": "The United States does not have a Prime Minister. Instead, it has a President. The first President of the United States was George Washington, who served from 1789 to 1797.",
      "category": "Politics",
      "keywords": ["american", "prime minister", "first", "usa", "george washington"]
    },
    "who is the prime minister of the uk": {
      "answer": "As of 2025, the current Prime Minister of the United Kingdom is [Check latest information], serving since [year].",
      "category": "Politics",
      "keywords": ["prime minister", "uk", "united kingdom", "britain"]
    },
    // ... Add additional entries in a similar structure
  },

  mathKeywords: ['calculate', 'solve', 'what is', 'find', 'area', 'perimeter', 'volume', 'divide', 'multiply', '+', '-', '*', '/', '=', 'equation'],
  
  fallbackResponses: [
    "I apologize, but I couldn't find an exact match for your query. Could you please rephrase your question?",
    "I'm not sure about that specific question. Would you like to know about our available services or booking process instead?",
    "I couldn't find a direct answer to your question. Would you like to speak with a customer service representative?"
    ]
};
  
class EnhancedMathSolver {
  constructor() {
    this.basicOperators = {
      'plus': '+',
      'add': '+',
      'sum': '+',
      'minus': '-',
      'subtract': '-',
      'difference': '-',
      'times': '*',
      'multiply': '*',
      'product': '*',
      'divided by': '/',
      'divide': '/',
      'over': '/',
      'power': '^',
      'squared': '^2',
      'cubed': '^3'
    };

    // Word problem patterns
    this.wordProblemPatterns = [
      {
        type: 'total_cost_with_change',
        patterns: [
          /bought (\d+) packs of biscuits for ₹(\d+) each and (\d+) chocolate bars for ₹(\d+) each. If I pay with a ₹(\d+) note, how much change will I get?/i,
          /purchased (\d+) items for ₹(\d+) each and (\d+) items for ₹(\d+) each. If I pay with a ₹(\d+) note, how much change will I get?/i
        ],
        solve: (matches) => {
          const qty1 = parseInt(matches[1]);
          const price1 = parseFloat(matches[2]);
          const qty2 = parseInt(matches[3]);
          const price2 = parseFloat(matches[4]);
          const paid = parseFloat(matches[5]);

          const totalCost = (qty1 * price1) + (qty2 * price2);
          const change = paid - totalCost;

          return {
            result: change,
            explanation: `Items Purchased:\n- ${qty1} packs of biscuits at ₹${price1} each\n- ${qty2} chocolate bars at ₹${price2} each\nTotal Cost: ₹${totalCost.toFixed(2)}\nAmount Paid: ₹${paid.toFixed(2)}\nChange Due: ₹${change.toFixed(2)}`
          };
        }
      },
      {
        type: 'discount_with_tax',
        patterns: [
          /A jacket originally costs ₹(\d+) but is on a (\d+)% discount. After applying a (\d+)% tax on the discounted price, what is the final cost?/i,
          /An item costs ₹(\d+) with a (\d+)% discount and a (\d+)% tax applied. What is the final price?/i
        ],
        solve: (matches) => {
          const originalPrice = parseFloat(matches[1]);
          const discount = parseFloat(matches[2]);
          const tax = parseFloat(matches[3]);

          const discountedPrice = originalPrice * (1 - discount / 100);
          const finalPrice = discountedPrice * (1 + tax / 100);

          return {
            result: finalPrice,
            explanation: `Original Price: ₹${originalPrice.toFixed(2)}\nDiscount: ${discount}% (-₹${(originalPrice * discount / 100).toFixed(2)})\nDiscounted Price: ₹${discountedPrice.toFixed(2)}\nTax: ${tax}% (+₹${(discountedPrice * tax / 100).toFixed(2)})\nFinal Price: ₹${finalPrice.toFixed(2)}`
          };
        }
      }
    ];
  }

  detectMathQuery(query) {
    const normalizedQuery = query.toLowerCase();
    
    // Check for word problems first
    for (const problemType of this.wordProblemPatterns) {
      for (const pattern of problemType.patterns) {
        if (pattern.test(normalizedQuery)) {
          return true;
        }
      }
    }
    
    // Then check for regular math expressions
    const hasNumbers = /\d+(\.\d+)?/.test(normalizedQuery);
    const hasAdvancedPattern = /(?:solve|calculate|evaluate|find|what is|compute)/i.test(normalizedQuery);
    
    return hasNumbers && hasAdvancedPattern;
  }

  solveMathProblem(query) {
    const normalizedQuery = query.toLowerCase();
    
    // Try to solve as a word problem first
    for (const problemType of this.wordProblemPatterns) {
      for (const pattern of problemType.patterns) {
        const matches = normalizedQuery.match(pattern);
        if (matches) {
          const solution = problemType.solve(matches);
          return this.formatWordProblemResponse(problemType.type, solution);
        }
      }
    }
    
    // If not a word problem, use existing math solving logic
    try {
      const parsedExpression = this.parseExpression(normalizedQuery);
      const result = eval(parsedExpression.replace(/\^/g, '**'));
      return this.formatArithmeticResponse(parsedExpression, result);
    } catch (e) {
      return this.handleError(query);
    }
  }

  formatWordProblemResponse(type, solution) {
    return `📝 Problem Solution:\n\n${solution.explanation}`;
  }

  formatArithmeticResponse(expression, result) {
    return `🔢 Result: ${result}`;
  }

  handleError(query) {
    return `❌ Unable to process the mathematical expression: "${query}"\nPlease check the format and try again.`;
  }

  parseExpression(query) {
    let normalizedQuery = query.toLowerCase()
      .replace(/what is|calculate|solve|find|evaluate|compute/g, '')
      .trim();

    Object.entries(this.basicOperators).forEach(([word, symbol]) => {
      normalizedQuery = normalizedQuery.replace(new RegExp(word, 'g'), symbol);
    });

    return normalizedQuery;
  }
}

class ChatSystem {
  constructor() {
    this.messageHistory = [];
    this.typingTimeout = null;
    this.similarityThreshold = 0.5;
    this.mathSolver = new EnhancedMathSolver();
    this.hasUserChatted = false; // Track if the user has started chatting
  }

  initialize() {
    this.chatInput = document.getElementById('chatInput');
    this.chatMessages = document.getElementById('chatMessages');
    this.sendButton = document.getElementById('sendButton');
    this.setupEventListeners();
    this.displayWelcomeMessage();
  }

  setupEventListeners() {
    this.sendButton.addEventListener('click', () => this.handleUserInput());
    this.chatInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        e.preventDefault();
        this.handleUserInput();
      }
    });
    this.chatInput.addEventListener('input', () => this.handleTypingIndicator());
  }

  handleTypingIndicator() {
    clearTimeout(this.typingTimeout);
    const typingIndicator = document.getElementById('typingIndicator');
    
    if (!typingIndicator) {
      const indicator = document.createElement('div');
      indicator.id = 'typingIndicator';
      indicator.className = 'message bot-message typing-indicator';
      indicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
      this.chatMessages.appendChild(indicator);
    }

    this.typingTimeout = setTimeout(() => {
      const indicator = document.getElementById('typingIndicator');
      if (indicator) indicator.remove();
    }, 1000);
  }

  displayWelcomeMessage() {
    const welcomeMessage = `
      <div id="welcomeMessage" class="message bot-message welcome-message">
        <div class="welcome-avatar"><i class="fas fa-message"></i></div>
        <div class="welcome-content">
          <p>👋 Hi! I'm your Assistant</p>
        </div>
      </div>`;
    this.chatMessages.innerHTML = welcomeMessage;
}

  async handleUserInput() {
    const query = this.chatInput.value.trim();
    if (!query) return;

    // Remove the welcome message if it's the first user message
    if (!this.hasUserChatted) {
      this.hasUserChatted = true;
      const welcomeMessage = document.getElementById('welcomeMessage');
      if (welcomeMessage) {
        welcomeMessage.classList.add('fade-out');
        setTimeout(() => welcomeMessage.remove(), 300); // Wait for fade-out animation
      }
    }

    this.displayMessage(query, 'user');
    this.chatInput.value = '';

    await this.simulateProcessing();
    const response = this.processQuery(query);
    this.displayMessage(response, 'bot');
    
    this.messageHistory.push({ query, response, timestamp: new Date() });
    this.scrollToBottom();
    
    this.setupClickButtons();
    this.setupTouchButtons();
  }

  setupClickButtons() {
      document.querySelectorAll('.click-button').forEach(button => {
          button.addEventListener('click', () => {
              const url = button.getAttribute('data-url');
              if (url) window.location.href = url;
          });
      });
  }

  setupTouchButtons() {
      document.querySelectorAll('.touch-button').forEach(button => {
          button.addEventListener('click', () => {
              const url = button.getAttribute('data-url');
              if (url) window.location.href = url;
          });
      });
  }

  processQuery(query) {
  const normalizedQuery = query.toLowerCase();

          // Check for loan-related keywords
          const loanKeywords = ["loan", "personal loan", "home loan", "business loan", "emi", "interest rate"];
        const isLoanQuery = loanKeywords.some(keyword => normalizedQuery.includes(keyword));

        if (isLoanQuery) {
            return this.processLoanQuery(normalizedQuery);
        }

      // Try course query processing first
      const courseResponse = this.processCourseQuery(normalizedQuery);
    if (courseResponse) return courseResponse;

      // Try franchise query processing first
      const franchiseResponse = this.processFranchiseQuery(normalizedQuery);
    if (franchiseResponse) return franchiseResponse;

      // Try movie query processing first
      const movieResponse = this.processMovieQuery(normalizedQuery);
    if (movieResponse) return movieResponse;

    // Try flight query processing
    const flightResponse = this.processFlightQuery(normalizedQuery);
  if (flightResponse) return flightResponse;

    const busResponse = this.processBusQuery(normalizedQuery);
    if (busResponse) return busResponse;

    // Try train query processing
    const trainResponse = this.processTrainQuery(normalizedQuery);
  if (trainResponse) return trainResponse;

    // Try live train status query processing
    const liveTrainResponse = this.processLiveTrainStatus(normalizedQuery);
  if (liveTrainResponse) return liveTrainResponse;

      // Try provider query processing first
      const providerResponse = this.processProviderQuery(normalizedQuery);
    if (providerResponse) return providerResponse;

      // Try disease query processing first
  const diseaseResponse = this.processDiseaseQuery(normalizedQuery);
  if (diseaseResponse) return diseaseResponse;

      // Try automotive query processing first
      const automotiveResponse = this.processAutomotiveQuery(normalizedQuery);
    if (automotiveResponse) return automotiveResponse;

  // Other query processing (unchanged)
  const domainResponse = this.processDomainRegistrationQuery(normalizedQuery);
  if (domainResponse) return domainResponse;

  // Check for stock query first
  const stockResponse = this.processStockQuery(normalizedQuery);
  if (stockResponse) return stockResponse;

  const realEstateResponse = this.processRealEstateQuery(normalizedQuery);
  if (realEstateResponse) return realEstateResponse;

  const jobResponse = this.processJobQuery(normalizedQuery);
  if (jobResponse) return jobResponse;

  const promptResponse = this.processPromptQuery(normalizedQuery);
  if (promptResponse) return promptResponse;

  if (this.mathSolver.detectMathQuery(normalizedQuery)) {
    return this.mathSolver.solveMathProblem(normalizedQuery);
  }

  const isCouponQuery = this.isCouponRelatedQuery(normalizedQuery);
  if (isCouponQuery) {
    const couponResponse = this.processCouponQuery(normalizedQuery);
    if (couponResponse) return couponResponse;
  }

  const qaResponse = this.searchDatabase(normalizedQuery);
  if (qaResponse) return qaResponse;

  return this.getRandomFallbackResponse();
}

processLoanQuery(query) {
        const normalizedQuery = query.toLowerCase();

        // Extract loan details from the query
        const loanDetails = this.extractLoanDetails(normalizedQuery);

        if (!loanDetails) {
            return "Sorry, I couldn't understand your loan request. Please provide more details.";
        }

        // Get relevant loan providers
        const relevantProviders = this.getRelevantLoanProviders(loanDetails);

        if (relevantProviders.length > 0) {
            return this.generateLoanResponse(relevantProviders, loanDetails);
        } else {
            return "Sorry, I couldn't find any loan providers matching your request. Please try a different query.";
        }
    }

    extractLoanDetails(query) {
        const loanTypes = ["personal", "home", "business"];
        const loanTypeMatch = loanTypes.find(type => query.includes(type));

        const amountMatch = query.match(/₹?\s*(\d+,\d+|\d+)\s*(?:lakh|lac|thousand|k|rs|rupees)?/i);
        const amount = amountMatch ? parseFloat(amountMatch[1].replace(/,/g, '')) : null;

        const interestRateMatch = query.match(/(\d+(\.\d+)?)\s*%?\s*interest rate/i);
        const interestRate = interestRateMatch ? parseFloat(interestRateMatch[1]) : null;

        const tenureMatch = query.match(/(\d+)\s*(?:years|yrs|year|yr)/i);
        const tenure = tenureMatch ? parseInt(tenureMatch[1]) : null;

        return {
            type: loanTypeMatch,
            amount: amount,
            interestRate: interestRate,
            tenure: tenure
        };
    }

    getRelevantLoanProviders(loanDetails) {
        let providers = searchConfig.loanProvidersDatabase.providers;

        // Filter by loan type
        if (loanDetails.type) {
            providers = providers.filter(provider => 
                provider.loanTypes.map(t => t.toLowerCase()).includes(loanDetails.type)
            );
        }

        // Filter by loan amount
        if (loanDetails.amount) {
            providers = providers.filter(provider => 
                loanDetails.amount >= provider.loanAmount.min && 
                loanDetails.amount <= provider.loanAmount.max
            );
        }

        // Filter by interest rate
        if (loanDetails.interestRate) {
            providers = providers.filter(provider => 
                loanDetails.interestRate >= provider.interestRate.min && 
                loanDetails.interestRate <= provider.interestRate.max
            );
        }

        // Filter by tenure
        if (loanDetails.tenure) {
            providers = providers.filter(provider => 
                loanDetails.tenure >= provider.tenure.min && 
                loanDetails.tenure <= provider.tenure.max
            );
        }

        return providers.slice(0, 5); // Return top 5 results
    }

    generateLoanResponse(providers, loanDetails) {
        const providerList = providers.map(provider => this.formatLoanProvider(provider)).join('');

        return `
            <div class="loan-container">
                <h3>🏦 Loan Options</h3>
                <div class="loan-providers-list">
                    ${providerList}
                </div>
            </div>
        `;
    }

    formatLoanProvider(provider) {
        const metadata = provider.metadata ? `
            <div class="loan-info hidden">
                ${Object.entries(provider.metadata).map(([section, items]) => `
                    <div class="info-section">
                        <h4>${section}</h4>
                        <ul>
                            ${items.map(item => `<li>${item}</li>`).join('')}
                        </ul>
                    </div>
                `).join('')}
            </div>
        ` : '';

        return `
            <div class="loan-item" onclick="toggleLoanInfo(this)">
                <div class="loan-header">
                    <div class="loan-name">${provider.name}</div>
                    <div class="loan-type">🏷️ ${provider.loanTypes.join(', ')}</div>
                </div>
                <div class="loan-details">
                    <div class="loan-amount">💰 ₹${provider.loanAmount.min.toLocaleString()} - ₹${provider.loanAmount.max.toLocaleString()}</div>
                    <div class="interest-rate">📈 ${provider.interestRate.min}% - ${provider.interestRate.max}%</div>
                    <div class="tenure">📅 ${provider.tenure.min} - ${provider.tenure.max} years</div>
                </div>
                <div class="loan-description">${provider.emiOptions}</div>
                <div class="action-buttons">
                    <a href="${provider.applyNowLink}" class="apply-now-button" target="_blank">
                        <i class="fas fa-file-signature"></i> Apply Now
                    </a>
                </div>
                ${metadata}
            </div>
        `;
    }

processCourseQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Extract course name or category from the query
    const courseMatch = normalizedQuery.match(/(web development|data science|python|full stack|course)/i);
    if (!courseMatch) return null;

    const courseNameOrCategory = courseMatch[0].toLowerCase();

    // Find courses in the database
    const courses = searchConfig.coursesDatabase.courses.filter(course => 
      course.name.toLowerCase().includes(courseNameOrCategory) || 
      course.category.toLowerCase().includes(courseNameOrCategory)
    );

    if (courses.length > 0) {
      return this.generateCourseResponse(courses);
    }

    return null;
  }

  generateCourseResponse(courses) {
    const courseList = courses.map(course => this.formatCourse(course)).join('');

    return `
      <div class="courses-container">
        <h3>📚 Courses</h3>
        <div class="courses-list">
          ${courseList}
        </div>
      </div>
    `;
  }

  formatCourse(course) {
    const metadata = course.metadata ? `
      <div class="course-info hidden">
        ${Object.entries(course.metadata).map(([section, items]) => `
          <div class="info-section">
            <h4>${section}</h4>
            <ul>
              ${items.map(item => `<li>${item}</li>`).join('')}
            </ul>
          </div>
        `).join('')}
      </div>
    ` : '';

    const actionButtons = course.actionButtons.map(btn => `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `).join('');

    return `
      <div class="course-item" onclick="toggleCourseInfo(this)">
        <div class="course-header">
          <div class="course-name">${course.name}</div>
          <div class="course-category">🏷️ ${course.category}</div>
        </div>
        <div class="course-details">
          <div class="course-duration">⏳ ${course.duration}</div>
          <div class="course-fees">💰 ${course.fees}</div>
        </div>
        <div class="course-description">${course.description}</div>
        <div class="action-buttons">
          ${actionButtons}
        </div>
        ${metadata}
      </div>
    `;
  }

processFranchiseQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Extract franchise name or category from the query
    const franchiseMatch = normalizedQuery.match(/(mcdonald's|domino's|pizza|fast food|franchise)/i);
    if (!franchiseMatch) return null;

    const franchiseNameOrCategory = franchiseMatch[0].toLowerCase();

    // Find franchises in the database
    const franchises = searchConfig.franchiseDatabase.franchises.filter(franchise => 
      franchise.name.toLowerCase().includes(franchiseNameOrCategory) || 
      franchise.category.toLowerCase().includes(franchiseNameOrCategory)
    );

    if (franchises.length > 0) {
      return this.generateFranchiseResponse(franchises);
    }

    return null;
  }

  generateFranchiseResponse(franchises) {
    const franchiseList = franchises.map(franchise => this.formatFranchise(franchise)).join('');

    return `
      <div class="franchise-container">
        <h3>🏢 Franchise Opportunities</h3>
        <div class="franchises-list">
          ${franchiseList}
        </div>
      </div>
    `;
  }

  formatFranchise(franchise) {
    const metadata = franchise.metadata ? `
      <div class="franchise-info hidden">
        ${Object.entries(franchise.metadata).map(([section, items]) => `
          <div class="info-section">
            <h4>${section}</h4>
            <ul>
              ${items.map(item => `<li>${item}</li>`).join('')}
            </ul>
          </div>
        `).join('')}
      </div>
    ` : '';

    const actionButtons = franchise.actionButtons.map(btn => `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `).join('');

    return `
      <div class="franchise-item" onclick="toggleFranchiseInfo(this)">
        <div class="franchise-header">
          <div class="franchise-name">${franchise.name}</div>
          <div class="franchise-category">🏷️ ${franchise.category}</div>
        </div>
        <div class="franchise-details">
          <div class="investment-range">💰 ${franchise.investmentRange}</div>
          <div class="franchise-fee">💵 Initial Fee: ${franchise.initialFranchiseFee}</div>
          <div class="royalty-fee">📊 Royalty Fee: ${franchise.royaltyFee}</div>
        </div>
        <div class="franchise-description">${franchise.description}</div>
        <div class="action-buttons">
          ${actionButtons}
        </div>
        ${metadata}
      </div>
    `;
  }

processMovieQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Extract movie name or genre from the query
    const movieMatch = normalizedQuery.match(/(machante malakha|the brutalist|drama|romance|action)/i);
    if (!movieMatch) return null;

    const movieNameOrGenre = movieMatch[0].toLowerCase();

    // Find movies in the database
    const movies = searchConfig.movieDatabase.movies.filter(movie => 
      movie.title.toLowerCase().includes(movieNameOrGenre) || 
      movie.genre.map(g => g.toLowerCase()).includes(movieNameOrGenre)
    );

    if (movies.length > 0) {
      return this.generateMovieResponse(movies);
    }

    return null;
  }

  generateMovieResponse(movies) {
    const movieList = movies.map(movie => this.formatMovie(movie)).join('');

    return `
      <div class="movie-container">
        <h3>🎬 Movies</h3>
        <div class="movies-list">
          ${movieList}
        </div>
      </div>
    `;
  }

  formatMovie(movie) {
    const metadata = movie.metadata ? `
      <div class="movie-info hidden">
        ${Object.entries(movie.metadata).map(([section, items]) => `
          <div class="info-section">
            <h4>${section}</h4>
            <ul>
              ${items.map(item => `<li>${item}</li>`).join('')}
            </ul>
          </div>
        `).join('')}
      </div>
    ` : '';

    const actionButtons = movie.actionButtons.map(btn => `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `).join('');

    return `
      <div class="movie-item" onclick="toggleMovieInfo(this)">
        <div class="movie-header">
          <div class="movie-title">${movie.title}</div>
          <div class="movie-release-date">📅 ${movie.releaseDate}</div>
        </div>
        <div class="movie-details">
          <div class="movie-genre">🎭 ${movie.genre.join(', ')}</div>
          <div class="movie-cast">🎬 ${movie.cast.join(', ')}</div>
        </div>
        <div class="movie-description">${movie.description}</div>
        <div class="action-buttons">
          ${actionButtons}
        </div>
        ${metadata}
      </div>
    `;
  }

  processFlightQuery(query) {
  const normalizedQuery = query.toLowerCase();

  // Airport data mapping city names to airport codes
  const airportData = {
    'mumbai': 'BOM',
    'delhi': 'DEL',
    'bangalore': 'BLR',
    'bengaluru': 'BLR',
    'hyderabad': 'HYD',
    'chennai': 'MAA',
    'kolkata': 'CCU',
    'pune': 'PNQ',
    'ahmedabad': 'AMD',
    'indore': 'IDR',
    'bhopal': 'BHO'
  };

  // Enhanced flight pattern to capture more details
  const flightPattern = /(?:book|search|find|check)?\s?(?:a )?flight\s?(?:from )?([\w\s]+)?\s?(?:to )?([\w\s]+)?\s?(?:on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?\s?(?:and return on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?/i;
  const match = normalizedQuery.match(flightPattern);

  if (match) {
    let fromCity = null;
    let toCity = null;

    // Extract cities from the query
    const cities = normalizedQuery.match(/\b(mumbai|delhi|bangalore|bengaluru|hyderabad|chennai|kolkata|pune|ahmedabad|indore|bhopal)\b/g);

    if (cities && cities.length >= 2) {
      // If the query explicitly mentions "from" and "to", use that order
      if (normalizedQuery.includes('from') && normalizedQuery.includes('to')) {
        const fromIndex = normalizedQuery.indexOf('from');
        const toIndex = normalizedQuery.indexOf('to');
        fromCity = normalizedQuery.slice(fromIndex + 5, toIndex).trim();
        toCity = normalizedQuery.slice(toIndex + 3).trim();
      } else {
        // Otherwise, assume the first city is "from" and the second is "to"
        fromCity = cities[0];
        toCity = cities[1];
      }
    } else if (cities && cities.length === 1) {
      // If only one city is mentioned, assume it's the "to" city
      toCity = cities[0];
    }

    const departureDate = match[3] ? this.parseDate(match[3].trim()) : null;
    const returnDate = match[4] ? this.parseDate(match[4].trim()) : null;

    // Extract class type from the query
    const classMatch = normalizedQuery.match(/\b(economy|premium economy|business|first class)\b/i);
    const classType = classMatch ? classMatch[0].charAt(0).toLowerCase() : 'e';

    // Extract number of travelers from the query
    const travellersMatch = normalizedQuery.match(/\b(\d+)\s?(?:passengers?|travelers?|people|adults?)\b/i);
    const travellers = travellersMatch ? parseInt(travellersMatch[1], 10) : 1;

    // Determine trip type
    const tripType = returnDate ? 'round-trip' : 'one-way';

    return this.createFlightSearchResponse(fromCity, toCity, departureDate, returnDate, classType, travellers, tripType);
  }

  // If the query is just a generic flight search
  if (/\b(?:search|find|book)\s?flight\b/i.test(normalizedQuery)) {
    return this.createFlightSearchResponse();
  }

  return null;
}

createFlightSearchResponse(fromCity = null, toCity = null, departureDate = null, returnDate = null, classType = 'e', travellers = 1, tripType = 'one-way') {
  const isRoundTrip = tripType === 'round-trip';

  const airportData = {
    'mumbai': 'BOM',
    'delhi': 'DEL',
    'bangalore': 'BLR',
    'bengaluru': 'BLR',
    'hyderabad': 'HYD',
    'chennai': 'MAA',
    'kolkata': 'CCU',
    'pune': 'PNQ',
    'ahmedabad': 'AMD',
    'indore': 'IDR',
    'bhopal': 'BHO'
  };

  const createCustomSelect = (id, label, selectedCity) => {
  const cities = Object.keys(airportData);
  return `
    <div class="custom-select-container">
      <input type="text" 
             id="${id}-input" 
             class="city-search-input" 
             placeholder="${label}"
             autocomplete="off"
             ${selectedCity ? `value="${selectedCity}"` : ''}>
      <input type="hidden" 
             id="${id}" 
             name="${id}" 
             ${selectedCity ? `value="${airportData[selectedCity]}"` : ''}>
      <div id="${id}-dropdown" class="city-dropdown">
        <div class="dropdown-header">
          <span>Select City</span>
          <button class="close-dropdown-btn">×</button>
        </div>
        <div class="city-options">
          ${cities.map(city => `
            <div class="city-option" 
                 data-city="${city}"
                 data-code="${airportData[city]}"
                 ${selectedCity === city ? 'data-selected="true"' : ''}>
              <div class="city-code">${airportData[city]}</div>
              <div class="city-name">${city.charAt(0).toUpperCase() + city.slice(1)}</div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;
};

const flightSearchHtml = `
    <div class="flight-search-response">
      <div class="trip-type-toggle">
        <button class="trip-type-btn ${tripType === 'one-way' ? 'active' : ''}" data-type="one-way">One-Way</button>
        <button class="trip-type-btn ${tripType === 'round-trip' ? 'active' : ''}" data-type="round-trip">Round-Trip</button>
      </div>

      <div class="city-selection">
        ${createCustomSelect('from', 'From', fromCity)}
        <button class="swap-cities-btn">
          <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M8 3L4 7L8 11" />
            <path d="M4 7H20" />
            <path d="M16 21L20 17L16 13" />
            <path d="M20 17H4" />
          </svg>
        </button>
        ${createCustomSelect('to', 'To', toCity)}
      </div>

      <div class="date-selection">
        <div class="date-field">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
            <line x1="16" y1="2" x2="16" y2="6" />
            <line x1="8" y1="2" x2="8" y2="6" />
            <line x1="3" y1="10" x2="21" y2="10" />
          </svg>
          <input type="date" id="departureDate" name="departureDate" value="${departureDate || ''}" required>
        </div>
        <div class="date-field return-date ${isRoundTrip ? 'visible' : ''}" id="returnDateGroup">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
            <line x1="16" y1="2" x2="16" y2="6" />
            <line x1="8" y1="2" x2="8" y2="6" />
            <line x1="3" y1="10" x2="21" y2="10" />
          </svg>
          <input type="date" id="returnDate" name="returnDate" value="${returnDate || ''}">
        </div>
      </div>

      <div class="traveller-class">
        <div class="traveller-btn">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2" />
            <circle cx="9" cy="7" r="4" />
            <path d="M23 21v-2a4 4 0 0 0-3-3.87" />
            <path d="M16 3.13a4 4 0 0 1 0 7.75" />
          </svg>
          <span>${travellers} Traveller${travellers > 1 ? 's' : ''} • ${classType === 'e' ? 'Economy' : classType === 'p' ? 'Premium Economy' : 'Business'}</span>
        </div>
        <div class="traveller-dropdown">
          <div class="traveller-count">
            <span>Travellers</span>
            <div class="counter">
              <button class="counter-btn minus">-</button>
              <input type="number" id="travellers" name="travellers" value="${travellers}" min="1" max="9">
              <button class="counter-btn plus">+</button>
            </div>
          </div>
          <div class="class-selection">
            <span>Class</span>
            <select id="class" name="class">
              <option value="e" ${classType === 'e' ? 'selected' : ''}>Economy</option>
              <option value="p" ${classType === 'p' ? 'selected' : ''}>Premium Economy</option>
              <option value="b" ${classType === 'b' ? 'selected' : ''}>Business</option>
            </select>
          </div>
        </div>
      </div>

      <button type="submit" class="search-btn">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="11" cy="11" r="8" />
          <line x1="21" y1="21" x2="16.65" y2="16.65" />
        </svg>
        Search Flight
      </button>
    </div>

    <style>
      .flight-search-response {
        background: #FF69B4;
        border-radius: 24px;
        padding: 24px;
        max-width: 600px;
        margin: 0 auto;
      }

      .trip-type-toggle {
        background: #000;
        border-radius: 100px;
        padding: 4px;
        display: flex;
        margin-bottom: 24px;
      }

      .trip-type-btn {
        flex: 1;
        padding: 12px;
        border: none;
        border-radius: 100px;
        background: transparent;
        color: white;
        cursor: pointer;
        font-weight: 600;
        transition: all 0.3s ease;
      }

      .trip-type-btn.active {
        background: white;
        color: black;
      }

      .city-selection {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 24px;
      }

      .swap-cities-btn {
        background: rgba(0, 0, 0, 0.1);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .swap-cities-btn:hover {
        background: rgba(0, 0, 0, 0.2);
      }

      .custom-select-container {
        flex: 1;
        position: relative;
      }

      .city-search-input {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: rgba(0, 0, 0, 0.1);
        color: white;
        font-size: 16px;
      }

      .city-search-input::placeholder {
        color: rgba(255, 255, 255, 0.7);
      }

/* Dropdown Container */
.city-dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  background: white;
  border-radius: 12px;
  margin-top: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  display: none;
  z-index: 1000;
  max-height: 300px;
  overflow-y: auto;
}

/* Dropdown Header */
.dropdown-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  color: #000;
  background: #fff;
  border-radius: 12px 12px 0 0;
  position: sticky;
  top: 0;
  z-index: 1;
}

/* Close Button */
.close-dropdown-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #666;
  padding: 0;
  line-height: 1;
}

.close-dropdown-btn:hover {
  color: #000;
}

/* City Options */
.city-options {
  padding: 8px 0;
}

.city-option {
  padding: 12px 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 12px;
}

.city-option:hover {
  background: rgba(0, 0, 0, 0.05);
}

.city-code {
  font-weight: 600;
  color: #000;
}

.city-name {
  color: #666;
}

      .date-selection {
        display: flex;
        gap: 12px;
        margin-bottom: 24px;
      }

      .date-field {
        flex: 1;
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .date-field input {
        background: transparent;
        border: none;
        color: white;
        font-size: 16px;
        width: 100%;
      }

      .return-date {
        display: none;
      }

      .return-date.visible {
        display: flex;
      }

      .traveller-class {
        margin-bottom: 24px;
        position: relative;
      }

      .traveller-btn {
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
        cursor: pointer;
        color: white;
      }

      .traveller-dropdown {
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background: white;
        border-radius: 12px;
        margin-top: 8px;
        padding: 16px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        display: none;
      }

      .traveller-count,
      .class-selection {
        margin-bottom: 16px;
      }

      .counter {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-top: 8px;
      }

      .counter-btn {
        width: 32px;
        height: 32px;
        border-radius: 50%;
        border: 1px solid #ddd;
        background: white;
        cursor: pointer;
      }

      .counter input {
        width: 40px;
        text-align: center;
        border: none;
        font-size: 16px;
      }

      .class-selection select {
        width: 100%;
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 8px;
        margin-top: 8px;
        font-size: 16px;
      }

      .search-btn {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: white;
        color: #FF69B4;
        font-size: 16px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .search-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      }

@media (max-width: 480px) {
  .flight-search-response {
    width: 83vw;
    padding: 16px;
    border-radius: 16px;
  }

  /* Improved city selection layout */
  .city-selection {
    flex-direction: row;
    gap: 12px;
    color: #FFF;
    align-items: center;
    flex-wrap: nowrap;
    margin-bottom: 16px;
  }

  .custom-select-container {
    flex: 1;
    min-width: 0;
    position: relative;
  }

  /* Enhanced city search input */
  .city-search-input {
    width: 100%;
    padding: 14px;
    font-size: 15px;
    height: 52px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    border-radius: 12px;
    background: #000;
    transition: all 0.2s ease;
  }

  .city-search-input:focus {
    border-color: #FFF;
    outline: none;
  }

.city-dropdown {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  top: auto;
  max-height: 75vh;
  overflow-y: auto;
  margin: 0;
  border-radius: 20px 20px 0 0;
  box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.12);
  padding: 24px 0 16px;
  z-index: 1001;
  background: #FFFFFF;
  transform: translateY(0);
  transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.city-dropdown.hidden {
  transform: translateY(100%);
}

.dropdown-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 16px 16px;
  color: #000;
  border-bottom: 1px solid rgba(0, 0, 0, 0.1);
}

.close-dropdown-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: #666;
}

.city-options {
  max-height: 60vh;
  overflow-y: auto;
}

  /* Improved typography and layout */
  .city-code {
    font-size: 18px;
    font-weight: 600;
    min-width: 48px;
    color: #1A1A1A;
  }

  .city-name {
    font-size: 15px;
    font-weight: 400;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    color: #333333;
  }

  /* Professional overlay background */
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(4px);
    z-index: 1000;
    opacity: 1;
    transition: opacity 0.3s ease;
  }

  .modal-overlay.hidden {
    opacity: 0;
    pointer-events: none;
  }

  /* Improved swap button */
  .swap-cities-btn {
    width: 36px;
    height: 36px;
    flex-shrink: 0;
    border-radius: 50%;
    background: #FFFFFF;
    border: 1px solid rgba(0, 0, 0, 0.12);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }

  .swap-cities-btn:active {
    background: rgba(0, 0, 0, 0.05);
    transform: scale(0.95);
  }

  /* Scrollbar styling */
  .city-dropdown::-webkit-scrollbar {
    width: 8px;
  }

  .city-dropdown::-webkit-scrollbar-track {
    background: transparent;
  }

  .city-dropdown::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 4px;
  }
}
            </style>
  `;

  // Add event listeners
  setTimeout(() => {
    const setupFlightCitySearch = (inputId) => {
  const input = document.getElementById(`${inputId}-input`);
  const hiddenInput = document.getElementById(inputId);
  const dropdown = document.getElementById(`${inputId}-dropdown`);
  const closeBtn = dropdown.querySelector('.close-dropdown-btn');
  const options = dropdown.querySelectorAll('.city-option');

  let isDropdownOpen = false;

  const openDropdown = () => {
    dropdown.style.display = 'block';
    isDropdownOpen = true;
  };

  const closeDropdown = () => {
    dropdown.style.display = 'none';
    isDropdownOpen = false;
  };

  // Open dropdown on input click (more reliable on mobile)
  input.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    if (!isDropdownOpen) {
      openDropdown();
    }
  });

  // Close dropdown when clicking outside
  document.addEventListener('click', (e) => {
    if (!input.contains(e.target) && !dropdown.contains(e.target)) {
      closeDropdown();
    }
  });

  // Close dropdown when clicking the close button
  closeBtn.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    closeDropdown();
  });

  // Handle city selection
  options.forEach(option => {
    option.addEventListener('click', (e) => {
      e.stopPropagation(); // Prevent event from bubbling up
      const cityName = option.dataset.city;
      const cityCode = option.dataset.code;
      input.value = `${cityCode} - ${cityName.charAt(0).toUpperCase() + cityName.slice(1)}`;
      hiddenInput.value = cityCode;
      closeDropdown();
    });
  });

  // Filter options based on input
  input.addEventListener('input', (e) => {
    const searchText = e.target.value.toLowerCase();
    options.forEach(option => {
      const cityName = option.dataset.city;
      const cityCode = option.dataset.code;
      const matches = cityName.toLowerCase().includes(searchText) || 
                     cityCode.toLowerCase().includes(searchText);
      option.style.display = matches ? 'flex' : 'none';
    });
  });
};

  // Flight dropdown setup
  setupFlightCitySearch('from');
  setupFlightCitySearch('to');

// Trip type toggle
const tripTypeBtns = document.querySelectorAll('.trip-type-btn');
const returnDateGroup = document.getElementById('returnDateGroup');

tripTypeBtns.forEach(btn => {
  btn.addEventListener('click', () => {
    tripTypeBtns.forEach(b => b.classList.remove('active'));
    btn.classList.add('active');
    const type = btn.dataset.type;
    returnDateGroup.style.display = type === 'round-trip' ? 'flex' : 'none';
    document.getElementById('tripType').value = type;
  });
});

// City swap functionality
const swapBtn = document.querySelector('.swap-cities-btn');
swapBtn.addEventListener('click', () => {
  const fromInput = document.getElementById('from-input');
  const toInput = document.getElementById('to-input');
  const fromHidden = document.getElementById('from');
  const toHidden = document.getElementById('to');

  const tempValue = fromInput.value;
  const tempHidden = fromHidden.value;
  
  fromInput.value = toInput.value;
  fromHidden.value = toHidden.value;
  toInput.value = tempValue;
  toHidden.value = tempHidden;
});

// Traveller and class selection
const travellerBtn = document.querySelector('.traveller-btn');
const travellerDropdown = document.querySelector('.traveller-dropdown');
const counterBtns = document.querySelectorAll('.counter-btn');
const travellersInput = document.getElementById('travellers');
const classSelect = document.getElementById('class');

travellerBtn.addEventListener('click', () => {
  travellerDropdown.style.display = travellerDropdown.style.display === 'block' ? 'none' : 'block';
});

document.addEventListener('click', (e) => {
  if (!travellerBtn.contains(e.target) && !travellerDropdown.contains(e.target)) {
    travellerDropdown.style.display = 'none';
  }
});

counterBtns.forEach(btn => {
  btn.addEventListener('click', () => {
    const isPlus = btn.classList.contains('plus');
    const currentValue = parseInt(travellersInput.value);
    
    if (isPlus && currentValue < 9) {
      travellersInput.value = currentValue + 1;
    } else if (!isPlus && currentValue > 1) {
      travellersInput.value = currentValue - 1;
    }
    
    updateTravellerDisplay();
  });
});

classSelect.addEventListener('change', updateTravellerDisplay);

function updateTravellerDisplay() {
  const travellers = travellersInput.value;
  const classType = classSelect.options[classSelect.selectedIndex].text;
  const displayText = `${travellers} Traveller${travellers > 1 ? 's' : ''} • ${classType}`;
  document.querySelector('.traveller-btn span').textContent = displayText;
}

  // Form submission
    // Form submission
    document.querySelector('.search-btn').addEventListener('click', (e) => {
      e.preventDefault();
      
      const formData = {
        from: document.getElementById('from').value,
        to: document.getElementById('to').value,
        departureDate: document.getElementById('departureDate').value,
        returnDate: document.getElementById('returnDate').value,
        class: document.getElementById('class').value,
        travellers: document.getElementById('travellers').value,
        tripType: document.querySelector('.trip-type-btn.active').dataset.type
      };

      // Validate required fields
      if (!formData.from || !formData.to || !formData.departureDate) {
        alert('Please fill out all required fields: From, To, and Departure Date.');
        return;
      }

      if (formData.tripType === 'round-trip' && !formData.returnDate) {
        alert('Please select a return date for round-trip flights.');
        return;
      }

      // Generate and open URL
      const baseUrl = 'https://www.ixigo.com/search/result/flight';
      
      const formatDate = (dateString) => {
        if (!dateString) return '';
        const [year, month, day] = dateString.split('-');
        return `${day}${month}${year}`;
      };

      const params = new URLSearchParams({
        from: formData.from,
        to: formData.to,
        date: formatDate(formData.departureDate),
        returnDate: formData.tripType === 'round-trip' ? formatDate(formData.returnDate) : '',
        adults: formData.travellers,
        children: '0',
        infants: '0',
        class: formData.class,
        source: 'Search Form'
      });

      const url = `${baseUrl}?${params.toString()}`;
      window.location.href = url; // Changed from window.open to window.location.href
    });

  // Date input formatting and validation
  const departureDateInput = document.getElementById('departureDate');
  const returnDateInput = document.getElementById('returnDate');

  departureDateInput.addEventListener('change', () => {
    if (returnDateInput.value) {
      validateDates();
    }
    returnDateInput.min = departureDateInput.value;
  });

  returnDateInput.addEventListener('change', validateDates);

  function validateDates() {
    const departureDate = new Date(departureDateInput.value);
    const returnDate = new Date(returnDateInput.value);
    
    if (returnDate < departureDate) {
      returnDateInput.value = departureDateInput.value;
    }
  }

  // Set minimum date to today
  const today = new Date().toISOString().split('T')[0];
  departureDateInput.min = today;
  returnDateInput.min = today;
}, 0);

return flightSearchHtml;
}

processTrainQuery(query) {
  const normalizedQuery = query.toLowerCase();

  // Station data mapping city names to station codes
  const stationData = {
    'mumbai': 'BCT',
    'delhi': 'NDLS',
    'bangalore': 'SBC',
    'bengaluru': 'SBC',
    'hyderabad': 'HYB',
    'chennai': 'MAS',
    'kolkata': 'HWH',
    'pune': 'PUNE',
    'ahmedabad': 'ADI',
    'indore': 'INDB',
    'bhopal': 'BPL'
  };

  // Enhanced train pattern to capture more details
  const trainPattern = /(?:book|search|find|check)?\s?(?:a )?train\s?(?:from )?([\w\s]+)?\s?(?:to )?([\w\s]+)?\s?(?:on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?/i;
  const match = normalizedQuery.match(trainPattern);

  if (match) {
    let fromStation = null;
    let toStation = null;

    // Extract stations from the query
    const stations = normalizedQuery.match(/\b(mumbai|delhi|bangalore|bengaluru|hyderabad|chennai|kolkata|pune|ahmedabad|indore|bhopal)\b/g);

    if (stations && stations.length >= 2) {
      // If the query explicitly mentions "from" and "to", use that order
      if (normalizedQuery.includes('from') && normalizedQuery.includes('to')) {
        const fromIndex = normalizedQuery.indexOf('from');
        const toIndex = normalizedQuery.indexOf('to');
        fromStation = normalizedQuery.slice(fromIndex + 5, toIndex).trim();
        toStation = normalizedQuery.slice(toIndex + 3).trim();
      } else {
        // Otherwise, assume the first station is "from" and the second is "to"
        fromStation = stations[0];
        toStation = stations[1];
      }
    } else if (stations && stations.length === 1) {
      // If only one station is mentioned, assume it's the "to" station
      toStation = stations[0];
    }

    const departureDate = match[3] ? this.parseDate(match[3].trim()) : null;

    return this.createTrainSearchResponse(fromStation, toStation, departureDate);
  }

  // If the query is just a generic train search
  if (/\b(?:search|find|book)\s?train\b/i.test(normalizedQuery)) {
    return this.createTrainSearchResponse();
  }

  return null;
}

createTrainSearchResponse(fromStation = null, toStation = null, departureDate = null) {
  const stationData = {
    'mumbai': 'BCT',
    'delhi': 'NDLS',
    'bangalore': 'SBC',
    'bengaluru': 'SBC',
    'hyderabad': 'HYB',
    'chennai': 'MAS',
    'kolkata': 'HWH',
    'pune': 'PUNE',
    'ahmedabad': 'ADI',
    'indore': 'INDB',
    'bhopal': 'BPL'
  };

  const createCustomSelect = (id, label, selectedStation) => {
  const stations = Object.keys(stationData);
  return `
    <div class="custom-select-container">
      <input type="text" 
             id="${id}-input" 
             class="station-search-input" 
             placeholder="${label}"
             autocomplete="off"
             ${selectedStation ? `value="${selectedStation}"` : ''}>
      <input type="hidden" 
             id="${id}" 
             name="${id}" 
             ${selectedStation ? `value="${stationData[selectedStation]}"` : ''}>
      <div id="${id}-dropdown" class="station-dropdown">
        <div class="dropdown-header">
          <span>Select Station</span>
          <button class="close-dropdown-btn">×</button>
        </div>
        <div class="station-options">
          ${stations.map(station => `
            <div class="station-option" 
                 data-station="${station}"
                 data-code="${stationData[station]}"
                 ${selectedStation === station ? 'data-selected="true"' : ''}>
              <div class="station-code">${stationData[station]}</div>
              <div class="station-name">${station.charAt(0).toUpperCase() + station.slice(1)}</div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;
};

const trainSearchHtml = `
  <div class="train-search-response">
    <div class="station-selection">
      ${createCustomSelect('departure', 'From', fromStation)}
      <button class="swap-stations-btn">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M8 3L4 7L8 11" />
          <path d="M4 7H20" />
          <path d="M16 21L20 17L16 13" />
          <path d="M20 17H4" />
        </svg>
      </button>
      ${createCustomSelect('destination', 'To', toStation)}
    </div>

    <div class="date-selection">
      <div class="date-field">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
          <line x1="16" y1="2" x2="16" y2="6" />
          <line x1="8" y1="2" x2="8" y2="6" />
          <line x1="3" y1="10" x2="21" y2="10" />
        </svg>
        <input type="date" id="departureDate" name="departureDate" value="${departureDate || ''}" required>
      </div>
    </div>

    <button type="submit" class="search-btn">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="11" cy="11" r="8" />
        <line x1="21" y1="21" x2="16.65" y2="16.65" />
      </svg>
      Search Train
    </button>
  </div>

    <style>
      .train-search-response {
        background: #FF69B4;
        border-radius: 24px;
        padding: 24px;
        max-width: 600px;
        margin: 0 auto;
      }

      .station-selection {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 24px;
      }

      .swap-stations-btn {
        background: rgba(0, 0, 0, 0.1);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .swap-stations-btn:hover {
        background: rgba(0, 0, 0, 0.2);
      }

      .custom-select-container {
        flex: 1;
        position: relative;
      }

      .station-search-input {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: rgba(0, 0, 0, 0.1);
        color: white;
        font-size: 16px;
      }

      .station-search-input::placeholder {
        color: rgba(255, 255, 255, 0.7);
      }

      .station-dropdown {
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background: white;
        border-radius: 12px;
        margin-top: 8px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        display: none;
        z-index: 1000;
        max-height: 300px;
        overflow-y: auto;
      }

      .dropdown-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 12px 16px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        color: #000;
        background: #fff;
        border-radius: 12px 12px 0 0;
        position: sticky;
        top: 0;
        z-index: 1;
      }

      .close-dropdown-btn {
        background: none;
        border: none;
        font-size: 24px;
        cursor: pointer;
        color: #666;
        padding: 0;
        line-height: 1;
      }

      .close-dropdown-btn:hover {
        color: #000;
      }

      .station-options {
        padding: 8px 0;
      }

      .station-option {
        padding: 12px 16px;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .station-option:hover {
        background: rgba(0, 0, 0, 0.05);
      }

      .station-code {
        font-weight: 600;
        color: #000;
      }

      .station-name {
        color: #666;
      }

      .date-selection {
        display: flex;
        gap: 12px;
        margin-bottom: 24px;
      }

      .date-field {
        flex: 1;
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .date-field input {
        background: transparent;
        border: none;
        color: white;
        font-size: 16px;
        width: 100%;
      }

      .search-btn {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: white;
        color: #FF69B4;
        font-size: 16px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .search-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      }
    

    @media (max-width: 480px) {
  .train-search-response {
    width: 83vw;
    padding: 16px;
    border-radius: 16px;
  }

  /* Station selection layout */
  .station-selection {
    flex-direction: row;
    gap: 12px;
    color: #FFF;
    align-items: center;
    flex-wrap: nowrap;
    margin-bottom: 16px;
  }

  .custom-select-container {
    flex: 1;
    min-width: 0;
    position: relative;
  }

  /* Station search input */
  .station-search-input {
    width: 100%;
    padding: 14px;
    font-size: 15px;
    height: 52px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    border-radius: 12px;
    background: #000;
    transition: all 0.2s ease;
  }

  .station-search-input:focus {
    border-color: #FFF;
    outline: none;
  }

  /* Bottom sheet station dropdown */
  .station-dropdown {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    top: auto;
    max-height: 75vh;
    overflow-y: auto;
    margin: 0;
    border-radius: 20px 20px 0 0;
    box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.12);
    padding: 24px 0 16px;
    z-index: 1001;
    background: #FFFFFF;
    transform: translateY(0);
    transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .station-dropdown.hidden {
    transform: translateY(100%);
  }

  .dropdown-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 16px 16px;
    color: #000;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }

  .close-dropdown-btn {
    background: none;
    border: none;
    font-size: 24px;
    cursor: pointer;
    color: #666;
  }

  .station-options {
    max-height: 60vh;
    overflow-y: auto;
  }

  /* Station option typography and layout */
  .station-code {
    font-size: 18px;
    font-weight: 600;
    min-width: 48px;
    color: #1A1A1A;
  }

  .station-name {
    font-size: 15px;
    font-weight: 400;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    color: #333333;
  }

  /* Modal overlay */
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(4px);
    z-index: 1000;
    opacity: 1;
    transition: opacity 0.3s ease;
  }

  .modal-overlay.hidden {
    opacity: 0;
    pointer-events: none;
  }

  /* Swap stations button */
  .swap-stations-btn {
    width: 36px;
    height: 36px;
    flex-shrink: 0;
    border-radius: 50%;
    background: #FFFFFF;
    border: 1px solid rgba(0, 0, 0, 0.12);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }

  .swap-stations-btn:active {
    background: rgba(0, 0, 0, 0.05);
    transform: scale(0.95);
  }

  /* Scrollbar styling */
  .station-dropdown::-webkit-scrollbar {
    width: 8px;
  }

  .station-dropdown::-webkit-scrollbar-track {
    background: transparent;
  }

  .station-dropdown::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 4px;
  }
}
    </style>
  `;

  // Add event listeners
  setTimeout(() => {
    const setupTrainStationSearch = (inputId) => {
  const input = document.getElementById(`${inputId}-input`);
  const hiddenInput = document.getElementById(inputId);
  const dropdown = document.getElementById(`${inputId}-dropdown`);
  const closeBtn = dropdown.querySelector('.close-dropdown-btn');
  const options = dropdown.querySelectorAll('.station-option');

  let isDropdownOpen = false;

  const openDropdown = () => {
    dropdown.style.display = 'block';
    isDropdownOpen = true;
  };

  const closeDropdown = () => {
    dropdown.style.display = 'none';
    isDropdownOpen = false;
  };

  // Open dropdown on input click (more reliable on mobile)
  input.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    if (!isDropdownOpen) {
      openDropdown();
    }
  });

  // Close dropdown when clicking outside
  document.addEventListener('click', (e) => {
    if (!input.contains(e.target) && !dropdown.contains(e.target)) {
      closeDropdown();
    }
  });

  // Close dropdown when clicking the close button
  closeBtn.addEventListener('click', (e) => {
    e.stopPropagation(); // Prevent event from bubbling up
    closeDropdown();
  });

  // Handle station selection
  options.forEach(option => {
    option.addEventListener('click', (e) => {
      e.stopPropagation(); // Prevent event from bubbling up
      const stationName = option.dataset.station;
      const stationCode = option.dataset.code;
      input.value = `${stationCode} - ${stationName.charAt(0).toUpperCase() + stationName.slice(1)}`;
      hiddenInput.value = stationCode;
      closeDropdown();
    });
  });

  // Filter options based on input
  input.addEventListener('input', (e) => {
    const searchText = e.target.value.toLowerCase();
    options.forEach(option => {
      const stationName = option.dataset.station;
      const stationCode = option.dataset.code;
      const matches = stationName.toLowerCase().includes(searchText) || 
                     stationCode.toLowerCase().includes(searchText);
      option.style.display = matches ? 'flex' : 'none';
    });
  });
};

// Train dropdown setup
  setupTrainStationSearch('departure');
  setupTrainStationSearch('destination');


    // Station swap functionality
    const swapBtn = document.querySelector('.swap-stations-btn');
    swapBtn.addEventListener('click', () => {
      const fromInput = document.getElementById('from-input');
      const toInput = document.getElementById('to-input');
      const fromHidden = document.getElementById('from');
      const toHidden = document.getElementById('to');

      const tempValue = fromInput.value;
      const tempHidden = fromHidden.value;
      
      fromInput.value = toInput.value;
      fromHidden.value = toHidden.value;
      toInput.value = tempValue;
      toHidden.value = tempHidden;
    });

    // Form submission
    document.querySelector('.search-btn').addEventListener('click', (e) => {
  e.preventDefault();
  
  const formData = {
    from: document.getElementById('departure').value,
    to: document.getElementById('destination').value,
    departureDate: document.getElementById('departureDate').value,
  };

  // Validate required fields
  if (!formData.from || !formData.to || !formData.departureDate) {
    alert('Please fill out all required fields: From, To, and Departure Date.');
    return;
  }

  // Generate and open URL
  const baseUrl = 'https://www.ixigo.com/search/result/train';
  
  const formatDate = (dateString) => {
    if (!dateString) return '';
    const [year, month, day] = dateString.split('-');
    return `${day}${month}${year}`;
  };

  const params = new URLSearchParams({
    from: formData.from,
    to: formData.to,
    date: formatDate(formData.departureDate),
  });

  const url = `${baseUrl}/${formData.from}/${formData.to}/${formatDate(formData.departureDate)}//1/0/0/0/all`;
  window.location.href = url;
});

    // Date input formatting and validation
    const departureDateInput = document.getElementById('departureDate');

    // Set minimum date to today
    const today = new Date().toISOString().split('T')[0];
    departureDateInput.min = today;
  }, 0);

  return trainSearchHtml;
}

processLiveTrainStatus(query) {
  const normalizedQuery = query.toLowerCase();

  // Train data mapping train names to train numbers
  const trainData = {
    '14722': 'Abohar Jodhpur Express',
    '54703': 'Abohar Jodhpur Passenger',
    '79438': 'Abu Road Mahesana DEMU',
    '55338': 'Achhnera Kasganj Fast Passenger',
    '55332': 'Achhnera Kasganj Passenger',
    '22188': 'Adhartal Rani Kamlapati Intercity Express',
    '17409': 'Adilabad Hazur Sahib Nanded Express'
  };

  // Enhanced train pattern to capture train name or number
  const trainPattern = /(?:live|status|check)?\s?(?:train)?\s?(?:status)?\s?(\d{5})?\s?([\w\s]+)?/i;
  const match = normalizedQuery.match(trainPattern);

  if (match) {
    const trainNumber = match[1];
    const trainName = match[2] ? match[2].trim() : null;

    // Find the train number if only the name is provided
    const foundTrainNumber = trainNumber || Object.keys(trainData).find(key => trainData[key].toLowerCase().includes(trainName.toLowerCase()));

    if (foundTrainNumber) {
      const trainName = trainData[foundTrainNumber];
      return this.createLiveTrainStatusResponse(foundTrainNumber, trainName);
    }
  }

  // If the query is just a generic live train status search
  if (/\b(?:live|status|check)\s?train\b/i.test(normalizedQuery)) {
    return this.createLiveTrainStatusResponse();
  }

  return null;
}

createLiveTrainStatusResponse(trainNumber = null, trainName = null) {
  const liveTrainStatusHtml = `
    <div class="live-train-status-response">
      <div class="train-input">
        <input type="text" 
               id="trainNumberInput" 
               class="train-search-input" 
               placeholder="Enter Train Number or Name"
               autocomplete="off"
               ${trainNumber ? `value="${trainNumber}"` : ''}>
        <input type="hidden" 
               id="trainNumber" 
               name="trainNumber" 
               ${trainNumber ? `value="${trainNumber}"` : ''}>
      </div>

      <button type="submit" class="check-status-btn">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <circle cx="11" cy="11" r="8" />
          <line x1="21" y1="21" x2="16.65" y2="16.65" />
        </svg>
        Check Live Status
      </button>
    </div>

    <style>
      .live-train-status-response {
        background: #FF69B4;
        border-radius: 24px;
        padding: 24px;
        max-width: 600px;
        margin: 0 auto;
      }

      .train-input {
        margin-bottom: 24px;
      }

      .train-search-input {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: rgba(0, 0, 0, 0.1);
        color: white;
        font-size: 16px;
      }

      .train-search-input::placeholder {
        color: rgba(255, 255, 255, 0.7);
      }

      .check-status-btn {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: white;
        color: #FF69B4;
        font-size: 16px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .check-status-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      }
    </style>
  `;

  // Add event listeners
  setTimeout(() => {
    const trainNumberInput = document.getElementById('trainNumberInput');
    const trainNumberHidden = document.getElementById('trainNumber');

    // Handle form submission
    document.querySelector('.check-status-btn').addEventListener('click', (e) => {
      e.preventDefault();

      const trainNumber = trainNumberHidden.value;
      const trainName = trainNumberInput.value;

      if (!trainNumber && !trainName) {
        alert('Please enter a valid train number or name.');
        return;
      }

      // Generate and open URL
      const baseUrl = 'https://www.redbus.in/railways/liveTrainDetails';
      const params = new URLSearchParams({
        trainNo: trainNumber || '',
        trainName: trainName || '',
        stn: 'null',
        from: 'LTS Home'
      });

      const url = `${baseUrl}?${params.toString()}`;
      window.location.href = url;
    });

    // Auto-fill train number if train name is entered
    trainNumberInput.addEventListener('input', (e) => {
      const searchText = e.target.value.toLowerCase();
      const foundTrainNumber = Object.keys(trainData).find(key => trainData[key].toLowerCase().includes(searchText));

      if (foundTrainNumber) {
        trainNumberHidden.value = foundTrainNumber;
      } else {
        trainNumberHidden.value = '';
      }
    });
  }, 0);

  return liveTrainStatusHtml;
}

processBusQuery(query) {
  const normalizedQuery = query.toLowerCase();

  // Bus city data mapping city names to city IDs
  const busCityData = {
    'mumbai': { name: 'Mumbai', id: '123' },
    'delhi': { name: 'Delhi', id: '456' },
    'bangalore': { name: 'Bangalore', id: '789' },
    'bengaluru': { name: 'Bangalore', id: '789' },
    'hyderabad': { name: 'Hyderabad', id: '101' },
    'chennai': { name: 'Chennai', id: '202' },
    'kolkata': { name: 'Kolkata', id: '303' },
    'pune': { name: 'Pune', id: '404' },
    'ahmedabad': { name: 'Ahmedabad', id: '505' },
    'indore': { name: 'Indore', id: '313' },
    'bhopal': { name: 'Bhopal', id: '979' }
  };

  // Enhanced bus pattern to capture more details
  const busPattern = /(?:book|search|find|check)?\s?(?:a )?bus\s?(?:from )?([\w\s]+)?\s?(?:to )?([\w\s]+)?\s?(?:on )?(\d{1,2}(?:th|st|nd|rd)?\s?\w+)?/i;
  const match = normalizedQuery.match(busPattern);

  if (match) {
    let fromCity = null;
    let toCity = null;

    // Extract cities from the query
    const cities = normalizedQuery.match(/\b(mumbai|delhi|bangalore|bengaluru|hyderabad|chennai|kolkata|pune|ahmedabad|indore|bhopal)\b/g);

    if (cities && cities.length >= 2) {
      // If the query explicitly mentions "from" and "to", use that order
      if (normalizedQuery.includes('from') && normalizedQuery.includes('to')) {
        const fromIndex = normalizedQuery.indexOf('from');
        const toIndex = normalizedQuery.indexOf('to');
        fromCity = normalizedQuery.slice(fromIndex + 5, toIndex).trim();
        toCity = normalizedQuery.slice(toIndex + 3).trim();
      } else {
        // Otherwise, assume the first city is "from" and the second is "to"
        fromCity = cities[0];
        toCity = cities[1];
      }
    } else if (cities && cities.length === 1) {
      // If only one city is mentioned, assume it's the "to" city
      toCity = cities[0];
    }

    const departureDate = match[3] ? this.parseDate(match[3].trim()) : null;

    return this.createBusSearchResponse(fromCity, toCity, departureDate);
  }

  // If the query is just a generic bus search
  if (/\b(?:search|find|book)\s?bus\b/i.test(normalizedQuery)) {
    return this.createBusSearchResponse();
  }

  return null;
}

createBusSearchResponse(fromCity = null, toCity = null, departureDate = null) {
  const busCityData = {
    'mumbai': { name: 'Mumbai', id: '123' },
    'delhi': { name: 'Delhi', id: '456' },
    'bangalore': { name: 'Bangalore', id: '789' },
    'bengaluru': { name: 'Bangalore', id: '789' },
    'hyderabad': { name: 'Hyderabad', id: '101' },
    'chennai': { name: 'Chennai', id: '202' },
    'kolkata': { name: 'Kolkata', id: '303' },
    'pune': { name: 'Pune', id: '404' },
    'ahmedabad': { name: 'Ahmedabad', id: '505' },
    'indore': { name: 'Indore', id: '313' },
    'bhopal': { name: 'Bhopal', id: '979' }
  };

  const createCustomSelect = (id, label, selectedCity) => {
  const cities = Object.keys(busCityData);
  return `
    <div class="custom-select-container">
      <input type="text" 
             id="${id}-input" 
             class="city-search-input" 
             placeholder="${label}"
             autocomplete="off"
             ${selectedCity ? `value="${selectedCity}"` : ''}>
      <input type="hidden" 
             id="${id}" 
             name="${id}" 
             ${selectedCity ? `value="${busCityData[selectedCity].id}"` : ''}>
      <div id="${id}-dropdown" class="city-dropdown">
        <div class="dropdown-header">
          <span>Select City</span>
          <button class="close-dropdown-btn">×</button>
        </div>
        <div class="city-options">
          ${cities.map(city => `
            <div class="city-option" 
                 data-city="${city}"
                 data-id="${busCityData[city].id}"
                 ${selectedCity === city ? 'data-selected="true"' : ''}>
              <div class="city-id">${busCityData[city].id}</div>
              <div class="city-name">${busCityData[city].name}</div>
            </div>
          `).join('')}
        </div>
      </div>
    </div>
  `;
};

const busSearchHtml = `
  <div class="bus-search-response">
    <div class="city-selection">
      ${createCustomSelect('origin', 'From', fromCity)}
      <button class="swap-cities-btn">
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <path d="M8 3L4 7L8 11" />
          <path d="M4 7H20" />
          <path d="M16 21L20 17L16 13" />
          <path d="M20 17H4" />
        </svg>
      </button>
      ${createCustomSelect('arrival', 'To', toCity)}
    </div>

    <div class="date-selection">
      <div class="date-field">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
          <rect x="3" y="4" width="18" height="18" rx="2" ry="2" />
          <line x1="16" y1="2" x2="16" y2="6" />
          <line x1="8" y1="2" x2="8" y2="6" />
          <line x1="3" y1="10" x2="21" y2="10" />
        </svg>
        <input type="date" id="departureDate" name="departureDate" value="${departureDate || ''}" required>
      </div>
    </div>

    <button type="submit" class="search-btn">
      <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <circle cx="11" cy="11" r="8" />
        <line x1="21" y1="21" x2="16.65" y2="16.65" />
      </svg>
      Check Bus
    </button>
  </div>

    <style>
      .bus-search-response {
        background: #FF69B4;
        border-radius: 24px;
        padding: 24px;
        max-width: 600px;
        margin: 0 auto;
      }

      .city-selection {
        display: flex;
        align-items: center;
        gap: 12px;
        margin-bottom: 24px;
      }

      .swap-cities-btn {
        background: rgba(0, 0, 0, 0.1);
        border: none;
        border-radius: 50%;
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .swap-cities-btn:hover {
        background: rgba(0, 0, 0, 0.2);
      }

      .custom-select-container {
        flex: 1;
        position: relative;
      }

      .city-search-input {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: rgba(0, 0, 0, 0.1);
        color: white;
        font-size: 16px;
      }

      .city-search-input::placeholder {
        color: rgba(255, 255, 255, 0.7);
      }

      .city-dropdown {
        position: absolute;
        top: 100%;
        left: 0;
        right: 0;
        background: white;
        border-radius: 12px;
        margin-top: 8px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        display: none;
        z-index: 1000;
        max-height: 300px;
        overflow-y: auto;
      }

      .dropdown-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 12px 16px;
        border-bottom: 1px solid rgba(0, 0, 0, 0.1);
        color: #000;
        background: #fff;
        border-radius: 12px 12px 0 0;
        position: sticky;
        top: 0;
        z-index: 1;
      }

      .close-dropdown-btn {
        background: none;
        border: none;
        font-size: 24px;
        cursor: pointer;
        color: #666;
        padding: 0;
        line-height: 1;
      }

      .close-dropdown-btn:hover {
        color: #000;
      }

      .city-options {
        padding: 8px 0;
      }

      .city-option {
        padding: 12px 16px;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .city-option:hover {
        background: rgba(0, 0, 0, 0.05);
      }

      .city-id {
        font-weight: 600;
        color: #000;
      }

      .city-name {
        color: #666;
      }

      .date-selection {
        display: flex;
        gap: 12px;
        margin-bottom: 24px;
      }

      .date-field {
        flex: 1;
        background: rgba(0, 0, 0, 0.1);
        border-radius: 12px;
        padding: 16px;
        display: flex;
        align-items: center;
        gap: 12px;
      }

      .date-field input {
        background: transparent;
        border: none;
        color: white;
        font-size: 16px;
        width: 100%;
      }

      .search-btn {
        width: 100%;
        padding: 16px;
        border: none;
        border-radius: 12px;
        background: white;
        color: #FF69B4;
        font-size: 16px;
        font-weight: 600;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
        cursor: pointer;
        transition: all 0.3s ease;
      }

      .search-btn:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
      }

      @media (max-width: 480px) {
  .bus-search-response {
    width: 83vw;
    padding: 16px;
    border-radius: 16px;
  }

  /* City selection layout */
  .city-selection {
    flex-direction: row;
    gap: 12px;
    color: #FFF;
    align-items: center;
    flex-wrap: nowrap;
    margin-bottom: 16px;
  }

  .custom-select-container {
    flex: 1;
    min-width: 0;
    position: relative;
  }

  /* City search input */
  .city-search-input {
    width: 100%;
    padding: 14px;
    font-size: 15px;
    height: 52px;
    border: 1px solid rgba(0, 0, 0, 0.12);
    border-radius: 12px;
    background: #000;
    transition: all 0.2s ease;
  }

  .city-search-input:focus {
    border-color: #FFF;
    outline: none;
  }

  /* Bottom sheet city dropdown */
  .city-dropdown {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    top: auto;
    max-height: 75vh;
    overflow-y: auto;
    margin: 0;
    border-radius: 20px 20px 0 0;
    box-shadow: 0 -4px 16px rgba(0, 0, 0, 0.12);
    padding: 24px 0 16px;
    z-index: 1001;
    background: #FFFFFF;
    transform: translateY(0);
    transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .city-dropdown.hidden {
    transform: translateY(100%);
  }

  .dropdown-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 16px 16px;
    color: #000;
    border-bottom: 1px solid rgba(0, 0, 0, 0.1);
  }

  .close-dropdown-btn {
    background: none;
    border: none;
    font-size: 24px;
    cursor: pointer;
    color: #666;
  }

  .city-options {
    max-height: 60vh;
    overflow-y: auto;
  }

  /* City option typography and layout */
  .city-id {
    font-size: 18px;
    font-weight: 600;
    min-width: 48px;
    color: #1A1A1A;
  }

  .city-name {
    font-size: 15px;
    font-weight: 400;
    flex: 1;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    color: #333333;
  }

  /* Modal overlay */
  .modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.4);
    backdrop-filter: blur(4px);
    z-index: 1000;
    opacity: 1;
    transition: opacity 0.3s ease;
  }

  .modal-overlay.hidden {
    opacity: 0;
    pointer-events: none;
  }

  /* Swap cities button */
  .swap-cities-btn {
    width: 36px;
    height: 36px;
    flex-shrink: 0;
    border-radius: 50%;
    background: #FFFFFF;
    border: 1px solid rgba(0, 0, 0, 0.12);
    display: flex;
    align-items: center;
    justify-content: center;
    transition: all 0.2s ease;
  }

  .swap-cities-btn:active {
    background: rgba(0, 0, 0, 0.05);
    transform: scale(0.95);
  }

  /* Scrollbar styling */
  .city-dropdown::-webkit-scrollbar {
    width: 8px;
  }

  .city-dropdown::-webkit-scrollbar-track {
    background: transparent;
  }

  .city-dropdown::-webkit-scrollbar-thumb {
    background: rgba(0, 0, 0, 0.2);
    border-radius: 4px;
  }
}
    </style>
  `;

  // Add event listeners
  setTimeout(() => {
    const setupBusCitySearch = (inputId) => {
      const input = document.getElementById(`${inputId}-input`);
      const hiddenInput = document.getElementById(inputId);
      const dropdown = document.getElementById(`${inputId}-dropdown`);
      const closeBtn = dropdown.querySelector('.close-dropdown-btn');
      const options = dropdown.querySelectorAll('.city-option');

      let isDropdownOpen = false;

      const openDropdown = () => {
        dropdown.style.display = 'block';
        isDropdownOpen = true;
      };

      const closeDropdown = () => {
        dropdown.style.display = 'none';
        isDropdownOpen = false;
      };

      input.addEventListener('click', (e) => {
        e.stopPropagation();
        if (!isDropdownOpen) {
          openDropdown();
        }
      });

      document.addEventListener('click', (e) => {
        if (!input.contains(e.target) && !dropdown.contains(e.target)) {
          closeDropdown();
        }
      });

      closeBtn.addEventListener('click', (e) => {
        e.stopPropagation();
        closeDropdown();
      });

      options.forEach(option => {
        option.addEventListener('click', (e) => {
          e.stopPropagation();
          const cityName = option.dataset.city;
          const cityId = option.dataset.id;
          input.value = `${cityId} - ${busCityData[cityName].name}`;
          hiddenInput.value = cityId;
          closeDropdown();
        });
      });

      input.addEventListener('input', (e) => {
        const searchText = e.target.value.toLowerCase();
        options.forEach(option => {
          const cityName = option.dataset.city;
          const cityId = option.dataset.id;
          const matches = cityName.toLowerCase().includes(searchText) || 
                         cityId.toLowerCase().includes(searchText);
          option.style.display = matches ? 'flex' : 'none';
        });
      });
    };

    setupBusCitySearch('origin');
    setupBusCitySearch('arrival');

    const swapBtn = document.querySelector('.swap-cities-btn');
    swapBtn.addEventListener('click', () => {
      const fromInput = document.getElementById('origin-input');
      const toInput = document.getElementById('arrival-input');
      const fromHidden = document.getElementById('origin');
      const toHidden = document.getElementById('arrival');

      const tempValue = fromInput.value;
      const tempHidden = fromHidden.value;
      
      fromInput.value = toInput.value;
      fromHidden.value = toHidden.value;
      toInput.value = tempValue;
      toHidden.value = tempHidden;
    });

    document.querySelector('.search-btn').addEventListener('click', (e) => {
      e.preventDefault();
      
      const fromCityId = document.getElementById('origin').value;
      const toCityId = document.getElementById('arrival').value;
      const departureDate = document.getElementById('departureDate').value;

      if (!fromCityId || !toCityId || !departureDate) {
        alert('Please fill out all required fields: From, To, and Departure Date.');
        return;
      }

      const fromCityName = document.getElementById('origin-input').value.split(' - ')[1];
      const toCityName = document.getElementById('arrival-input').value.split(' - ')[1];

      const formatDate = (dateString) => {
        const [year, month, day] = dateString.split('-');
        const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
        return `${day}-${months[parseInt(month) - 1]}-${year}`;
      };

      const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);

      let url;
      if (isMobile) {
        url = `https://www.redbus.in/bus-tickets/${fromCityName.toLowerCase()}-to-${toCityName.toLowerCase()}?fromCityId=${fromCityId}&fromCityName=${encodeURIComponent(fromCityName)}&toCityId=${toCityId}&toCityName=${encodeURIComponent(toCityName)}&doj=${formatDate(departureDate)}&srcType=null&destType=null&srcCountry=IND&destCountry=IND&ref=mweb&step=srp`;
      } else {
        url = `https://www.redbus.in/search?fromCityName=${encodeURIComponent(fromCityName)}&fromCityId=${fromCityId}&toCityName=${encodeURIComponent(toCityName)}&toCityId=${toCityId}&onward=${formatDate(departureDate)}&srcCountry=IND&destCountry=IND`;
      }

      window.location.href = url;
    });

    const departureDateInput = document.getElementById('departureDate');
    const today = new Date().toISOString().split('T')[0];
    departureDateInput.min = today;
  }, 0);

  return busSearchHtml;
}

processDiseaseQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Extract disease name from the query
    const diseaseMatch = normalizedQuery.match(/(asthma|diabetes|flu|cancer|hypertension)/i);
    if (!diseaseMatch) return null;

    const diseaseName = diseaseMatch[0].toLowerCase();

    // Find the disease in the database
    const disease = searchConfig.diseaseDatabase.diseases.find(d => 
      d.name.toLowerCase() === diseaseName
    );

    if (disease) {
      return this.generateDiseaseResponse(disease);
    }

    return null;
  }

  generateDiseaseResponse(disease) {
  const metadata = disease.metadata ? `
    <div class="disease-info hidden">
      ${Object.entries(disease.metadata).map(([section, items]) => `
        <div class="info-section">
          <h3>${section}</h3>
          <ul>
            ${items.map(item => `<li>${item}</li>`).join('')}
          </ul>
        </div>
      `).join('')}
    </div>
  ` : '';

  const actionButtons = disease.actionButtons.map(btn => {
    if (btn.label === 'Find a Doctor') {
      return `
        <button class="action-button" onclick="openFindDoctorsModal()">
          <i class="fas ${btn.icon}"></i> ${btn.label}
        </button>
      `;
    }
    return `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `;
  }).join('');

  return `
    <div class="disease-item" onclick="toggleDiseaseInfo(this)">
      <div class="disease-header">
        <div class="disease-name">${disease.name}</div>
      </div>
      <div class="disease-description">${disease.description}</div>
      <div class="action-buttons">
        ${actionButtons}
      </div>
      ${metadata}
    </div>
  `;
}

  processDomainRegistrationQuery(query) {
    const domainPattern = /(?:register|check) (?:a )?domain (?:named )?([\w-]+\.\w+)/i;
    const match = query.match(domainPattern);
    if (match) {
      const domain = match[1];
      const url = `https://www.godaddy.com/en-in/domainsearch/find?domainToCheck=${domain}`;
      return this.createDomainRegistrationResponse(domain, url);
    }
    return null;
  }

  processStockQuery(query) {
  // Enhanced stock pattern to match stock/share price queries with more variations
  const stockPattern = /(?:\b(?:stock price|what is share price of|share price of|share price|live stock price|current stock price|market price|stock value|share value|stock rate|share rate)\b)[^\w&.-]*([\w&.-]+)|([\w&.-]+)[^\w&.-]*\b(?:stock price|share price|live stock price|current stock price|market price|stock value|share value|stock rate|share rate)\b/i;
  const match = query.match(stockPattern);

  // Check if the query explicitly mentions stock-related keywords
  const isStockQuery = /\b(stock price|share price of|what is share price of|share price|live stock price|current stock price|market price|stock value|share value|stock rate|share rate)\b/i.test(query);

  if (match && isStockQuery) {
    // Determine which capture group has the stock name
    const stock = (match[1] || match[2]).trim().toUpperCase();

    // Map of company names to their respective URLs (normalized keys)
    const stockUrls = {
      'RELIANCE': 'https://upstox.com/stocks/reliance-industries-ltd-share-price/',
      'TCS': 'https://upstox.com/stocks/tata-consultancy-serv-lt-share-price/INE467B01029/',
      'TESLA': 'https://finance.yahoo.com/quote/TSLA/',
      'APPLE': 'https://finance.yahoo.com/quote/AAPL/',
      'GOOGLE': 'https://finance.yahoo.com/quote/GOOGL/',
      'MICROSOFT': 'https://finance.yahoo.com/quote/MSFT/',
      // Add other stocks here...
    };

    // Get the URL for the queried stock
    const stockUrl = stockUrls[stock] || null;

    // Always include the button in the response
    return this.createStockResponse(stock, stockUrl);
  }
  return null;
}


  createDomainRegistrationResponse(domain, url) {
    return `
      <div class="domain-registration-response">
        <p>🌐 Domain Registration Details:</p>
        <ul>
          <li>Domain: ${domain}</li>
        </ul>
        <button class="click-button" data-url="${url}">
          <i class="fas fa-globe"></i> Check Domain
        </button>
      </div>
    `;
  }

  createStockResponse(stock, url) {
  // If the URL is available, show an enabled button with the correct URL
  const buttonHtml = url
    ? `<button class="click-button" data-url="${url}">
         <i class="fas fa-chart-line"></i> Check Stock
       </button>`
    : `<button class="click-button" disabled>
         <i class="fas fa-chart-line"></i> Stock Not Supported
       </button>`;

  return `
    <div class="stock-response">
      <p>📈 Stock Details:</p>
      <ul>
        <li>Stock: ${stock}</li>
      </ul>
      ${buttonHtml}
    </div>
  `;
}

processProviderQuery(query) {
    const normalizedQuery = query.toLowerCase();
    
    // Extract service type and location from the query
    const serviceMatch = normalizedQuery.match(/(wifi|solar energy|broadband|internet)/i);
    const locationMatch = normalizedQuery.match(/(delhi|bhopal|mumbai|bangalore)/i);
    
    if (!serviceMatch || !locationMatch) return null;

    const service = serviceMatch[0].toLowerCase();
    const location = locationMatch[0].toLowerCase();

    // Get relevant providers
    const providers = searchConfig.providerDatabase[service]?.filter(provider => 
      provider.location.toLowerCase().includes(location)
    );

    if (providers && providers.length > 0) {
      return this.generateProviderResponse(providers, service, location);
    }

    return null;
  }

  generateProviderResponse(providers, service, location) {
    const providerList = providers.map(provider => this.formatProvider(provider)).join('');

    return `
      <div class="provider-container">
        <div class="provider-header">
          <h2>${service.charAt(0).toUpperCase() + service.slice(1)} Providers in ${location.charAt(0).toUpperCase() + location.slice(1)}</h2>
        </div>
        <div class="providers-list">
          ${providerList}
        </div>
      </div>
    `;
  }

  formatProvider(provider) {
    const metadata = provider.metadata ? `
      <div class="provider-info hidden">
        ${Object.entries(provider.metadata).map(([section, items]) => `
          <div class="info-section">
            <h3>${section}</h3>
            <ul>
              ${items.map(item => `<li>${item}</li>`).join('')}
            </ul>
          </div>
        `).join('')}
      </div>
    ` : '';

    const actionButtons = provider.actionButtons.map(btn => `
      <a href="${btn.url}" class="action-button">
        <i class="fas ${btn.icon}"></i> ${btn.label}
      </a>
    `).join('');

    return `
      <div class="provider-item" onclick="toggleProviderInfo(this)">
        <div class="provider-header">
          <div class="provider-name">${provider.name}</div>
          <div class="provider-rating">⭐ ${provider.rating}</div>
        </div>
        <div class="provider-details">
          <div class="provider-location">📍 ${provider.location}</div>
          <div class="provider-contact">📞 ${provider.contact}</div>
        </div>
        <div class="provider-description">${provider.description}</div>
        <div class="action-buttons">
          ${actionButtons}
        </div>
        ${metadata}
      </div>
    `;
  }


processAutomotiveQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Check if the query is related to cars or bikes
    const isCarQuery = normalizedQuery.includes('car') || normalizedQuery.includes('cars');
    const isBikeQuery = normalizedQuery.includes('bike') || normalizedQuery.includes('bikes');

    if (isCarQuery || isBikeQuery) {
      const vehicles = searchConfig.automotiveDatabase.vehicles.filter(vehicle =>
        isCarQuery ? vehicle.type === 'car' : vehicle.type === 'bike'
      );
      return this.generateAutomotiveResponse(vehicles, isCarQuery ? 'car' : 'bike');
    }

    return null;
  }

  generateAutomotiveResponse(vehicles, type) {
    const vehicleList = vehicles.map(vehicle => this.formatVehicle(vehicle)).join('');

    return `
      <div class="automotive-response">
        <p>🚗 Here are some ${type}s :</p>
        <div class="vehicles-list">
          ${vehicleList}
        </div>
      </div>
    `;
  }

  formatVehicle(vehicle) {
  const carousel = this.createAutomotiveCarousel(vehicle.images); // Use the new automotive carousel
  const vehicleInfo = vehicle.vehicleInfo ? `
    <div class="vehicle-info hidden">
      ${Object.entries(vehicle.vehicleInfo).map(([section, items]) => `
        <div class="info-section">
          <h3>${section}</h3>
          <ul>
            ${items.map(item => `<li>${item}</li>`).join('')}
          </ul>
        </div>
      `).join('')}
    </div>
  ` : '';

  return `
    <div class="vehicle-item" onclick="toggleVehicleInfo(this)">
      <div class="vehicle-header">
        <div class="vehicle-title">${vehicle.title}</div>
        <div class="vehicle-price">${vehicle.price}</div>
      </div>
      <div class="vehicle-details">
        <div class="vehicle-brand">🏷️ ${vehicle.brand}</div>
        <div class="vehicle-fuel">⛽ ${vehicle.fuelType}</div>
        <div class="vehicle-seating">🪑 ${vehicle.seatingCapacity} Seater</div>
      </div>
      ${carousel} <!-- Use the new automotive carousel here -->
      <div class="vehicle-description">${vehicle.description}</div>
      <div class="vehicle-contact">📞 ${vehicle.contact}</div>
      <div class="action-buttons">
        <a href="${vehicle.bookingLink}" class="book-now-button" target="_blank">
          <i class="fas fa-calendar-check"></i> Book Now
        </a>
        <a href="${vehicle.whatsappLink}" class="whatsapp-button" target="_blank">
          <i class="fab fa-whatsapp"></i> WhatsApp
        </a>
      </div>
      ${vehicleInfo}
    </div>
  `;
}

createAutomotiveCarousel(images) {
  return `
    <div class="automotive-carousel">
      ${images.map((image, index) => `
        <div class="automotive-carousel-item ${index === 0 ? 'active' : ''}">
          <img src="${image}" alt="Vehicle Image ${index + 1}">
        </div>
      `).join('')}
      <button class="automotive-carousel-control prev" onclick="changeAutomotiveSlide(this.parentElement, -1)">&#10094;</button>
      <button class="automotive-carousel-control next" onclick="changeAutomotiveSlide(this.parentElement, 1)">&#10095;</button>
    </div>
  `;
}

  processRealEstateQuery(query) {
    const normalizedQuery = query.toLowerCase();

    // Real estate-related keywords and synonyms
    const realEstateKeywords = ['property', 'real estate', 'house', 'apartment', 'villa', 'flat', 'rent', 'buy', 'sell'];
    const isRealEstateQuery = realEstateKeywords.some(keyword => normalizedQuery.includes(keyword));
    if (!isRealEstateQuery) return null;

    // Extract real estate search context
    const context = {
      type: this.extractPropertyType(normalizedQuery),
      location: this.extractLocation(normalizedQuery),
      price: this.extractPriceRange(normalizedQuery),
      bedrooms: this.extractBedrooms(normalizedQuery),
      amenities: this.extractAmenities(normalizedQuery)
    };

    // Get relevant properties
    const relevantProperties = this.getRelevantProperties(context);

    // Generate response
    return this.generateRealEstateResponse(relevantProperties, context);
  }

  extractPropertyType(query) {
    const types = ['apartment', 'villa', 'house', 'flat'];
    return types.find(type => query.includes(type)) || null;
  }

  extractPriceRange(query) {
    const rangeMatch = query.match(/(between)\s*₹?\s*(\d+,\d+|\d+)\s*and\s*₹?\s*(\d+,\d+|\d+)/i);
    if (rangeMatch) {
      const minAmount = parseFloat(rangeMatch[2].replace(/,/g, ''));
      const maxAmount = parseFloat(rangeMatch[3].replace(/,/g, ''));
      return { min: minAmount, max: maxAmount };
    }

    const priceMatch = query.match(/(under|less than|below|above|more than)\s*₹?\s*(\d+,\d+|\d+)/i);
    if (priceMatch) {
      const operator = priceMatch[1].toLowerCase();
      const amount = parseFloat(priceMatch[2].replace(/,/g, ''));
      return { operator, amount };
    }

    return null;
  }

  extractBedrooms(query) {
    const bedroomMatch = query.match(/(\d+)\s*(BHK|bedroom|bedrooms)/i);
    return bedroomMatch ? parseInt(bedroomMatch[1]) : null;
  }

  extractAmenities(query) {
    const amenities = ['swimming pool', 'gym', 'parking', 'security', 'garden'];
    return amenities.filter(amenity => query.includes(amenity));
  }

  getRelevantProperties(context) {
    let properties = searchConfig.realEstateDatabase.properties;

    // Filter by type
    if (context.type) {
      properties = properties.filter(property => property.type.toLowerCase().includes(context.type));
    }

    // Filter by location
    if (context.location) {
      properties = properties.filter(property => property.location.toLowerCase().includes(context.location));
    }

    // Filter by price range
    if (context.price) {
      if (context.price.min && context.price.max) {
        properties = properties.filter(property => {
          const propertyPrice = parseFloat(property.price.replace(/[^0-9]/g, ''));
          return propertyPrice >= context.price.min && propertyPrice <= context.price.max;
        });
      } else if (context.price.operator && context.price.amount) {
        const { operator, amount } = context.price;
        properties = properties.filter(property => {
          const propertyPrice = parseFloat(property.price.replace(/[^0-9]/g, ''));
          if (operator === 'under' || operator === 'less than' || operator === 'below') {
            return propertyPrice <= amount;
          } else if (operator === 'above' || operator === 'more than') {
            return propertyPrice >= amount;
          }
          return true;
        });
      }
    }

    // Filter by number of bedrooms
    if (context.bedrooms) {
      properties = properties.filter(property => property.bedrooms >= context.bedrooms);
    }

    // Filter by amenities
    if (context.amenities.length > 0) {
      properties = properties.filter(property => 
        context.amenities.every(amenity => property.amenities.map(a => a.toLowerCase()).includes(amenity))
      );
    }

    return properties.slice(0, 5); // Return top 5 results
  }

  generateRealEstateResponse(properties, context) {
    if (properties.length === 0) {
      return "Sorry, I couldn't find any properties matching your request. Please try a different query.";
    }

    const header = this.generateRealEstateHeader(context);
    const propertyList = properties.map(property => this.formatProperty(property)).join('');

    return `
      <div class="real-estate-container">
        ${header}
        <div class="properties-list">
          ${propertyList}
        </div>
      </div>
    `;
  }

  generateRealEstateHeader(context) {
    let headerText = 'Here are some properties';

    if (context.location) {
        headerText += ` in ${context.location}`;
    }

    if (context.type) {
        headerText += ` of type ${context.type}`;
    }

    if (context.price) {
        if (context.price.min && context.price.max) {
            headerText += ` with price between ₹${context.price.min.toLocaleString()} and ₹${context.price.max.toLocaleString()}`;
        } else if (context.price.operator && context.price.amount) {
            headerText += ` with price ${context.price.operator} ₹${context.price.amount.toLocaleString()}`;
        }
    }

    if (context.bedrooms) {
        headerText += ` with ${context.bedrooms} bedrooms`;
    }

    if (context.amenities.length > 0) {
        headerText += ` with amenities: ${context.amenities.join(', ')}`;
    }

    return `<div class="real-estate-header"><h2>${headerText}</h2></div>`;
}

  formatProperty(property) {
    const carousel = this.createImageCarousel(property.images);
    const propertyInfo = property.propertyInfo ? `
      <div class="property-info hidden">
        ${Object.entries(property.propertyInfo).map(([section, items]) => `
          <div class="info-section">
            <h3>${section}</h3>
            <ul>
              ${items.map(item => `<li>${item}</li>`).join('')}
            </ul>
          </div>
        `).join('')}
      </div>
    ` : '';

    // Add "Book Now" and "WhatsApp" buttons
    const actionButtons = `
      <div class="action-buttons">
        ${property.bookingLink ? `
          <a href="${property.bookingLink}" class="book-now-button" target="_blank">
            <i class="fas fa-calendar-check"></i> Book Now
          </a>
        ` : ''}
        ${property.whatsappLink ? `
          <a href="${property.whatsappLink}" class="whatsapp-button" target="_blank">
            <i class="fab fa-whatsapp"></i> WhatsApp
          </a>
        ` : ''}
      </div>
    `;

    return `
      <div class="property-item" onclick="togglePropertyInfo(this)">
        <div class="property-header">
          <div class="property-title">${property.title}</div>
          <div class="property-price">${property.price}</div>
        </div>
        <div class="property-details">
          <div class="property-location">📍 ${property.location}</div>
          <div class="property-area">🏠 ${property.area}</div>
          <div class="property-bedrooms">🛏️ ${property.bedrooms} BHK</div>
        </div>
        ${carousel}
        <div class="property-description">${property.description}</div>
        <div class="property-contact">📞 ${property.contact}</div>
        ${actionButtons} <!-- Add the action buttons here -->
        ${propertyInfo}
      </div>
    `;
  }

  createImageCarousel(images) {
    return `
      <div class="carousel">
        ${images.map((image, index) => `
          <div class="carousel-item ${index === 0 ? 'active' : ''}">
            <img src="${image}" alt="Property Image ${index + 1}">
          </div>
        `).join('')}
        <button class="carousel-control prev" onclick="changeSlide(this.parentElement, -1)">&#10094;</button>
        <button class="carousel-control next" onclick="changeSlide(this.parentElement, 1)">&#10095;</button>
      </div>
    `;
  }

  processJobQuery(query) {
  const normalizedQuery = query.toLowerCase();

  // Job-related keywords and synonyms
  const jobKeywords = ['job', 'jobs', 'vacancy', 'vacancies', 'career', 'hiring', 'opening', 'work', 'employment'];
  const jobTypes = ['full-time', 'part-time', 'contract', 'internship', 'government', 'freelance'];
  const salaryKeywords = ['salary', 'pay', 'income', 'compensation', 'under', 'above', 'less than', 'more than', 'between'];
  const experienceKeywords = ['experience', 'exp', 'years', 'fresher', 'freshers', 'entry-level'];

  // Check if it's a job-related query
  const isJobQuery = jobKeywords.some(keyword => normalizedQuery.includes(keyword));
  if (!isJobQuery) return null;

  // Extract job search context
  const context = {
    company: this.extractCompany(normalizedQuery),
    location: this.extractLocation(normalizedQuery),
    salary: this.extractSalaryRange(normalizedQuery),
    experience: this.extractExperienceRange(normalizedQuery),
    type: this.extractJobType(normalizedQuery, jobTypes),
    role: this.extractJobRole(normalizedQuery)
  };

  // Get relevant jobs
  const relevantJobs = this.getRelevantJobs(context);

  // Generate response
  return this.generateJobResponse(relevantJobs, context);
}

// Helper methods for job query processing
extractCompany(query) {
  const companies = ['google', 'tech corp', 'data insights', 'national bank', 'madhya pradesh police', 'infosys', 'tcs', 'wipro'];
  return companies.find(company => query.includes(company)) || null;
}

extractLocation(query) {
  const locations = ['bangalore', 'bhopal', 'hyderabad', 'mumbai', 'madhya pradesh', 'delhi', 'chennai', 'pune', 'kolkata'];
  return locations.find(location => query.includes(location)) || null;
}

extractSalaryRange(query) {
  // Handle "between X and Y" salary ranges
  const rangeMatch = query.match(/(between)\s*₹?\s*(\d+,\d+|\d+)\s*and\s*₹?\s*(\d+,\d+|\d+)/i);
  if (rangeMatch) {
    const minAmount = parseFloat(rangeMatch[2].replace(/,/g, ''));
    const maxAmount = parseFloat(rangeMatch[3].replace(/,/g, ''));
    return { min: minAmount, max: maxAmount };
  }

  // Handle "under/above X" salary ranges
  const salaryMatch = query.match(/(under|less than|below|above|more than)\s*₹?\s*(\d+,\d+|\d+)/i);
  if (salaryMatch) {
    const operator = salaryMatch[1].toLowerCase();
    const amount = parseFloat(salaryMatch[2].replace(/,/g, ''));
    return { operator, amount };
  }

  return null;
}

extractExperienceRange(query) {
  // Handle "X to Y years" experience ranges
  const rangeMatch = query.match(/(\d+)\s*(to|-)\s*(\d+)\s*years/i);
  if (rangeMatch) {
    return {
      min: parseInt(rangeMatch[1]),
      max: parseInt(rangeMatch[3])
    };
  }

  // Handle "X years" experience
  const singleMatch = query.match(/(\d+)\s*years/i);
  if (singleMatch) {
    return {
      min: parseInt(singleMatch[1]),
      max: null
    };
  }

  return null;
}

extractJobType(query, jobTypes) {
  return jobTypes.find(type => query.includes(type)) || null;
}

extractJobRole(query) {
  const roles = ['teacher', 'engineer', 'developer', 'manager', 'analyst', 'designer', 'doctor', 'nurse', 'police'];
  return roles.find(role => query.includes(role)) || null;
}

getRelevantJobs(context) {
  let jobs = searchConfig.jobsDatabase.jobs;

  // Filter by company
  if (context.company) {
    jobs = jobs.filter(job => job.company.toLowerCase().includes(context.company));
  }

  // Filter by location
  if (context.location) {
    jobs = jobs.filter(job => job.location.toLowerCase().includes(context.location));
  }

  // Filter by salary range
  if (context.salary) {
    if (context.salary.min && context.salary.max) {
      // Handle "between X and Y" salary ranges
      jobs = jobs.filter(job => {
        const jobSalary = parseFloat(job.salary.replace(/[^0-9]/g, ''));
        return jobSalary >= context.salary.min && jobSalary <= context.salary.max;
      });
    } else if (context.salary.operator && context.salary.amount) {
      // Handle "under/above X" salary ranges
      const { operator, amount } = context.salary;
      jobs = jobs.filter(job => {
        const jobSalary = parseFloat(job.salary.replace(/[^0-9]/g, ''));
        if (operator === 'under' || operator === 'less than' || operator === 'below') {
          return jobSalary <= amount;
        } else if (operator === 'above' || operator === 'more than') {
          return jobSalary >= amount;
        }
        return true;
      });
    }
  }

  // Filter by experience range
  if (context.experience) {
    jobs = jobs.filter(job => {
      const experienceMatch = job.experience.match(/(\d+)\s*-\s*(\d+)\s*years/i);
      if (experienceMatch) {
        const minExp = parseInt(experienceMatch[1]);
        const maxExp = parseInt(experienceMatch[2]);
        return context.experience.min >= minExp && (!context.experience.max || context.experience.max <= maxExp);
      }
      return false;
    });
  }

  // Filter by job type
  if (context.type) {
    jobs = jobs.filter(job => job.type.toLowerCase() === context.type);
  }

  // Filter by role
  if (context.role) {
    jobs = jobs.filter(job => job.title.toLowerCase().includes(context.role));
  }

  return jobs.slice(0, 5); // Return top 5 results
}

generateJobResponse(jobs, context) {
  if (jobs.length === 0) {
    return "Sorry, I couldn't find any jobs matching your request. Please try a different query.";
  }

  const header = this.generateJobHeader(context);
  const jobList = jobs.map(job => this.formatJob(job)).join('');

  return `
    <div class="jobs-container">
      ${header}
      <div class="jobs-list">
        ${jobList}
      </div>
    </div>
  `;
}

generateJobHeader(context) {
    let headerText = 'Here are some jobs';

    if (context.location) {
        headerText += ` in ${context.location}`;
    }

    if (context.salary) {
        if (context.salary.min && context.salary.max) {
            headerText += ` with salary between ₹${context.salary.min.toLocaleString()} and ₹${context.salary.max.toLocaleString()}`;
        } else if (context.salary.operator && context.salary.amount) {
            headerText += ` with salary ${context.salary.operator} ₹${context.salary.amount.toLocaleString()}`;
        }
    }

    if (context.company) {
        headerText += ` at ${context.company}`;
    }

    if (context.role) {
        headerText += ` for ${context.role} roles`;
    }

    if (context.experience) {
        if (context.experience.min && context.experience.max) {
            headerText += ` with ${context.experience.min} to ${context.experience.max} years of experience`;
        } else if (context.experience.min) {
            headerText += ` with at least ${context.experience.min} years of experience`;
        }
    }

    return `<div class="job-header-text"><h2>${headerText}</h2></div>`;
}

formatJob(job) {
  const jobInfo = job.jobInfo ? `
    <div class="job-info hidden">
      ${Object.entries(job.jobInfo).map(([section, items]) => `
        <div class="info-section">
          <h3>${section}</h3>
          <ul>
            ${items.map(item => `<li>${item}</li>`).join('')}
          </ul>
        </div>
      `).join('')}
    </div>
  ` : '';

  return `
    <div class="job-item" onclick="toggleJobInfo(this)">
      <div class="job-header">
        <div class="job-title">${job.title}</div>
        <div class="job-company">${job.company}</div>
      </div>
      <div class="job-details">
        <div class="job-location">📍 ${job.location}</div>
        <div class="job-salary">💰 ${job.salary}</div>
        <div class="job-experience">📅 ${job.experience}</div>
      </div>
      <div class="job-description">${job.description}</div>
      <a href="${job.applyLink}" class="apply-button" target="_blank">Apply Now</a>
      ${jobInfo}
    </div>
  `;
}

isCouponRelatedQuery(query) {
    // Check for coupon-related keywords
    const couponKeywords = searchConfig.couponDatabase.keywords;
    const hasCouponKeyword = couponKeywords.some(keyword => query.includes(keyword));

    // Check for brand names
    const brands = searchConfig.couponDatabase.brands;
    const hasBrandName = brands.some(brand => query.includes(brand.toLowerCase()));

    return hasCouponKeyword || hasBrandName;
  }

  processPromptQuery(query) {
    // Normalize the query for better matching
    const normalizedQuery = query.toLowerCase().trim();

    // Check for exact match in prompt database
    if (searchConfig.promptDatabase[normalizedQuery]) {
      return this.createPromptResponse(searchConfig.promptDatabase[normalizedQuery]);
    }

    // Check for partial matches or similar queries
    let bestMatch = null;
    let highestSimilarity = 0;

    for (const [prompt, data] of Object.entries(searchConfig.promptDatabase)) {
      const normalizedPrompt = prompt.toLowerCase().trim();

      // Check if the query contains the prompt or vice versa
      if (
        normalizedQuery.includes(normalizedPrompt) ||
        normalizedPrompt.includes(normalizedQuery)
      ) {
        return this.createPromptResponse(data);
      }

      // Check for similarity using a similarity threshold
      const similarity = this.calculateSimilarity(normalizedQuery, normalizedPrompt);
      if (similarity > highestSimilarity && similarity > this.similarityThreshold) {
        highestSimilarity = similarity;
        bestMatch = data;
      }
    }

    // Return the best match if found
    if (bestMatch) {
      return this.createPromptResponse(bestMatch);
    }

    return null;
  }

  createPromptResponse(promptData) {
  const elaborateInfo = promptData.elaborateInfo ? `
    <div class="elaborate-info hidden">
      ${Object.entries(promptData.elaborateInfo).map(([section, items]) => `
        <div class="elaborate-section">
          <h3>${section}</h3>
          <ul>
            ${items.map(item => `<li>${item}</li>`).join('')}
          </ul>
        </div>
      `).join('')}
    </div>
  ` : '';

  return `
    <div class="prompt-response" onclick="toggleElaborateInfo(this)">
      <div class="answer">${promptData.answer}</div>
      <div class="click-buttons">
        ${promptData.clickButtons.map(btn => `
          <button class="click-button" data-url="${btn.url}">
            <i class="fas ${btn.icon}"></i> ${btn.label}
          </button>
        `).join('')}
      </div>
      ${elaborateInfo}
    </div>
  `;
}

  processCouponQuery(query) {
    // Extract intent and context
    const intent = this.detectCouponIntent(query);
    const context = this.extractCouponContext(query);

    // Get relevant coupons
    const relevantCoupons = this.getRelevantCoupons(intent, context);

    if (relevantCoupons.length > 0) {
      return this.generateCouponResponse(relevantCoupons, intent, context);
    } else {
      return "Sorry, I couldn't find any coupons matching your request. Please try a different query.";
    }
  }

  detectCouponIntent(query) {
    const intents = {
      'fashion': ['fashion', 'clothing', 'apparel', 'dress', 'shoes', 'accessories'],
      'food': ['food', 'restaurant', 'meal', 'dining', 'eat', 'delivery'],
      'electronics': ['electronics', 'gadgets', 'phones', 'laptops', 'tv', 'appliances'],
      'shopping': ['shopping', 'deals', 'offers', 'discounts', 'sale'],
      'travel': ['travel', 'flight', 'hotel', 'booking', 'vacation'],
      'entertainment': ['movie', 'streaming', 'music', 'concert', 'tickets']
    };

    for (const [intent, keywords] of Object.entries(intents)) {
      if (keywords.some(keyword => query.includes(keyword))) {
        return intent;
      }
    }

    return 'general';
  }

  extractCouponContext(query) {
    const context = {
      timeFrame: this.extractTimeFrame(query),
      specificity: this.determineSpecificity(query),
      brand: this.extractBrand(query),
      category: this.extractCategory(query),
      modifiers: this.extractModifiers(query)
    };

    return context;
  }

  extractTimeFrame(query) {
    if (query.includes('today')) return 'today';
    if (query.includes('this week')) return 'week';
    if (query.includes('this month')) return 'month';
    return null;
  }

  determineSpecificity(query) {
    if (query.includes('best') || query.includes('top')) return 'best';
    if (query.includes('new') || query.includes('latest')) return 'new';
    return 'general';
  }

  extractBrand(query) {
    const brands = searchConfig.couponDatabase.brands;
    return brands.find(brand => query.includes(brand.toLowerCase())) || null;
  }

  extractCategory(query) {
    const categories = ['fashion', 'food', 'electronics', 'travel', 'entertainment'];
    return categories.find(category => query.includes(category)) || null;
  }

  extractModifiers(query) {
    const modifiers = [];
    if (query.includes('free shipping')) modifiers.push('free_shipping');
    if (query.includes('cashback')) modifiers.push('cashback');
    if (query.includes('first order')) modifiers.push('first_order');
    return modifiers;
  }

  getRelevantCoupons(intent, context) {
    let coupons = [];
    
    // Get all coupons from the database
    for (const brand of searchConfig.couponDatabase.brands) {
      coupons = coupons.concat(searchConfig.couponDatabase.items[brand] || []);
    }

    // Filter by intent and context
    coupons = coupons.filter(coupon => {
      // Filter by category
      if (intent !== 'general' && !coupon.category.toLowerCase().includes(intent)) {
        return false;
      }

      // Filter by brand if specified
      if (context.brand && coupon.brand.toLowerCase() !== context.brand.toLowerCase()) {
        return false;
      }

      // Filter by time frame
      if (context.timeFrame === 'today' && !this.isCouponValidToday(coupon)) {
        return false;
      }

      // Filter by modifiers
      if (context.modifiers.length > 0) {
        const couponModifiers = coupon.eligibility?.Modifiers || [];
        if (!context.modifiers.every(mod => couponModifiers.includes(mod))) {
          return false;
        }
      }

      return true;
    });

    // Sort coupons based on specificity
    if (context.specificity === 'best') {
      coupons.sort((a, b) => {
        const discountA = parseFloat(a.discount);
        const discountB = parseFloat(b.discount);
        return discountB - discountA;
      });
    } else if (context.specificity === 'new') {
      coupons.sort((a, b) => new Date(b.validity) - new Date(a.validity));
    }

    return coupons.slice(0, 5); // Return top 5 results
  }

  isCouponValidToday(coupon) {
    const today = new Date();
    const validityDate = new Date(coupon.validity);
    return validityDate >= today;
  }

  generateCouponResponse(coupons, intent, context) {
    if (coupons.length === 0) {
      return "Sorry, I couldn't find any coupons matching your request.";
    }

    const header = this.generateCouponHeader(intent, context);
    const couponList = coupons.map(coupon => this.formatCoupon(coupon)).join('');

    return `
      <div class="coupons-container">
        ${header}
        <div class="coupons-list">
          ${couponList}
        </div>
      </div>
    `;
  }

  generateCouponHeader(intent, context) {
    let headerText = 'Here are some coupons';
    
    if (context.specificity === 'best') {
        headerText = 'Here are the best coupons';
    } else if (context.specificity === 'new') {
        headerText = 'Here are the latest coupons';
    }

    if (context.brand) {
        headerText += ` for ${context.brand}`;
    } else if (intent !== 'general') {
        headerText += ` for ${intent}`;
    }

    if (context.timeFrame) {
        headerText += ` valid ${context.timeFrame}`;
    }

    return `<div class="coupon-header-text"><h2>${headerText}</h2></div>`;
}

  formatCoupon(coupon) {
    return `
      <div class="coupon-item" onclick="toggleCouponInfo(this)">
        <div class="coupon-header">
          <div class="brand-info">
            <i class="fas ${coupon.icon}"></i>
            <span>${coupon.brand}</span>
          </div>
          <div class="discount-tag">${coupon.discount}</div>
        </div>
        <div class="coupon-content">
          <p class="description">${coupon.description}</p>
          <div class="code-container">
            <code class="coupon-code">${coupon.code}</code>
            <button class="copy-button" onclick="event.stopPropagation(); copyCouponCode('${coupon.code}')">
              <i class="fas fa-copy"></i>
            </button>
          </div>
          <div class="validity">Valid till: ${coupon.validity}</div>
        </div>
        <div class="coupon-info hidden">
          ${Object.entries(coupon.eligibility).map(([section, items]) => `
            <div class="info-section">
              <h4>${section}</h4>
              ${Array.isArray(items) 
                ? `<ul>${items.map(item => `<li>${item}</li>`).join('')}</ul>`
                : `<p>${items}</p>`
              }
            </div>
          `).join('')}
        </div>
      </div>
    `;
  };

  searchDatabase(query) {
    const normalizedQuery = query.toLowerCase();
    const words = normalizedQuery.split(/\s+/);
    const results = [];

    for (const [question, data] of Object.entries(searchConfig.qaDatabase)) {
      if (normalizedQuery === question.toLowerCase()) {
        results.push({ similarity: 1, data });
        continue;
      }

      const keywordMatch = data.keywords?.some(keyword => 
        words.includes(keyword.toLowerCase())
      );
      if (keywordMatch) {
        results.push({ similarity: 0.8, data });
        continue;
      }

      const similarity = this.calculateSimilarity(normalizedQuery, question.toLowerCase());
      if (similarity > this.similarityThreshold) {
        results.push({ similarity, data });
      }
    }

    if (results.length > 0) {
      results.sort((a, b) => b.similarity - a.similarity);
      return results[0].data.answer;
    }

    return this.getRandomFallbackResponse();
  }

  calculateSimilarity(str1, str2) {
  const words1 = str1.split(/\s+/);
  const words2 = str2.split(/\s+/);
  const set1 = new Set(words1);
  const set2 = new Set(words2);
  const intersection = new Set([...set1].filter(x => set2.has(x)));
  const union = new Set([...set1, ...set2]);
  const jaccardSimilarity = intersection.size / union.size;

  let orderSimilarity = 0;
  words1.forEach((word, index) => {
    const pos2 = words2.indexOf(word);
    if (pos2 !== -1) {
      orderSimilarity += 1 - Math.abs(index - pos2) / Math.max(words1.length, words2.length);
    }
  });

  return (jaccardSimilarity * 0.7) + (orderSimilarity / words1.length * 0.3);
}

  getRandomFallbackResponse() {
    const responses = searchConfig.fallbackResponses;
    return responses[Math.floor(Math.random() * responses.length)];
  }

  displayMessage(content, type) {
    const messageDiv = document.createElement('div');
    messageDiv.className = `message ${type}-message`;

    // Add long-press event listeners
    let pressTimer;
    messageDiv.addEventListener('mousedown', () => {
      pressTimer = setTimeout(() => {
        this.showActionIcons(messageDiv, content);
      }, 1000); // 1 second long press
    });

    messageDiv.addEventListener('mouseup', () => {
      clearTimeout(pressTimer);
    });

    messageDiv.addEventListener('mouseleave', () => {
      clearTimeout(pressTimer);
    });

    // Touch events for mobile devices
    messageDiv.addEventListener('touchstart', () => {
      pressTimer = setTimeout(() => {
        this.showActionIcons(messageDiv, content);
      }, 1000); // 1 second long press
    });

    messageDiv.addEventListener('touchend', () => {
      clearTimeout(pressTimer);
    });

    messageDiv.addEventListener('touchmove', () => {
      clearTimeout(pressTimer);
    });

    if (type === 'bot') {
      messageDiv.innerHTML = `
        <div class="message-content">${content}</div>`;
    } else {
      messageDiv.innerHTML = `
        <div class="message-content">${content}</div>`;
    }

    this.chatMessages.appendChild(messageDiv);
  }

  showActionIcons(container, content) {
    // Remove any existing action icons
    const existingIcons = container.querySelector('.action-icons');
    if (existingIcons) existingIcons.remove();

    // Create action icons container
    const actionIcons = document.createElement('div');
    actionIcons.className = 'action-icons';

    // Copy icon
    const copyIcon = document.createElement('div');
    copyIcon.className = 'action-icon copy-icon';
    copyIcon.innerHTML = `<i class="fas fa-copy"></i><span>Copy</span>`;
    copyIcon.addEventListener('click', (e) => {
      e.stopPropagation();
      this.copyMessage(container);
      this.hideActionIcons(actionIcons); // Hide icons after action
    });

    // Share icon
    const shareIcon = document.createElement('div');
    shareIcon.className = 'action-icon share-icon';
    shareIcon.innerHTML = `<i class="fas fa-share-alt"></i><span>Share</span>`;
    shareIcon.addEventListener('click', (e) => {
      e.stopPropagation();
      this.shareMessage(container);
      this.hideActionIcons(actionIcons); // Hide icons after action
    });

    // Append icons to container
    actionIcons.appendChild(copyIcon);
    actionIcons.appendChild(shareIcon);

    // Append container to the target element
    container.appendChild(actionIcons);

    // Hide icons if no action is taken after a short delay
    setTimeout(() => {
      if (actionIcons.parentElement) {
        this.hideActionIcons(actionIcons);
      }
    }, 3000); // Hide after 3 seconds of inactivity
  }

  hideActionIcons(actionIcons) {
    actionIcons.classList.add('fade-out');
    setTimeout(() => {
      if (actionIcons.parentElement) {
        actionIcons.parentElement.removeChild(actionIcons);
      }
    }, 200); // Match the fade-out animation duration
  }

  copyMessage(container) {
    let textToCopy = '';

    // Handle different types of content
    if (container.classList.contains('job-item')) {
      textToCopy = this.getJobText(container);
    } else if (container.classList.contains('coupon-item')) {
      textToCopy = this.getCouponText(container);
    } else if (container.classList.contains('property-item')) {
      textToCopy = this.getPropertyText(container);
    } else {
      textToCopy = container.innerText; // Default to copying all text
    }

    navigator.clipboard.writeText(textToCopy).then(() => {
      this.showToast('Copied to clipboard!');
    }).catch(err => {
      this.showToast('Failed to copy');
    });
  }

  shareMessage(container) {
    let textToShare = '';

    // Handle different types of content
    if (container.classList.contains('job-item')) {
      textToShare = this.getJobText(container);
    } else if (container.classList.contains('coupon-item')) {
      textToShare = this.getCouponText(container);
    } else if (container.classList.contains('property-item')) {
      textToShare = this.getPropertyText(container);
    } else {
      textToShare = container.innerText; // Default to sharing all text
    }

    if (navigator.share) {
      navigator.share({
        title: 'Check this out!',
        text: textToShare,
        url: window.location.href,
      })
        .then(() => {
          this.showToast('Shared successfully!');
        })
        .catch((err) => {
          this.showToast('Failed to share');
        });
    } else {
      // Fallback for devices that don't support the Web Share API
      const shareUrl = `mailto:?subject=Check this out!&body=${encodeURIComponent(textToShare)}`;
      window.location.href = shareUrl;
    }
  }

  getJobText(jobItem) {
    const title = jobItem.querySelector('.job-title')?.innerText || '';
    const company = jobItem.querySelector('.job-company')?.innerText || '';
    const location = jobItem.querySelector('.job-location')?.innerText || '';
    const salary = jobItem.querySelector('.job-salary')?.innerText || '';
    const description = jobItem.querySelector('.job-description')?.innerText || '';
    return `${title}\n${company}\n${location}\n${salary}\n\n${description}`;
  }

  getCouponText(couponItem) {
    const brand = couponItem.querySelector('.brand-info span')?.innerText || '';
    const discount = couponItem.querySelector('.discount-tag')?.innerText || '';
    const code = couponItem.querySelector('.coupon-code')?.innerText || '';
    const validity = couponItem.querySelector('.validity')?.innerText || '';
    return `${brand}\n${discount}\nCode: ${code}\n${validity}`;
  }

  getPropertyText(propertyItem) {
    const title = propertyItem.querySelector('.property-title')?.innerText || '';
    const price = propertyItem.querySelector('.property-price')?.innerText || '';
    const location = propertyItem.querySelector('.property-location')?.innerText || '';
    const description = propertyItem.querySelector('.property-description')?.innerText || '';
    return `${title}\n${price}\n${location}\n\n${description}`;
  }

  async simulateProcessing() {
    const typingIndicator = document.createElement('div');
    typingIndicator.className = 'message bot-message typing-indicator';
    typingIndicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
    this.chatMessages.appendChild(typingIndicator);
    this.scrollToBottom();
    await new Promise(resolve => setTimeout(resolve, 1000));
    typingIndicator.remove();
  }

  scrollToBottom() {
    this.chatMessages.scrollTop = this.chatMessages.scrollHeight;
  }
}

function createDoctorSearchModal() {
  // Create modal container
  const modal = document.createElement('div');
  modal.id = 'findDoctorsModal';
  modal.className = 'modal';

  // Modal inner content
  modal.innerHTML = `
    <div class="modal-content">
      <span class="close">&times;</span>
      <h2>Find Doctors</h2>
      <form id="findDoctorsForm">
        <div class="form-group">
          <label for="city">City</label>
          <input type="text" id="city" placeholder="Enter your city" required>
        </div>
        <div class="form-group">
          <label for="specialty">Specialty</label>
          <select id="specialty" required>
            <option value="Dentist">Dentist</option>
            <option value="Cardiologist">Cardiologist</option>
            <option value="Dermatologist">Dermatologist</option>
            <option value="Gynecologist">Gynecologist</option>
            <option value="Pediatrician">Pediatrician</option>
            <option value="Orthopedist">Orthopedist</option>
            <option value="General Physician">General Physician</option>
          </select>
        </div>
        <button type="submit" class="search-button">Search</button>
      </form>
    </div>
  `;

  // Append to body
  document.body.appendChild(modal);

  // Event listener to close modal
  modal.querySelector('.close').onclick = function () {
    modal.style.display = 'none';
  };

  // Optional: Close modal when clicking outside content
  window.onclick = function (event) {
    if (event.target === modal) {
      modal.style.display = 'none';
    }
  };

  // Optional: Form submission logic
  document.getElementById('findDoctorsForm').onsubmit = function (e) {
    e.preventDefault();
    const city = document.getElementById('city').value;
    const specialty = document.getElementById('specialty').value;
    console.log(`Searching for ${specialty} in ${city}`);
    // Add your search logic here
  };
}

// Call this function to create and display the modal
createDoctorSearchModal();

// Open Modal
function openFindDoctorsModal() {
  const modal = document.getElementById('findDoctorsModal');
  modal.style.display = 'flex';
}

// Close Modal
function closeFindDoctorsModal() {
  const modal = document.getElementById('findDoctorsModal');
  modal.style.display = 'none';
}

// Handle Form Submission
document.getElementById('findDoctorsForm').addEventListener('submit', function (e) {
  e.preventDefault();

  const city = document.getElementById('city').value.trim();
  const specialty = document.getElementById('specialty').value.trim();

  if (!city || !specialty) {
    alert('Please fill in all fields.');
    return;
  }

  // Generate Practo Search Link
  const practoLink = generatePractoLink(city, specialty);
  window.location.href = practoLink; // Redirect to Practo
});

// Generate Practo Link
function generatePractoLink(city, specialty) {
  const baseUrl = 'https://www.practo.com/search/doctors?results_type=doctor';
  const query = encodeURIComponent(
    JSON.stringify([{ word: specialty, autocompleted: true, category: 'subspeciality' }])
  );
  return `${baseUrl}&q=${query}&city=${encodeURIComponent(city)}`;
}

// Close Modal on Click Outside
window.addEventListener('click', function (e) {
  const modal = document.getElementById('findDoctorsModal');
  if (e.target === modal) {
    closeFindDoctorsModal();
  }
});

// Close Modal on Close Button Click
document.querySelector('.close').addEventListener('click', closeFindDoctorsModal);

// Menu System Implementation
function setupMenu() {
  const menuButton = document.getElementById('menuButton');
  const menuOverlay = document.getElementById('menuOverlay');
  const menuItems = document.querySelectorAll('.menu-item');
  const chatMessages = document.getElementById('chatMessages');
  
  const closeButton = document.createElement('button');
  closeButton.innerHTML = '<i class="fas fa-times"></i>';
  closeButton.className = 'menu-close-button';
  menuOverlay.appendChild(closeButton);

  function toggleMenu(show) {
      menuOverlay.style.display = show ? 'block' : 'none';
      menuOverlay.classList.toggle('fade-in', show);
      document.body.style.overflow = show ? 'hidden' : '';
  }

  menuButton.addEventListener('click', () => toggleMenu(true));
  closeButton.addEventListener('click', () => toggleMenu(false));
  menuOverlay.addEventListener('click', (e) => {
      if (e.target === menuOverlay) toggleMenu(false);
  });

  menuItems.forEach(item => {
      item.addEventListener('click', () => {
          const selectedItem = item.textContent.trim();
          const message = document.createElement('div');
          message.className = 'message bot-message';
          message.innerHTML = `
              <div class="message-content">You selected: ${selectedItem}. How can I assist you further?</div>
          `;
          chatMessages.appendChild(message);
          chatMessages.scrollTop = chatMessages.scrollHeight;
          toggleMenu(false);
      });
  });

  document.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') toggleMenu(false);
  });
}

        // Voice Recognition Class
        class VoiceRecognition {
            constructor() {
                this.recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
                this.recognition.continuous = false;
                this.recognition.interimResults = false;
                this.recognition.lang = 'en-US';

                this.recognition.onresult = (event) => {
                    const transcript = event.results[0][0].transcript;
                    this.handleVoiceInput(transcript);
                };

                this.recognition.onerror = (event) => {
                    console.error('Speech recognition error:', event.error);
                    this.showToast('Error: ' + event.error);
                };

                this.recognition.onend = () => {
                    this.recognition.stop();
                };
            }

            start() {
                this.recognition.start();
                this.showToast('Listening...');
            }

            handleVoiceInput(transcript) {
                const chatInput = document.getElementById('chatInput');
                chatInput.value = transcript;
                chatSystem.handleUserInput();
            }

            showToast(message) {
                const toast = document.createElement('div');
                toast.className = 'toast';
                toast.textContent = message;
                document.body.appendChild(toast);

                setTimeout(() => {
                    toast.classList.add('show');
                    setTimeout(() => {
                        toast.classList.remove('show');
                        setTimeout(() => toast.remove(), 300);
                    }, 2000);
                }, 100);
            }
        }

        // Initialize Voice Recognition
        const voiceRecognition = new VoiceRecognition();

        // Add Event Listener to Voice Button
        document.getElementById('voiceButton').addEventListener('click', () => {
            voiceRecognition.start();
        });
     
// Utility function to toggle detailed information
function toggleDetailedInfo(element) {
  const infoPanel = element.closest('.list-item').querySelector('.detailed-info');
  infoPanel.classList.toggle('hidden');
}

function toggleVehicleInfo(element) {
  const infoPanel = element.querySelector('.vehicle-info');
  infoPanel.classList.toggle('hidden');
}

function toggleProviderInfo(element) {
  const infoPanel = element.querySelector('.provider-info');
  infoPanel.classList.toggle('hidden');
}

function toggleFranchiseInfo(element) {
  const infoPanel = element.querySelector('.franchise-info');
  infoPanel.classList.toggle('hidden');
}

function toggleMovieInfo(element) {
  const infoPanel = element.querySelector('.movie-info');
  infoPanel.classList.toggle('hidden');
}

function toggleElaborateInfo(promptResponse) {
    const infoPanel = promptResponse.querySelector('.elaborate-info');
    infoPanel.classList.toggle('hidden');
}

function changeSlide(carousel, direction) {
  const items = carousel.querySelectorAll('.carousel-item');
  let activeIndex = Array.from(items).findIndex(item => item.classList.contains('active'));
  items[activeIndex].classList.remove('active');
  activeIndex = (activeIndex + direction + items.length) % items.length;
  items[activeIndex].classList.add('active');
}

function changeAutomotiveSlide(carousel, direction) {
  const items = carousel.querySelectorAll('.automotive-carousel-item');
  let activeIndex = Array.from(items).findIndex(item => item.classList.contains('active'));
  items[activeIndex].classList.remove('active');
  activeIndex = (activeIndex + direction + items.length) % items.length;
  items[activeIndex].classList.add('active');
}

function togglePropertyInfo(element) {
  const infoPanel = element.querySelector('.property-info');
  infoPanel.classList.toggle('hidden');
}

function toggleLoanInfo(element) {
    const infoPanel = element.querySelector('.loan-info');
    infoPanel.classList.toggle('hidden');
}

// Utility functions
function toggleCouponInfo(element) {
  const infoPanel = element.querySelector('.coupon-info');
  infoPanel.classList.toggle('hidden');
}

function toggleJobInfo(element) {
  const infoPanel = element.querySelector('.job-info');
  infoPanel.classList.toggle('hidden');
}

function toggleCourseInfo(element) {
  const infoPanel = element.querySelector('.course-info');
  infoPanel.classList.toggle('hidden');
}

function toggleDiseaseInfo(element) {
  const infoPanel = element.querySelector('.disease-info');
  infoPanel.classList.toggle('hidden');
}

function copyCouponCode(code) {
  navigator.clipboard.writeText(code).then(() => {
    showToast('Coupon code copied!');
  }).catch(err => {
    showToast('Failed to copy code');
  });
}

function showToast(message) {
  const toast = document.createElement('div');
  toast.className = 'toast';
  toast.textContent = message;
  document.body.appendChild(toast);
  
  setTimeout(() => {
    toast.classList.add('show');
    setTimeout(() => {
      toast.classList.remove('show');
      setTimeout(() => toast.remove(), 300);
    }, 2000);
  }, 100);
}

document.addEventListener('DOMContentLoaded', function() {
  const chatSystem = new ChatSystem();
  chatSystem.initialize();
  setupMenu();

  if (typeof flatpickr !== 'undefined') {
      flatpickr("#dateInput", {
          minDate: "today",
          maxDate: new Date().fp_incr(30),
          dateFormat: "Y-m-d"
      });

      flatpickr("#timeInput", {
          enableTime: true,
          noCalendar: true,
          dateFormat: "H:i",
          minTime: "09:00",
          maxTime: "18:00",
          minuteIncrement: 30
      });
  }
});

const style = document.createElement('style');
style.textContent = `
  .typing-indicator {
      padding: 20px;
  }

  .typing-indicator .dots {
      display: inline-flex;
      gap: 4px;
  }

  .typing-indicator .dots span {
      width: 8px;
      height: 8px;
      background: #666;
      border-radius: 50%;
      animation: bounce 1.4s infinite ease-in-out;
  }

  .typing-indicator .dots span:nth-child(1) { animation-delay: -0.32s; }
  .typing-indicator .dots span:nth-child(2) { animation-delay: -0.16s; }

  @keyframes bounce {
      0%, 80%, 100% { transform: scale(0); }
      40% { transform: scale(1); }
  }

    .message {
    display: flex; 
    gap: 10px; 
    max-width: 100%; 
    margin-bottom: 16px; 
    animation: messageSlide 0.3s ease-out; 
    }

    .bot-message { 
  align-self: flex-start; 
}
   
   .user-message {
        background: #lalala;
        color: white;
        margin-left: auto;
    }

    .message-avatar {
        width: 30px;
        height: 30px;
        background: #lalala;
        color: #FFD700;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .welcome-message ul {
        margin: 10px 0;
        padding-left:20px;
    }

    .fade-out {
  animation: fadeOut 0.3s ease-out forwards;
    }

   @keyframes fadeOut {
   from { opacity: 1; }
   to { opacity: 0; transform: translateY(-20px); }
   }

    .menu-close-button {
        position: absolute;
        top: 15px;
        right: 15px;
        background: #1a1a1f;
        border: none;
        color: #FFD700;
        font-size: 24px;
        cursor: pointer;
        padding: 5px;
        z-index: 1001;
    }

    .menu-close-button:hover {
        color: #FFD700;
    }

/* Action Icons Container */
.action-icons {
  display: flex;
  gap: 12px;
  position: absolute;
  right: 16px;
  top: 16px;
  background: rgba(255, 255, 255, 0.9);
  padding: 8px 12px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  z-index: 10;
  animation: fadeIn 0.2s ease;
}

.action-icons.fade-out {
  animation: fadeOut 0.2s ease forwards;
}

/* Action Icon */
.action-icon {
  display: flex;
  align-items: center;
  gap: 6px;
  cursor: pointer;
  padding: 6px 10px;
  border-radius: 6px;
  background: rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease;
}

.action-icon:hover {
  background: rgba(0, 0, 0, 0.1);
}

.action-icon i {
  font-size: 14px;
  color: #333;
}

.action-icon span {
  font-size: 12px;
  font-weight: 500;
  color: #333;
}

/* Share Icon Specific Styles */
.share-icon i {
  color: #2563eb; /* Blue color for share icon */
}

/* Animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(-10px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeOut {
  from { opacity: 1; transform: translateY(0); }
  to { opacity: 0; transform: translateY(-10px); }
}

/* Toast Messages */
.toast {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  background: rgba(0, 0, 0, 0.8);
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  transition: transform 0.3s ease;
  z-index: 1000;
}

.toast.show {
  transform: translateX(-50%) translateY(0);
}

.prompt-response {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
  width: 100%; /* Full width */
  margin-left: 0; /* Remove auto margin to align to the edges */
  margin-right: 0; /* Remove auto margin to align to the edges */
}

    .click-buttons {
        display: flex;
        align-items: center;
        gap: 8px;
        margin-top: 10px;
        height: 40px;
        pointer-events: none; /* Prevent action buttons from triggering detailed info */
    }

    .click-button {
        pointer-events: auto; /* Re-enable clicks for action buttons */
        padding: 8px 16px;
        background: linear-gradient(to bottom, #2563eb, #1d4ed8);
        color: white;
        border: none;
        border-radius: 6px;
        cursor: pointer;
        display: flex;
        align-items: center;
        gap: 8px;
        font-family: Poppins;
        font-weight: 500;
        transition: all 0.2s ease;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        height: 40px;
        line-height: 40px;
    }

    .click-button:hover {
        background: linear-gradient(to bottom, #1d4ed8, #1e40af);
        transform: translateY(-1px);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.15);
    }

    .elaborate-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}
    .hidden {
        display: none;
    }

    .elaborate-section {
        margin-bottom: 15px;
    }

    .elaborate-section h3 {
  font-size: 18px; /* Larger font size for section headings on desktop */
  margin-bottom: 8px;
  color: #FFD700;
}

    .elaborate-section ul {
        margin: 0;
        padding-left: 20px;
    }

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .prompt-response {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

  .coupons-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
    max-width: 100%;
      padding: 0;
  margin: 0;
  }

.coupon-item {
  width: 100%;
  max-width: 100%;
  box-sizing: border-box;
  margin-left: 0;
  margin-right: 0;
    background: #1a1a1f;
    border-radius: 12px;
    padding: 16px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    border: 1px solid rgba(255, 255, 255, 0.1);
    margin-bottom: 16px; /* Add margin-bottom for extra spacing */
}

  .coupon-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
  }

/* Update the coupon header styles */
.coupon-header-text h2 {
    font-size: 24px;
    font-weight: 600;
    color: #FFF;
    margin-bottom: 16px;
    border-bottom: none;
    text-decoration: none;
    border: none;
    position: relative;
}

.coupon-header-text h2::after {
    content: none;
}

  .coupon-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
  }

  .brand-info {
    display: flex;
    align-items: center;
    gap: 8px;
    font-weight: 500;
  }

  .brand-info i {
    font-size: 20px;
    color: #FFD700;
  }

  .discount-tag {
    background: linear-gradient(135deg, #2563eb, #1d4ed8);
    color: white;
    padding: 4px 8px;
    margin-left: 18px;
    border-radius: 4px;
    font-weight: 600;
  }

  .coupon-content {
    margin-bottom: 12px;
  }

  .description {
    margin-bottom: 8px;
    color: #fff;
  }

  .code-container {
    display: flex;
    align-items: center;
    gap: 8px;
    margin: 12px 0;
    background: rgba(255, 255, 255, 0.05);
    padding: 8px;
    border-radius: 6px;
  }

  .coupon-code {
    font-family: monospace;
    font-size: 16px;
    letter-spacing: 1px;
    color: #FFD700;
    background: transparent;
  }

  .copy-button {
    background: transparent;
    border: none;
    color: #FFD700;
    cursor: pointer;
    padding: 4px 8px;
    transition: transform 0.2s ease;
  }

  .copy-button:hover {
    transform: scale(1.1);
  }

  .validity {
    font-size: 0.9em;
    color: #888;
  }

  .toast {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%) translateY(100px);
    background: rgba(0, 0, 0, 0.8);
    color: white;
    padding: 12px 24px;
    border-radius: 8px;
    transition: transform 0.3s ease;
    z-index: 1000;
  }

  .toast.show {
    transform: translateX(-50%) translateY(0);
  }

  .coupon-info {
    margin-top: 16px;
    padding-top: 16px;
    border-top: none !important;
  }

  .coupon-info .info-section {
    margin-bottom: 12px;
  }

  .coupon-info h4 {
    font-size: 14.9px;
    margin-left: 0px;
    color: #FFD700;
    margin-bottom: 8px;
  }

  .coupon-info ul {
    font-size: 14px;
    margin-left: 0px;
    list-style: none;
    padding-left: 0;
  }

  .coupon-info li {
    color: #fff;
    margin-bottom: 4px;
    position: relative;
    padding-left: 20px;
  }

  .coupon-info li:before {
    content: '•';
    position: absolute;
    left: 0;
    color: #FFD700;
  }

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }

  @keyframes messageSlide {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
  }

  @media (max-width: 480px) {
    .coupons-container {
    width: 82vw;
  }

    .coupon-header-text h2 {
        font-size: 18px; /* Smaller size for mobile */
    }
}

.real-estate-container {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
  max-width: 540px; /* Drastically reduced width */
  margin: 0 auto;
  padding: 8px; /* Minimal padding */
}

.property-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
  width: 100%; /* Ensure property items take full width */
  box-sizing: border-box; /* Include padding and border in the width */
}

.property-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.property-header {
  display: flex;
  flex-direction: column; /* Stack title and price vertically on mobile */
  gap: 8px; /* Add spacing between title and price */
  margin-bottom: 12px;
}

.real-estate-header h2 {
    font-size: 24px;
    font-weight: 600;
    color: #FFF;
    margin-bottom: 16px;
    border-bottom: none;
    text-decoration: none;
    border: none;
    position: relative;
}

.real-estate-header h2::after {
    content: none;
}

.property-title {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.property-price {
  font-size: 16px;
  color: #fff;
}

.property-details {
  display: flex;
  gap: 12px; /* Space between details */
  margin-bottom: 12px;
  font-size: 18px;
  color: #fff;
  overflow-x: auto; /* Allow horizontal scrolling if needed */
  white-space: nowrap; /* Prevent wrapping */
}

.property-details > div {
  display: flex;
  align-items: center;
  gap: 4px; /* Space between icon and text */
  flex-shrink: 0; /* Prevent shrinking */
}

.property-description {
  font-size: 16px;
  color: #ccc;
  margin-bottom: 12px;
}

.property-contact {
  font-size: 14px;
  color: #fff;
  margin-bottom: 12px;
}

.carousel {
  position: relative;
  width: 100%;
  height: 200px; /* Reduce height for mobile */
  overflow: hidden;
  border-radius: 8px;
  margin-bottom: 12px;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.carousel-item.active {
  opacity: 1;
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.5);
  color: white;
  border: none;
  padding: 8px; /* Reduce padding for mobile */
  cursor: pointer;
  font-size: 16px; /* Reduce font size for mobile */
  z-index: 100;
}

.carousel-control.prev {
  left: 8px; /* Adjust position for mobile */
}

.carousel-control.next {
  right: 8px; /* Adjust position for mobile */
}

.property-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h3 {
  font-size: 20px; /* Larger font size for section headings on desktop */
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 18px; /* Larger font size for list items on desktop */
  color: #fff;
  margin-bottom: 4px;
}

.action-buttons {
  display: flex;
  gap: 12px; /* Space between buttons */
  margin-top: 16px;
}

.book-now-button,
.whatsapp-button {
  display: inline-flex; /* Use flex for better alignment */
  align-items: center;
  justify-content: center;
  padding: 10px 16px; /* Adjust padding for better touch targets */
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
  cursor: pointer;
  flex: 1; /* Equal width for both buttons */
}

.book-now-button {
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
}

.whatsapp-button {
  background: linear-gradient(135deg, #25d366, #128c7e);
}

.book-now-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.whatsapp-button:hover {
  background: linear-gradient(135deg, #128c7e, #075e54);
  transform: translateY(-1px);
}

.book-now-button i,
.whatsapp-button i {
  margin-right: 8px;
}

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .real-estate-container {
    width: 85vw;
  }

  .property-item {
    border-radius: 12px;
    margin-left: 0; /* Remove left margin */
    margin-right: 0; /* Remove right margin */
  }

  .property-header {
    flex-direction: column; /* Stack title and price vertically */
  }

    .real-estate-header h2 {
      font-size:  18px; /* Smaller size for mobile */
    }

  .property-details {
    gap: 8px; /* Reduce gap for smaller screens */
    font-size: 12px; /* Reduce font size for smaller screens */
  }

  .property-details > div {
    gap: 2px; /* Reduce gap between icon and text */
  }

    .property-description {
    font-size: 12px; /* Reduce font size for smaller screens */
    margin-bottom: 12px; /* Reduce margin for smaller screens */
    line-height: 1.5; /* Adjust line height for mobile */
  }

  .carousel {
    height: 150px; /* Further reduce height for smaller mobile screens */
  }

  .carousel-control {
    padding: 6px; /* Further reduce padding for smaller screens */
    font-size: 14px; /* Further reduce font size for smaller screens */
  }

    .info-section h3 {
    font-size: 16px; /* Slightly smaller font size for section headings on mobile */
  }

  .info-section li {
    font-size: 14px; /* Smaller font size for list items on mobile */
  }

  .action-buttons {
    flex-direction: row; /* Ensure buttons stay horizontal */
    gap: 8px; /* Reduce gap for smaller screens */
  }

  .book-now-button,
  .whatsapp-button {
    padding: 8px 12px; /* Adjust padding for smaller screens */
    font-size: 14px; /* Reduce font size for smaller screens */
  }
}

/* Default styles for jobs container */
.jobs-container {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
  max-width: 540px; /* Drastically reduced width */
  margin: 0 auto;
  padding: 8px; /* Minimal padding */
}

/* Default styles for job items */
.job-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
  width: 100%; /* Full width */
  margin-left: 0; /* Remove auto margin to align to the edges */
  margin-right: 0; /* Remove auto margin to align to the edges */
}

.job-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.job-header {
  display: flex;
  flex-direction: column; /* Stack title and company vertically on mobile */
  gap: 8px; /* Add spacing between title and company */
  margin-bottom: 12px;
}

.job-header-text h2 {
    font-size: 24px;
    font-weight: 600;
    color: #FFF;
    margin-bottom: 16px;
    border-bottom: none;
    text-decoration: none;
    border: none;
    position: relative;
}

.job-header-text h2::after {
    content: none;
}

.job-title {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.job-company {
  font-size: 14px;
  color: #888;
}

.job-details {
  display: flex;
  flex-wrap: wrap; /* Allow details to wrap on smaller screens */
  gap: 8px; /* Reduce gap for mobile */
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.job-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.apply-button {
  display: inline-block;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.apply-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.job-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h3 {
  font-size: 18px; /* Larger font size for section headings on desktop */
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 16px; /* Larger font size for list items on desktop */
  color: #fff;
  margin-bottom: 8px; /* Increase spacing between list items */
  line-height: 1.6; /* Improve readability with more line spacing */
}

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .job-item {
    width: 85vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }

  .job-header {
    flex-direction: column; /* Stack title and company vertically */
    gap: 4px; /* Reduce gap for smaller screens */
  }

 .job-header-text h2 {
    font-size: 18px; /* Smaller size for mobile */
  }

  .job-title {
    font-size: 16px; /* Reduce font size for smaller screens */
  }

  .job-company {
    font-size: 12px; /* Reduce font size for smaller screens */
  }

  .job-details {
    flex-direction: column; /* Stack details vertically */
    gap: 4px; /* Reduce gap for smaller screens */
    font-size: 12px; /* Reduce font size for smaller screens */
  }

  .job-description {
    font-size: 12px; /* Reduce font size for smaller screens */
    margin-bottom: 8px; /* Reduce margin for smaller screens */
  }

  .apply-button {
    padding: 6px 12px; /* Adjust padding for smaller screens */
    font-size: 14px; /* Reduce font size for smaller screens */
  }

  .info-section h3 {
    font-size: 16px; /* Slightly smaller font size for section headings on mobile */
  }

  .info-section li {
    font-size: 14px; /* Smaller font size for list items on mobile */
    margin-bottom: 6px; /* Adjust spacing for smaller screens */
    line-height: 1.5; /* Adjust line height for mobile */
  }
}

.automotive-response {
  display: flex;
  flex-direction: column;
  gap: 6px;
  width: 100%;
  max-width: 540px; /* Drastically reduced width */
  margin: 0 auto;
  padding: 8px; /* Minimal padding */
}

.automotive-response p {
  margin-top: -20px;
  font-size: 18px; /* Minimal font size */
  font-weight: 600;
  color: #FFF;
}

.vehicles-list {
  display: flex;
  flex-direction: column;
  gap: 8px; /* Minimal gap */
  width: 100%;
}

.vehicle-item {
  background: #1a1a1f;
  border-radius: 12px; /* Reduced radius */
  padding: 10px; /* Minimal padding */
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  max-width: 520px; /* Much smaller width */
  margin: 0 auto;
}

.vehicle-item:hover {
  transform: translateY(-1px);
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
}

.vehicle-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px; /* Minimal margin */
}

.vehicle-title {
  font-size: 15px; /* Minimal title size */
  font-weight: 600;
  color: #FFD700;
}

.vehicle-price {
  font-size: 14px; /* Minimal font size */
  color: #fff;
  font-weight: 500;
}

.vehicle-details {
  display: flex;
  gap: 10px; /* Reduced gap */
  margin-bottom: 6px; /* Minimal margin */
  font-size: 12px; /* Very small font */
  color: #fff;
}

.vehicle-description {
  font-size: 12px; /* Very small font */
  color: #ccc;
  margin-bottom: 6px; /* Minimal margin */
  line-height: 1.3; /* Compact line height */
}

.vehicle-contact {
  font-size: 12px; /* Very small font */
  color: #fff;
  margin-bottom: 6px; /* Minimal margin */
}

.vehicle-info {
  margin-top: 8px; /* Minimal margin */
  padding: 10px; /* Minimal padding */
  background: #1a1a1f;
  border-radius: 5px; /* Reduced radius */
  clear: both;
  animation: fadeIn 0.3s ease-out;
  max-width: 520px; /* Much smaller width */
  margin-left: auto;
  margin-right: auto;
}

.vehicle-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 8px; /* Minimal margin */
}

.info-section h3 {
  font-size: 14px; /* Minimal font size */
  margin-bottom: 4px; /* Minimal margin */
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 12px; /* Minimal padding */
}

.info-section li {
  font-size: 12px; /* Very small font */
  color: #fff;
  margin-bottom: 3px; /* Minimal margin */
  line-height: 1.2; /* Very compact */
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

/* Drastically Reduced Carousel */
.automotive-carousel {
  position: relative;
  width: 100%;
  height: 220px; /* Much smaller height */
  overflow: hidden;
  border-radius: 6px; /* Reduced radius */
  margin-bottom: 8px; /* Minimal margin */
  max-width: 420px; /* Much smaller width */
  margin-left: auto;
  margin-right: auto;
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.2);
}

.automotive-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.automotive-carousel-item.active {
  opacity: 1;
}

.automotive-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: contain;
  background-color: #121216;
}

.automotive-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.7);
  color: white;
  border: none;
  padding: 4px; /* Minimal padding */
  cursor: pointer;
  font-size: 12px; /* Very small font */
  z-index: 100;
  border-radius: 50%;
  width: 24px; /* Very small control */
  height: 24px; /* Very small control */
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.automotive-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
}

.automotive-carousel-control.prev {
  left: 8px; /* Minimal spacing */
}

.automotive-carousel-control.next {
  right: 8px; /* Minimal spacing */
}

.action-buttons {
  display: flex;
  gap: 8px; /* Minimal gap */
  margin-top: 8px; /* Minimal margin */
  justify-content: center;
}

.book-now-button,
.whatsapp-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 4px; /* Minimal gap */
  padding: 5px 10px; /* Minimal padding */
  color: white;
  border: none;
  border-radius: 4px; /* Reduced radius */
  text-align: center;
  text-decoration: none;
  font-weight: 600;
  font-size: 12px; /* Very small font */
  transition: all 0.2s ease;
  cursor: pointer;
  min-width: 100px; /* Much smaller width */
}

.book-now-button {
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
}

.whatsapp-button {
  background: linear-gradient(135deg, #25d366, #128c7e);
}

.book-now-button:hover,
.whatsapp-button:hover {
  transform: translateY(-1px);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

@media (min-width: 481px) {
  .automotive-response {
    width: 85vw;
  }
  
  .vehicle-item {
    border-radius: 10px;
    padding: 16px;
  }
  
  .automotive-carousel {
    height: 300px;
  }
}

@media (min-width: 1080px) {
  .automotive-response {
    margin-left: 450px;
  }
}

/* Media Queries for Mobile Devices */
@media (max-width: 480px) {
  .vehicle-item {
    width: 85vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }

  /* Automotive Carousel Styles */
.automotive-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 8px;
  margin-bottom: 12px;
}

  .action-buttons {
    flex-direction: row;
    gap: 8px;
  }

  .book-now-button,
  .whatsapp-button {
    padding: 8px 8px;
    font-size: 14px;
  }
}  

.provider-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.provider-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-top: 0px;
}

.provider-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.provider-header h2 {
    font-size: 18px;
    font-weight: 600;
    color: #FFF;
    margin-top: -10px;
    border-bottom: none; /* Remove border */
    text-decoration: none; /* Remove underline */
}

.provider-header-text h2::after {
    content: none;
}

.provider-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4px;
}

.provider-name {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.provider-rating {
  font-size: 16px;
  color: #fff;
}

.provider-details {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.provider-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.provider-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.provider-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h3 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@media (max-width: 480px) {
  .provider-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

.disease-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.disease-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.disease-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.disease-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.disease-name {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.disease-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.disease-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.disease-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h3 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@media (max-width: 480px) {
  .disease-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
  .action-button {
    padding: 6px 12px; /* Smaller padding for mobile */
    font-size: 12px; /* Smaller font size for mobile */
    gap: 4px; /* Reduce gap between icon and text */
  }

  .action-button i {
    font-size: 12px; /* Smaller icon size for mobile */
  }
}

.domain-registration-response,
.stock-response {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  margin-bottom: 16px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.domain-registration-response p,
.stock-response p {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
  margin-bottom: 12px;
}

.domain-registration-response ul,
.stock-response ul {
  margin: 0;
  padding-left: 20px;
  color: #fff;
  margin-bottom: 12px;
}

.train-booking-response button,
.flight-booking-response button,
.domain-registration-response button,
.bus-booking-response button,
.stock-response button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s ease;
}

.train-booking-response button:hover,
.flight-booking-response button:hover,
.domain-registration-response button:hover,
.stock-response button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

/* Modal Styles */
.modal {
  display: none;
  position: fixed;
  z-index: 1000;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  justify-content: center;
  align-items: center;
}

.modal-content {
  background: #1a1a1f;
  padding: 20px;
  border-radius: 12px;
  width: 90%;
  max-width: 400px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.modal-content h2 {
  font-size: 24px;
  font-weight: 600;
  color: #FFD700;
  margin-bottom: 16px;
}

.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  cursor: pointer;
}

.close:hover {
  color: #fff;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  font-size: 14px;
  color: #fff;
  margin-bottom: 8px;
}

.form-group input,
.form-group select {
  width: 100%;
  padding: 8px;
  border-radius: 6px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  background: #2a2a2f;
  color: #fff;
  font-size: 14px;
}

.search-button {
  width: 100%;
  padding: 10px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.search-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.movie-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.movie-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.movie-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.movie-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.movie-title {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.movie-release-date {
  font-size: 14px;
  color: #fff;
}

.movie-details {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.movie-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.movie-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.movie-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h4 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@media (max-width: 480px) {
  .movie-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

.franchise-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.franchise-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.franchise-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.franchise-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.franchise-name {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.franchise-category {
  font-size: 14px;
  color: #fff;
}

.franchise-details {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.franchise-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.franchise-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.franchise-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h4 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@media (max-width: 480px) {
  .franchise-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

.courses-container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  width: 100%;
  padding: 0;
  margin: 0;
}

.course-item {
  background: #1a1a1f;
  border-radius: 12px;
  padding: 16px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
  margin-bottom: 16px;
}

.course-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.course-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.course-name {
  font-size: 18px;
  font-weight: 600;
  color: #FFD700;
}

.course-category {
  font-size: 14px;
  color: #fff;
}

.course-details {
  display: flex;
  gap: 12px;
  margin-bottom: 12px;
  font-size: 14px;
  color: #fff;
}

.course-description {
  font-size: 14px;
  color: #ccc;
  margin-bottom: 12px;
}

.action-buttons {
  display: flex;
  gap: 12px;
  margin-top: 16px;
}

.action-button {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: linear-gradient(135deg, #2563eb, #1d4ed8);
  color: white;
  border: none;
  border-radius: 6px;
  text-align: center;
  text-decoration: none;
  font-weight: 500;
  transition: all 0.2s ease;
}

.action-button:hover {
  background: linear-gradient(135deg, #1d4ed8, #1e40af);
  transform: translateY(-1px);
}

.course-info {
  margin-top: 16px;
  padding: 16px;
  background: #1a1a1f;
  border-radius: 4px;
  clear: both;
  animation: fadeIn 0.3s ease-out;
}

.course-info.hidden {
  display: none;
}

.info-section {
  margin-bottom: 16px;
}

.info-section h4 {
  font-size: 16px;
  margin-bottom: 8px;
  color: #FFD700;
}

.info-section ul {
  margin: 0;
  padding-left: 20px;
}

.info-section li {
  font-size: 14px;
  color: #fff;
  margin-bottom: 4px;
}

@media (max-width: 480px) {
  .course-item {
    width: 82vw;
    padding: 12px;
    border-radius: 12px;
    margin-bottom: 12px;
  }
}

.loan-container {
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
    padding: 0;
    margin: 0;
}

.loan-item {
    background: #1a1a1f;
    border-radius: 12px;
    padding: 16px;
    cursor: pointer;
    transition: transform 0.2s ease, box-shadow 0.2s ease;
    border: 1px solid rgba(255, 255, 255, 0.1);
    margin-bottom: 16px;
}

.loan-item:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
}

.loan-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
}

.loan-name {
    font-size: 18px;
    font-weight: 600;
    color: #FFD700;
}

.loan-type {
    font-size: 14px;
    color: #fff;
}

.loan-details {
    display: flex;
    gap: 12px;
    margin-bottom: 12px;
    font-size: 14px;
    color: #fff;
}

.loan-description {
    font-size: 14px;
    color: #ccc;
    margin-bottom: 12px;
}

.apply-now-button {
    display: inline-flex;
    align-items: center;
    gap: 8px;
    padding: 8px 16px;
    background: linear-gradient(135deg, #2563eb, #1d4ed8);
    color: white;
    border: none;
    border-radius: 6px;
    text-align: center;
    text-decoration: none;
    font-weight: 500;
    transition: all 0.2s ease;
}

.apply-now-button:hover {
    background: linear-gradient(135deg, #1d4ed8, #1e40af);
    transform: translateY(-1px);
}

.loan-info {
    margin-top: 16px;
    padding: 16px;
    background: #1a1a1f;
    border-radius: 4px;
    clear: both;
    animation: fadeIn 0.3s ease-out;
}

.loan-info.hidden {
    display: none;
}

.info-section {
    margin-bottom: 16px;
}

.info-section h4 {
    font-size: 16px;
    margin-bottom: 8px;
    color: #FFD700;
}

.info-section ul {
    margin: 0;
    padding-left: 20px;
}

.info-section li {
    font-size: 14px;
    color: #fff;
    margin-bottom: 4px;
}

@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

@media (max-width: 480px) {
    .loan-item {
        width: 82vw;
        padding: 12px;
        border-radius: 12px;
        margin-bottom: 12px;
    }
}

      `;

document.head.appendChild(style);

                const townsData = [
  {
    name: "Mumbai",
    image: "https://i.pinimg.com/originals/2c/72/40/2c72408b797d4841b6fe6c395183ba35.png",
    quote: "The town of dreams where ambitions take flight and opportunities never sleep"
  },
  {
    name: "Delhi",
    image: "https://www.mistay.in/travel-blog/content/images/2020/07/travel-4813658_1920.jpg",
    quote: "Where ancient heritage meets modern aspirations in perfect harmony"
  },
  {
    name: "Jaipur",
    image: "https://www.tripsavvy.com/thmb/Afl1v6bgmGid9kPfseymDiAYWa0=/3595x2397/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-518387310-04a30994bfb1461bb8000f1864ca1fc5.jpg",
    quote: "Pink Town, where royal heritage colors modern dreams"
  }
];

function showTownsOverlay() {
  let overlay = document.getElementById('townsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'townsOverlay';
    overlay.className = 'towns-overlay';
    document.body.appendChild(overlay);
  }
  overlay.innerHTML = `
    <div class="towns-content">
      <div class="towns-header">
        <h2>Cities We Serve</h2>
        <button class="close-towns" onclick="hideTownsOverlay()">
          <i class="fas fa-long-arrow-alt-left"></i>
        </button>
      </div>
      <div class="towns-grid">
        ${townsData.map(town => `
          <div class="town-card" onclick="selectTown('${town.name}')">
            <div class="town-image-container">
              <img src="${town.image}" alt="${town.name}" class="town-image">
              <div class="town-overlay"></div>
            </div>
            <div class="town-info">
              <h3 class="town-name">${town.name}</h3>
              <p class="town-quote">${town.quote}</p>
            </div>
          </div>
        `).join('')}
      </div>
    </div>
  `;
  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
.towns-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background:  #000000;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
}

.towns-overlay.active {
  opacity: 1;
  visibility: visible;
}

.towns-content {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.towns-header {
  padding: 8px 16px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 2px solid var(--primary-color);
  backdrop-filter: blur(10px);
  height: 65px;
  /* Flexbox for horizontal layout */
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 5px;
}

.towns-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  flex-grow: 0;
}

.close-towns {
  background: none;
  border: none;
  color: var(--primary-color);
  font-size: 1.5em;
  cursor: pointer;
  padding: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.towns-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  margin-top: 10px; /* Ensure content starts below the header */
}

.town-card {
  border-radius: 12px;
  overflow: hidden;
  background: rgba(26, 26, 31, 0.98);
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.town-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.town-image-container {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.town-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.town-card:hover .town-image {
  transform: scale(1.1);
}

.town-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.town-info {
  padding: 20px;
  position: relative;
  z-index: 1;
}

.town-name {
  color: var(--text-light);
  margin: 0 0 10px 0;
  font-size: 1.4em;
}

.town-quote {
  color: #cccccc;
  font-size: 0.9em;
  line-height: 1.4;
  margin: 0;
}

    .list-container {
      padding: 15px;
      background: #f8f9fa;
      border-radius: 8px;
    }

    .list-item {
      background: white;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 15px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      transition: all 0.3s ease;
    }

    .list-item:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    }

    .item-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
    }

    .rating {
      color: #ffd700;
      font-weight: bold;
    }

    .item-content {
      margin-bottom: 15px;
    }

    .item-specific {
      font-size: 0.9em;
      color: #666;
      margin: 5px 0;
    }

    .item-actions {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    .action-button {
      padding: 8px 16px;
      background: linear-gradient(to bottom, #2563eb, #1d4ed8);
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      font-weight: 500;
      transition: all 0.2s ease;
    }

    .action-button:hover {
      background: linear-gradient(to bottom, #1d4ed8, #1e40af);
      transform: translateY(-1px);
    }

    .info-button {
      background: #6c757d;
      color: white;
      border: none;
      border-radius: 6px;
      padding: 8px 16px;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: all 0.2s ease;
    }

    .info-button:hover {
      background: #5a6268;
    }

    .detailed-info {
      margin-top: 15px;
      padding: 15px;
      background: #f8f9fa;
      border-radius: 4px;
      animation: fadeIn 0.3s ease-out;
    }

    .hidden {
      display: none;
    }

    .info-section {
      margin-bottom: 15px;
    }

    .info-section h4 {
      margin-bottom: 8px;
      color: #333;
    }

    .info-section ul {
      margin: 0;
      padding-left: 20px;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(10px); }
      to { opacity: 1; transform: translateY(0); }
    }
  
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 768px) {
  .towns-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  }
  
  .town-image-container {
    height: 160px;
  }
}

    `;
  document.head.appendChild(style);
}

function hideTownsOverlay() {
  const overlay = document.getElementById('townsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function selectTown(townName) {
  console.log(`Selected town: ${townName}`);
  showAlert(`Selected location: ${townName}`, 'success');
  hideTownsOverlay();
}
              
document.addEventListener('DOMContentLoaded', function() {
    // Select the location link in the menu section
    const menuLocationLink = document.querySelector('.menu-section .location-link');
    
    if (menuLocationLink) {
        menuLocationLink.addEventListener('click', function(e) {
            e.preventDefault(); // Prevent default link behavior
            showTownsOverlay(); // Call the existing function to show towns overlay
        });
    }
});


const nysaFeedData = [
  // Service Providers
  {
    id: "feed-001",
    category: "Service Providers",
    details: {
      title: "Premium Home Cleaning Services",
      headerImage: "https://logodownload.org/wp-content/uploads/2021/07/dominos-pizza-logo-0.png",
      detailImage: "https://logodownload.org/wp-content/uploads/2021/07/dominos-pizza-logo-0.png",
      images: [
        "https://images.unsplash.com/photo-1581578731548-c64695cc6952?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1600585152220-90363fe7e115?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1513694203232-719a280e022f?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Professional home cleaning services with eco-friendly products. Serving all areas of the city with trained staff.",
      provider: "CleanMaster Pro",
      location: "New Delhi",
      datePosted: "2 hours ago",
      price: "₹499 per hour",
      contact: "+91 9876543210",
      content: `
        <h2>About Our Service</h2>
        <p>We provide top-quality home cleaning services with 100% satisfaction guarantee. Our team is trained in modern cleaning techniques and uses only eco-friendly products.</p>
        
        <h3>Service Details</h3>
        <ul>
          <li>Deep cleaning for homes and offices</li>
          <li>Regular maintenance cleaning packages</li>
          <li>Specialized kitchen and bathroom cleaning</li>
          <li>Carpet and sofa cleaning</li>
          <li>Post-renovation cleaning</li>
        </ul>
        
        <h3>Why Choose Us?</h3>
        <ul>
          <li>5 years of trusted service</li>
          <li>300+ satisfied customers</li>
          <li>Verified and background-checked staff</li>
          <li>Flexible scheduling</li>
          <li>Affordable pricing</li>
        </ul>
        
        <h3>Service Areas</h3>
        <p>All major areas of New Delhi including South Delhi, West Delhi, and Noida.</p>
      `,
      tags: ["cleaning", "home services", "professional"],
      actionButton: {
        text: "Book Now",
        url: "#"
      },
      social: {
        website: "https://cleanmasterpro.com",
        whatsapp: "https://wa.me/919876543210"
      }
    }
  },
  {
    id: "feed-002",
    category: "Healthcare",
    details: {
      title: "Dr. Sharma's Dental Clinic",
      headerImage: "https://example.com/providers/dr-sharma-header.jpg",
      detailImage: "https://example.com/providers/dr-sharma-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1588776814546-1ffcf47267a5?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Experienced dental care with modern equipment. Specializing in cosmetic dentistry and dental implants.",
      provider: "Dr. Rajesh Sharma",
      location: "Mumbai",
      datePosted: "1 day ago",
      price: "Consultation: ₹500",
      contact: "+91 8765432109",
      content: `
        <h2>About Our Clinic</h2>
        <p>Dr. Sharma's Dental Clinic has been serving patients for over 15 years with state-of-the-art dental care facilities.</p>
        
        <h3>Services Offered</h3>
        <ul>
          <li>Teeth Whitening</li>
          <li>Dental Implants</li>
          <li>Root Canal Treatment</li>
          <li>Braces and Aligners</li>
          <li>General Dentistry</li>
        </ul>
        
        <h3>Clinic Hours</h3>
        <p>Monday to Saturday: 9:00 AM - 8:00 PM<br>
        Sunday: Emergency only</p>
        
        <h3>Our Team</h3>
        <p>Led by Dr. Rajesh Sharma (MDS) with 3 supporting dentists and 5 dental hygienists.</p>
      `,
      tags: ["dentist", "healthcare", "mumbai"],
      actionButton: {
        text: "Book Appointment",
        url: "#"
      },
      social: {
        website: "https://drsharmadental.com",
        instagram: "https://instagram.com/drsharmadental"
      }
    }
  },
  {
    id: "feed-003",
    category: "Jobs",
    details: {
      title: "Marketing Manager - Digital Marketing",
      headerImage: "https://example.com/providers/techsolutions-header.jpg",
      detailImage: "https://example.com/providers/techsolutions-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1551288049-bebda4e38f71?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Looking for an experienced Digital Marketing Manager to lead our marketing team. Minimum 5 years experience required.",
      provider: "TechSolutions Pvt. Ltd.",
      location: "Bangalore",
      datePosted: "3 days ago",
      salary: "₹8-12 LPA",
      requirements: `
        <h3>Requirements</h3>
        <ul>
          <li>Bachelor's degree in Marketing or related field</li>
          <li>5+ years digital marketing experience</li>
          <li>Expertise in SEO, SEM, and social media</li>
          <li>Analytical skills and data-driven thinking</li>
          <li>Excellent communication skills</li>
        </ul>
        
        <h3>Responsibilities</h3>
        <ul>
          <li>Develop digital marketing strategy</li>
          <li>Manage all digital marketing channels</li>
          <li>Measure and report performance</li>
          <li>Identify trends and insights</li>
          <li>Optimize spend and performance</li>
        </ul>
      `,
      tags: ["job", "marketing", "remote"],
      actionButton: {
        text: "Apply Now",
        url: "#"
      },
      social: {
        website: "https://techsolutions.com/careers",
        linkedin: "https://linkedin.com/company/techsolutions"
      }
    }
  },
  {
    id: "feed-004",
    category: "Real Estate",
    details: {
      title: "3BHK Luxury Apartment in Gurgaon",
      headerImage: "https://example.com/providers/elite-properties-header.jpg",
      detailImage: "https://example.com/providers/elite-properties-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1580587771525-78b9dba3b914?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1560448204-e02f11c3d0e2?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1493809842364-78817add7ffb?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1502672260266-1c1ef2d93688?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Spacious 3BHK apartment in premium society with all modern amenities. Ready to move in.",
      provider: "Elite Properties",
      location: "Gurgaon, Sector 56",
      datePosted: "1 week ago",
      price: "₹2.5 Cr",
      specs: "2100 sq.ft | 3 Bed | 3 Bath | 2 Balcony",
      content: `
        <h2>Property Details</h2>
        <p>This luxurious apartment offers premium living in one of Gurgaon's most sought-after locations.</p>
        
        <h3>Key Features</h3>
        <ul>
          <li>Spacious living/dining area</li>
          <li>Modern modular kitchen</li>
          <li>Master bedroom with walk-in closet</li>
          <li>3 bathrooms with premium fittings</li>
          <li>Floor-to-ceiling windows</li>
        </ul>
        
        <h3>Society Amenities</h3>
        <ul>
          <li>24/7 security and CCTV</li>
          <li>Swimming pool</li>
          <li>Clubhouse</li>
          <li>Gymnasium</li>
          <li>Children's play area</li>
          <li>Power backup</li>
        </ul>
        
        <h3>Location Advantages</h3>
        <ul>
          <li>5 mins from Delhi-Jaipur Highway</li>
          <li>10 mins from Metro Station</li>
          <li>Close to top schools and hospitals</li>
          <li>Near shopping malls and restaurants</li>
        </ul>
      `,
      tags: ["real estate", "apartment", "gurgaon"],
      actionButton: {
        text: "Schedule Visit",
        url: "#"
      },
      social: {
        website: "https://eliteproperties.in",
        whatsapp: "https://wa.me/919876543210"
      }
    }
  },
  {
    id: "feed-005",
    category: "Government Schemes",
    details: {
      title: "PM Awas Yojana - Housing for All",
      headerImage: "https://example.com/providers/govt-scheme-header.jpg",
      detailImage: "https://example.com/providers/govt-scheme-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1560518883-ce09059eeffa?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Affordable housing scheme for urban poor. Apply now to get benefits under this government scheme.",
      provider: "Ministry of Housing",
      datePosted: "2 weeks ago",
      deadline: "31 December 2024",
      content: `
        <h2>Scheme Details</h2>
        <p>Pradhan Mantri Awas Yojana (Urban) aims to provide housing for all in urban areas by 2024.</p>
        
        <h3>Benefits</h3>
        <ul>
          <li>Interest subsidy of up to ₹2.67 lakh</li>
          <li>Credit Linked Subsidy Scheme (CLSS)</li>
          <li>Affordable housing in partnership</li>
          <li>Benefit for slum dwellers</li>
        </ul>
        
        <h3>Eligibility</h3>
        <ul>
          <li>Family income up to ₹18 lakh per annum</li>
          <li>No pucca house in name of any family member</li>
          <li>Beneficiary family should not have availed central assistance</li>
        </ul>
        
        <h3>How to Apply</h3>
        <ol>
          <li>Visit official website pmaymis.gov.in</li>
          <li>Click on "Citizen Assessment"</li>
          <li>Fill the application form</li>
          <li>Upload required documents</li>
          <li>Submit application</li>
        </ol>
      `,
      tags: ["government", "housing", "scheme"],
      actionButton: {
        text: "Apply Now",
        url: "https://pmaymis.gov.in"
      }
    }
  },
  {
    id: "feed-006",
    category: "Education",
    details: {
      title: "CBSE Board Exam Preparation Classes",
      headerImage: "https://example.com/providers/excel-academy-header.jpg",
      detailImage: "https://example.com/providers/excel-academy-detail.jpg",
      images: [
        "https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80",
        "https://images.unsplash.com/photo-1523050854058-8df90110c9f1?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80"
      ],
      description: "Specialized coaching for CBSE Class 10 and 12 students. Experienced faculty and small batch sizes.",
      provider: "Excel Academy",
      location: "Hyderabad",
      datePosted: "5 days ago",
      price: "₹3000/month",
      content: `
        <h2>About Our Program</h2>
        <p>Excel Academy has been helping students achieve academic excellence for over 10 years with our specialized CBSE coaching program.</p>
        
        <h3>Subjects Offered</h3>
        <ul>
          <li>Mathematics</li>
          <li>Science (Physics, Chemistry, Biology)</li>
          <li>Social Studies</li>
          <li>English</li>
          <li>Hindi</li>
        </ul>
        
        <h3>Features</h3>
        <ul>
          <li>Small batch sizes (max 15 students)</li>
          <li>Weekly tests and progress reports</li>
          <li>Doubt clearing sessions</li>
          <li>Study material provided</li>
          <li>Mock board exams</li>
        </ul>
        
        <h3>Faculty</h3>
        <p>Our faculty includes retired CBSE examiners and subject matter experts with 10+ years teaching experience.</p>
      `,
      tags: ["education", "cbse", "coaching"],
      actionButton: {
        text: "Enquire Now",
        url: "#"
      },
      social: {
        website: "https://excelacademy.in",
        facebook: "https://facebook.com/excelacademyhyd"
      }
    }
  }
];

function showNysaFeedOverlay() {
  let overlay = document.getElementById('nysaFeedOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'nysaFeedOverlay';
    overlay.className = 'nysa-feed-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(nysaFeedData.map(feed => feed.category))];

  overlay.innerHTML = `
    <div class="nysa-feed-content">
      <div class="nysa-feed-header">
        <h2>Nysa Feed</h2>
        <button class="close-nysa-feed" onclick="hideNysaFeedOverlay()">
          <i class="fas fa-times"></i>
        </button>
      </div>
      
      <div class="nysa-feed-filter-container">
        <div class="nysa-feed-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterNysaFeed('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="nysa-feed-grid" id="nysaFeedGrid">
        ${renderNysaFeedCards(nysaFeedData)}
      </div>
    </div>

    <div class="nysa-feed-search-container">
      <div class="input-container">
        <input type="text" id="nysaFeedSearchInput" placeholder="Search services, jobs, properties..." />
        <button class="voice-search" id="nysaFeedVoiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const searchInput = document.getElementById('nysaFeedSearchInput');
  const nysaFeedGrid = document.getElementById('nysaFeedGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performNysaFeedSearch(nysaFeedData, query, nysaFeedGrid, filterButtons, renderNysaFeedCards);
  }, 300);

  searchInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  searchInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performNysaFeedSearch(nysaFeedData, searchInput.value, nysaFeedGrid, filterButtons, renderNysaFeedCards);
    }
  });

  initializeNysaFeedVoiceSearch(searchInput, (query) => {
    performNysaFeedSearch(nysaFeedData, query, nysaFeedGrid, filterButtons, renderNysaFeedCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
      :root {
      --service-providers: #4CAF50;
      --healthcare: #F44336;
      --jobs: #2196F3;
      --real-estate: #FF9800;
      --government-schemes: #9C27B0;
      --education: #00BCD4;
      --primary-color: #FFD700;
      --background-dark: #1a1a1f;
      --card-background: rgba(255, 255, 255, 0.03);
      --border-color: rgba(255, 255, 255, 0.1);
      --text-primary: #FFFFFF;
      --text-secondary: rgba(255, 255, 255, 0.8);
      --text-tertiary: rgba(255, 255, 255, 0.6);
      --transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
      --border-radius: 12px;
    }

    .nysa-feed-overlay {
      position: fixed;
      top: 60px;
      left: 0;
      right: 0;
      bottom: 60px;
      background: #1a1a1f;
      z-index: 1000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
      overflow-y: auto;
      width: 100%;
      display: flex;
      flex-direction: column;
      scrollbar-width: none;
      -ms-overflow-style: none;
    }

    .nysa-feed-overlay::-webkit-scrollbar {
      display: none;
    }

    .nysa-feed-overlay.active {
      opacity: 1;
      visibility: visible;
    }

    .nysa-feed-content {
      flex: 1;
      width: 100%;
      max-width: 100%;
      padding: 20px;
      margin: 0 auto;
      position: relative;
    }

    /* Original Header Style */
    .nysa-feed-header {
      padding: 16px 24px;
      background: rgba(26, 26, 31, 0.95);
      position: fixed;
      width: 100%;
      top: 0;
      left: 0;
      z-index: 30;
      border-bottom: 1px solid rgba(255, 215, 0, 0.15);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      height: 72px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }

    .nysa-feed-header h2 {
      color: var(--primary-color);
      margin: 0;
      font-size: 1.75em;
      font-weight: 600;
      letter-spacing: -0.02em;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .nysa-feed-header h2::before {
      content: '';
      display: inline-block;
      width: 24px;
      height: 24px;
      background: var(--primary-color);
      mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M22 12h-4l-3 9L9 3l-3 9H2'%3E%3C/path%3E%3C/svg%3E");
      mask-repeat: no-repeat;
      mask-position: center;
      mask-size: contain;
    }

    .close-nysa-feed {
      background: rgba(255, 255, 255, 0.1);
      border: none;
      color: white;
      font-size: 1em;
      cursor: pointer;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: all 0.2s ease;
    }

    .close-nysa-feed:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: scale(1.05);
    }

    .close-nysa-feed:active {
      transform: scale(0.95);
    }

    /* Original Filter Buttons Style */
    .nysa-feed-filter-container {
      position: fixed;
      top: 70px;
      left: 0;
      width: 100vw;
      max-width: 100vw;
      z-index: 10;
      background: #1a1a1f;
      margin-left: 10px;
      padding: 10px 15px;
      box-sizing: border-box;
    }

    .nysa-feed-filter-buttons {
      display: flex;
      justify-content: flex-start;
      gap: 4px;
      padding-bottom: 0px;
      width: 100vw;
      margin-left: calc(-50vw + 50%);
      overflow-x: auto;
      scrollbar-width: none;
    }

    .nysa-feed-filter-buttons::-webkit-scrollbar {
      height: 0px;
    }

    .filter-button {
      margin-top: 2px;
      margin-left: 2px;
      margin-right: 5px;
      flex-shrink: 0;
      background: rgba(255, 255, 255, 0.05);
      color: var(--text-light);
      border: 1px solid rgba(255, 255, 255, 0.1);
      padding: 10px 15px;
      border-radius: 25px;
      transition: all 0.3s ease;
      cursor: pointer;
      white-space: nowrap;
      font-weight: 500;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }

    .filter-button:hover {
      background: var(--primary-color);
      color: black;
      transform: scale(1.05);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .filter-button.active {
      background: var(--primary-color);
      color: black;
      font-weight: 700;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    }

    /* Original Search Container Style */
    .nysa-feed-search-container {
      position: fixed;
      bottom: 0px;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      flex: 1;
      position: relative;
      display: flex;
      align-items: center;
    }

    #nysaFeedSearchInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #nysaFeedSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i { 
      font-size: 1.5em; 
    }

    /* Rest of the styles remain the same as your improved version */
    .nysa-feed-grid {
      margin-top: 70px;
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(450px, 1fr));
      gap: 20px;
      animation: fadeInUp 0.5s ease-out;
      padding-bottom: 140px;
      width: 100%;
    }

    .nysa-feed-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      overflow: hidden;
      border: 1px solid rgba(255, 255, 255, 0.1);
      position: relative;
    }

    .nysa-feed-card:hover {
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
      border-color: rgba(255, 215, 0, 0.3);
    }

    .feed-card-header {
      display: flex;
      align-items: center;
      padding: 12px 16px;
      background: rgba(255, 255, 255, 0.03);
      border-bottom: 1px solid rgba(255, 255, 255, 0.05);
    }

    .feed-provider-avatar {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      object-fit: cover;
      margin-right: 12px;
      border: 1px solid rgba(255, 215, 0, 0.3);
    }

    .feed-provider-info {
      flex: 1;
    }

    .feed-provider-name {
      font-weight: 600;
      font-size: 0.95em;
      margin-bottom: 2px;
    }

.feed-posted-info {
  display: flex;
  gap: 12px;
  font-size: 0.8em;
  color: #fff;
}

/* Improved Info Item Styles */
.info-item {
  display: flex;
  align-items: center;
  gap: 6px;
  background: rgba(255, 255, 255, 0.05);
  padding: 5px 10px;
  border-radius: 16px;
  font-size: 0.75em;
  transition: all 0.2s ease;
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
  border: 1px solid rgba(255, 255, 255, 0.05);
}

.info-item:hover {
  background: rgba(255, 255, 255, 0.1);
  transform: translateY(-1px);
}

.info-item i {
  font-size: 0.8em;
  color: var(--primary-color);
  opacity: 0.8;
}

/* Image Slider Container */
.feed-card-image-container {
  position: relative;
  width: 100%;
  height: 450px;
  overflow: hidden;
  cursor: grab;
}

.feed-card-image-container:active {
  cursor: grabbing;
}

.feed-card-image-slider {
  display: flex;
  width: 100%;
  height: 100%;
  transition: transform 0.4s cubic-bezier(0.22, 1, 0.36, 1);
}

.feed-card-slide {
  min-width: 100%;
  height: 100%;
  position: relative;
}

.feed-card-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.nysa-feed-card:hover .feed-card-image {
  transform: scale(1.03);
}

.feed-card-carousel {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 8px;
  z-index: 2;
  padding: 6px 12px;
  background: rgba(0, 0, 0, 0.4);
  border-radius: 20px;
  backdrop-filter: blur(5px);
  -webkit-backdrop-filter: blur(5px);
}

.carousel-indicator {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  transition: all 0.3s ease;
  cursor: pointer;
}

.carousel-indicator.active {
  background: var(--primary-color);
  transform: scale(1.2);
}
  
    .feed-card-content {
      padding: 16px;
    }

    .feed-card-title {
      font-size: 1.2em;
      font-weight: 700;
      margin: 0 0 8px 0;
      color: white;
      line-height: 1.3;
    }

    .feed-card-description {
      font-size: 0.95em;
      color: rgba(255, 255, 255, 0.8);
      margin-bottom: 12px;
      line-height: 1.5;
    }

    .feed-card-meta {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 12px;
    }

    .feed-card-price {
      font-size: 1.1em;
      font-weight: 600;
      color: var(--primary-color);
    }

    .feed-card-location {
      font-size: 0.9em;
      color: rgba(255, 255, 255, 0.6);
      display: flex;
      align-items: center;
    }

    .feed-card-location i {
      margin-right: 5px;
    }

    .feed-card-actions {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-top: 12px;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
    }

    .feed-action-button {
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      border: none;
      padding: 8px 16px;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .feed-action-button:hover {
      background: rgba(255, 215, 0, 0.2);
    }

    .feed-action-button i {
      font-size: 0.9em;
    }

    .feed-share-button {
      background: transparent;
      border: none;
      color: rgba(255, 255, 255, 0.7);
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 0.9em;
      transition: all 0.3s ease;
    }

    .feed-share-button:hover {
      color: var(--primary-color);
    }

    .feed-card-tags {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-top: 12px;
    }

    .feed-tag {
      background: rgba(255, 255, 255, 0.1);
      padding: 4px 10px;
      border-radius: 20px;
      font-size: 0.8em;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .nysa-feed-grid {
        grid-template-columns: 1fr;
        padding: 0 10px;
        margin-top: 80px;
        gap: 15px;
      }

      .nysa-feed-card {
        width: 100%;
        margin: 0;
      }

      .feed-card-image-container {
        height: 350px;
      }

      .nysa-feed-content {
        padding: 10px 0;
        padding-bottom: 200px;
      }

      #nysaFeedSearchInput {
        padding: 20px 20px;
      }
    }

    @media (min-width: 769px) {
      .nysa-feed-search-container {
        padding: 24px;
        bottom: 0px;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #nysaFeedSearchInput {
        height: 80px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 8px;
      }

      .voice-search i {
        font-size: 2.1em;
      }

      .nysa-feed-grid {
        padding-bottom: 180px;
      }
    }
  `;
  document.head.appendChild(style);
}

function getCategoryButtonStyle(category) {
  const colors = {
    'All': 'background: rgba(255, 255, 255, 0.1); color: white;',
    'Service Providers': 'background: var(--service-providers); color: white;',
    'Healthcare': 'background: var(--healthcare); color: white;',
    'Jobs': 'background: var(--jobs); color: white;',
    'Real Estate': 'background: var(--real-estate); color: white;',
    'Government Schemes': 'background: var(--government-schemes); color: white;',
    'Education': 'background: var(--education); color: white;'
  };
  
  const activeColors = {
    'All': 'background: white; color: black;',
    'Service Providers': 'background: #3d8b40; color: white;',
    'Healthcare': 'background: #d32f2f; color: white;',
    'Jobs': 'background: #1976d2; color: white;',
    'Real Estate': 'background: #f57c00; color: white;',
    'Government Schemes': 'background: #7b1fa2; color: white;',
    'Education': 'background: #0097a7; color: white;'
  };
  
  const hoverColors = {
    'All': 'background: rgba(255, 255, 255, 0.2);',
    'Service Providers': 'background: #66bb6a;',
    'Healthcare': 'background: #ef5350;',
    'Jobs': 'background: #42a5f5;',
    'Real Estate': 'background: #ffa726;',
    'Government Schemes': 'background: #ab47bc;',
    'Education': 'background: #26c6da;'
  };
  
  return `
    ${colors[category] || colors['All']}
    border: none;
  `;
}

function getActionButtonStyle(category) {
  const colors = {
    'Service Providers': 'var(--service-providers)',
    'Healthcare': 'var(--healthcare)',
    'Jobs': 'var(--jobs)',
    'Real Estate': 'var(--real-estate)',
    'Government Schemes': 'var(--government-schemes)',
    'Education': 'var(--education)'
  };
  
  return `
    background: ${colors[category] || 'var(--primary-color)'};
    color: white;
  `;
}

function getActionButtonText(category) {
  const texts = {
    'Service Providers': 'Book Now',
    'Healthcare': 'Book Appointment',
    'Jobs': 'Apply Now',
    'Real Estate': 'Schedule Visit',
    'Government Schemes': 'Apply Now',
    'Education': 'Enquire Now'
  };
  
  return texts[category] || 'View Details';
}

function renderNysaFeedCards(filteredData) {
  return filteredData.map(feed => `
    <div class="nysa-feed-card" onclick="openNysaFeedDetail('${feed.id}')">
      <div class="feed-card-header">
        <img src="${feed.details.headerImage || `https://ui-avatars.com/api/?name=${encodeURIComponent(feed.details.provider)}&background=random`}" 
             alt="${feed.details.provider}" class="feed-provider-avatar">
        <div class="feed-provider-info">
          <div class="feed-provider-name">${feed.details.provider}</div>
          <div class="feed-posted-info">
            <div class="info-item">
              <i class="far fa-clock"></i>
              <span>${feed.details.datePosted}</span>
            </div>
            ${feed.details.location ? `
              <div class="info-item">
                <i class="fas fa-map-marker-alt"></i>
                <span>${feed.details.location}</span>
              </div>
            ` : ''}
          </div>
        </div>
      </div>
      
      <div class="feed-card-image-container" id="image-container-${feed.id}">
        <div class="feed-card-image-slider">
          ${feed.details.images.map((image, index) => `
            <div class="feed-card-slide ${index === 0 ? 'active' : ''}" data-index="${index}">
              <img src="${image}" alt="${feed.details.title}" class="feed-card-image">
            </div>
          `).join('')}
        </div>
        
        ${feed.details.images.length > 1 ? `
          <div class="feed-card-carousel">
            ${feed.details.images.map((_, index) => `
              <div class="carousel-indicator ${index === 0 ? 'active' : ''}" data-index="${index}"></div>
            `).join('')}
          </div>
        ` : ''}
      </div>

      <div class="feed-card-content">
        <h3 class="feed-card-title">${feed.details.title}</h3>
        <p class="feed-card-description">${feed.details.description}</p>
        
        <div class="feed-card-meta">
          ${feed.details.price || feed.details.salary ? `
            <div class="feed-card-price">${feed.details.price || feed.details.salary}</div>
          ` : ''}
          ${feed.details.location ? `
            <div class="feed-card-location">
              <i class="fas fa-map-marker-alt"></i> ${feed.details.location}
            </div>
          ` : ''}
        </div>
        
        <div class="feed-card-actions">
          <button class="feed-action-button" onclick="event.stopPropagation(); handleFeedAction('${feed.id}')" 
                  style="${getActionButtonStyle(feed.category)}">
            <i class="fas ${getActionIcon(feed.category)}"></i> ${getActionButtonText(feed.category)}
          </button>
          <button class="feed-share-button" onclick="event.stopPropagation(); shareFeed('${feed.id}')">
            <i class="fas fa-share"></i> Share
          </button>
        </div>
        
        ${feed.details.tags ? `
          <div class="feed-card-tags">
            ${feed.details.tags.map(tag => `
              <span class="feed-tag">#${tag}</span>
            `).join('')}
          </div>
        ` : ''}
      </div>
    </div>
  `).join('');
}

function getActionIcon(category) {
  const icons = {
    'Service Providers': 'fa-calendar-alt',
    'Healthcare': 'fa-calendar-check',
    'Jobs': 'fa-briefcase',
    'Real Estate': 'fa-home',
    'Government Schemes': 'fa-file-alt',
    'Education': 'fa-graduation-cap'
  };
  return icons[category] || 'fa-info-circle';
}

function openNysaFeedDetail(feedId) {
  const feed = nysaFeedData.find(item => item.id === feedId);
  if (!feed) return;

  const feedPage = document.createElement('div');
  feedPage.className = 'nysa-feed-detail-page';
  feedPage.innerHTML = `
    <div class="nysa-feed-detail-header">
      <button class="back-button" onclick="closeNysaFeedDetail()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <div class="header-title">${feed.details.title}</div>
      <div class="header-actions">
        <button class="nysa-share-button" onclick="shareFeed('${feed.id}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="nysa-feed-detail-content">
      <div class="feed-detail-image-container">
        <div class="feed-image-slider">
          ${feed.details.images.map((image, index) => `
            <div class="feed-slide ${index === 0 ? 'active' : ''}" data-index="${index}">
              <img src="${image}" alt="${feed.details.title} - Image ${index + 1}" class="feed-detail-image">
            </div>
          `).join('')}
        </div>
        <button class="slider-nav prev" onclick="navigateSlide(-1)"><i class="fas fa-chevron-left"></i></button>
        <button class="slider-nav next" onclick="navigateSlide(1)"><i class="fas fa-chevron-right"></i></button>
        ${feed.details.images.length > 1 ? `
          <div class="feed-detail-carousel">
            ${feed.details.images.map((_, index) => `
              <div class="carousel-dot ${index === 0 ? 'active' : ''}" data-index="${index}"></div>
            `).join('')}
          </div>
        ` : ''}
      </div>
      
      <div class="feed-detail-info-container">
        <div class="feed-detail-basic-info">
          <div class="feed-provider-section">
            <img src="${feed.details.detailImage || `https://ui-avatars.com/api/?name=${encodeURIComponent(feed.details.provider)}&background=random`}" 
                 alt="${feed.details.provider}" class="feed-provider-avatar">
            <div class="feed-provider-meta">
              <div class="feed-provider-name">${feed.details.provider}</div>
              <div class="feed-posted-info">
                <span><i class="far fa-clock"></i> ${feed.details.datePosted}</span>
                ${feed.details.location ? `<span><i class="fas fa-map-marker-alt"></i> ${feed.details.location}</span>` : ''}
              </div>
            </div>
          </div>
          
          <div class="feed-content-section">
            <h1 class="feed-title">${feed.details.title}</h1>
            <p class="feed-description">${feed.details.description}</p>
            
            ${feed.details.price || feed.details.salary ? `
              <div class="feed-price-section">
                <span class="price-label">${feed.category === 'Jobs' ? 'Salary' : 'Price'}</span>
                <span class="price-value">${feed.details.price || feed.details.salary}</span>
              </div>
            ` : ''}
            
            ${feed.details.specs ? `
              <div class="feed-specs-section">
                <span class="specs-label">Details</span>
                <span class="specs-value">${feed.details.specs}</span>
              </div>
            ` : ''}
            
            ${feed.details.deadline ? `
              <div class="feed-deadline-section">
                <span class="deadline-label">Deadline</span>
                <span class="deadline-value">${feed.details.deadline}</span>
              </div>
            ` : ''}
          </div>
          
          <div class="feed-detail-full-content">
            ${feed.details.content}
          </div>
          
          ${feed.details.contact ? `
            <div class="feed-contact-section">
              <h3 class="section-title">Contact Information</h3>
              <div class="contact-methods">
                <div class="contact-method">
                  <i class="fas fa-phone"></i>
                  <span>${feed.details.contact}</span>
                </div>
                ${feed.details.social?.whatsapp ? `
                  <a href="${feed.details.social.whatsapp}" target="_blank" class="contact-method">
                    <i class="fab fa-whatsapp"></i>
                    <span>Chat on WhatsApp</span>
                  </a>
                ` : ''}
                ${feed.details.social?.website ? `
                  <a href="${feed.details.social.website}" target="_blank" class="contact-method">
                    <i class="fas fa-globe"></i>
                    <span>Visit Website</span>
                  </a>
                ` : ''}
              </div>
            </div>
          ` : ''}
          
          <div class="feed-action-section">
            <button class="feed-action-detail-button" onclick="handleFeedAction('${feed.id}')" 
                    style="${getActionButtonStyle(feed.category)}">
              <i class="fas ${getActionIcon(feed.category)}"></i>
              <span>${getActionButtonText(feed.category)}</span>
            </button>
          </div>
        </div>
        
        ${feed.details.tags ? `
          <div class="feed-tags-section">
            <h3 class="section-title">Tags</h3>
            <div class="tags-container">
              ${feed.details.tags.map(tag => `
                <span class="tag">#${tag}</span>
              `).join('')}
            </div>
          </div>
        ` : ''}
      </div>
    </div>
  `;

  document.body.appendChild(feedPage);
  addNysaFeedDetailStyles();
  initializeFeedCardSliders();
  initializeImageSlider();
  scrollToTop();
}

function initializeFeedCardSliders() {
  nysaFeedData.forEach(feed => {
    const container = document.getElementById(`image-container-${feed.id}`);
    if (!container || feed.details.images.length <= 1) return;
    
    const slider = container.querySelector('.feed-card-image-slider');
    const slides = container.querySelectorAll('.feed-card-slide');
    const indicators = container.querySelectorAll('.carousel-indicator');
    let currentIndex = 0;
    let touchStartX = 0;
    let touchEndX = 0;
    
    // Touch event handlers
    container.addEventListener('touchstart', (e) => {
      touchStartX = e.changedTouches[0].clientX;
    }, { passive: true });
    
    container.addEventListener('touchend', (e) => {
      touchEndX = e.changedTouches[0].clientX;
      handleSwipe();
    }, { passive: true });
    
    function handleSwipe() {
      const threshold = 50;
      const difference = touchStartX - touchEndX;
      
      if (difference > threshold) {
        // Swipe left - next slide
        goToSlide(currentIndex + 1);
      } else if (difference < -threshold) {
        // Swipe right - previous slide
        goToSlide(currentIndex - 1);
      }
    }
    
    function goToSlide(index) {
      if (index < 0) index = slides.length - 1;
      if (index >= slides.length) index = 0;
      
      currentIndex = index;
      slider.style.transform = `translateX(-${currentIndex * 100}%)`;
      
      // Update indicators
      indicators.forEach((indicator, i) => {
        indicator.classList.toggle('active', i === currentIndex);
      });
    }
    
    // Click events for indicators
    indicators.forEach(indicator => {
      indicator.addEventListener('click', (e) => {
        e.stopPropagation();
        const index = parseInt(indicator.getAttribute('data-index'));
        goToSlide(index);
      });
    });
  });
}

function initializeImageSlider() {
  const slider = document.querySelector('.feed-image-slider');
  const slides = document.querySelectorAll('.feed-slide');
  const dots = document.querySelectorAll('.carousel-dot');
  const prevBtn = document.querySelector('.slider-nav.prev');
  const nextBtn = document.querySelector('.slider-nav.next');
  
  if (!slider || slides.length === 0) return;
  
  let currentIndex = 0;
  let touchStartX = 0;
  let touchEndX = 0;
  
  // Function to update the slider
  function updateSlider() {
    slides.forEach((slide, index) => {
      slide.classList.toggle('active', index === currentIndex);
    });
    
    if (dots) {
      dots.forEach((dot, index) => {
        dot.classList.toggle('active', index === currentIndex);
      });
    }
  }
  
  // Navigation functions
  function goToSlide(index) {
    if (index < 0) index = slides.length - 1;
    if (index >= slides.length) index = 0;
    currentIndex = index;
    updateSlider();
  }
  
  function navigateSlide(direction) {
    goToSlide(currentIndex + direction);
  }
  
  // Event listeners for buttons
  if (prevBtn) prevBtn.addEventListener('click', () => navigateSlide(-1));
  if (nextBtn) nextBtn.addEventListener('click', () => navigateSlide(1));
  
  // Touch events for swipe
  slider.addEventListener('touchstart', (e) => {
    touchStartX = e.changedTouches[0].screenX;
  }, { passive: true });
  
  slider.addEventListener('touchend', (e) => {
    touchEndX = e.changedTouches[0].screenX;
    handleSwipe();
  }, { passive: true });
  
  function handleSwipe() {
    const difference = touchStartX - touchEndX;
    if (difference > 50) {
      // Swipe left - next slide
      navigateSlide(1);
    } else if (difference < -50) {
      // Swipe right - previous slide
      navigateSlide(-1);
    }
  }
  
  // Click events for dots
  if (dots) {
    dots.forEach(dot => {
      dot.addEventListener('click', () => {
        const index = parseInt(dot.getAttribute('data-index'));
        goToSlide(index);
      });
    });
  }
  
  // Auto-advance slides (optional)
  let slideInterval = setInterval(() => navigateSlide(1), 5000);
  
  // Pause on hover
  slider.addEventListener('mouseenter', () => clearInterval(slideInterval));
  slider.addEventListener('mouseleave', () => {
    slideInterval = setInterval(() => navigateSlide(1), 5000);
  });
  
  // Keyboard navigation
  document.addEventListener('keydown', (e) => {
    if (e.key === 'ArrowLeft') navigateSlide(-1);
    if (e.key === 'ArrowRight') navigateSlide(1);
  });
}

function addNysaFeedDetailStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .nysa-feed-detail-page {
      font-family: poppins;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: var(--background-dark);
      z-index: 2000;
      overflow-y: auto;
      color: var(--text-primary);
      animation: fadeIn 0.3s ease-out;
    }

    /* Enhanced Header */
    .nysa-feed-detail-header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      padding: 16px 24px;
      background: rgba(26, 26, 31, 0.98);
      backdrop-filter: blur(20px);
      -webkit-backdrop-filter: blur(20px);
      z-index: 10;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 2px 15px rgba(0, 0, 0, 0.3);
      border-bottom: 1px solid var(--border-color);
    }

    .header-title {
      font-size: 1.1em;
      font-weight: 600;
      color: var(--text-primary);
      flex: 1;
      text-align: center;
      padding: 0 20px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .back-button {
      background: rgba(255, 255, 255, 0.1);
      border: none;
      color: var(--text-primary);
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: var(--transition);
      flex-shrink: 0;
    }

    .back-button:hover {
      background: rgba(255, 255, 255, 0.2);
      transform: translateX(-2px);
    }

    .header-actions {
      display: flex;
      gap: 10px;
      flex-shrink: 0;
    }

    .nysa-share-button {
      background: rgba(255, 255, 255, 0.05);
      border: none;
      color: var(--text-primary);
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: var(--transition);
    }

    .nysa-share-button:hover {
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
    }

    /* Content Layout */
    .nysa-feed-detail-content {
      display: flex;
      flex-direction: column;
      padding-top: 72px;
      min-height: 100vh;
    }

    /* Enhanced Image Slider */
    .feed-detail-image-container {
      position: relative;
      width: 100%;
      height: 400px;
      overflow: hidden;
      background: rgba(0, 0, 0, 0.3);
    }

    .feed-image-slider {
      display: flex;
      width: 100%;
      height: 100%;
      transition: transform 0.5s ease;
    }

    .feed-slide {
      min-width: 100%;
      height: 100%;
      position: relative;
      opacity: 0;
      transition: opacity 0.5s ease;
    }

    .feed-slide.active {
      opacity: 1;
    }

    .feed-detail-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }

    .slider-nav {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0, 0, 0, 0.5);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      z-index: 2;
      transition: var(--transition);
    }

    .slider-nav:hover {
      background: rgba(0, 0, 0, 0.8);
    }

    .slider-nav.prev {
      left: 20px;
    }

    .slider-nav.next {
      right: 20px;
    }

    .slider-nav i {
      font-size: 1.2em;
    }

    .feed-detail-carousel {
      position: absolute;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      gap: 8px;
      z-index: 2;
    }

    .carousel-dot {
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.5);
      cursor: pointer;
      transition: var(--transition);
    }

    .carousel-dot.active {
      background: var(--primary-color);
      transform: scale(1.2);
    }

    /* Info Container */
    .feed-detail-info-container {
      padding: 30px;
      background: var(--background-dark);
    }

    /* Provider Section */
    .feed-provider-section {
      display: flex;
      align-items: center;
      gap: 15px;
      margin-bottom: 25px;
      padding-bottom: 20px;
      border-bottom: 1px solid var(--border-color);
    }

    .feed-provider-avatar {
      width: 60px;
      height: 60px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid var(--primary-color);
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }

    .feed-provider-meta {
      flex: 1;
    }

    .feed-provider-name {
      font-weight: 600;
      font-size: 1.1em;
      margin-bottom: 5px;
    }

    .feed-posted-info {
      display: flex;
      gap: 15px;
      font-size: 0.85em;
      color: var(--text-tertiary);
    }

    .feed-posted-info span {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .feed-posted-info i {
      font-size: 0.9em;
    }

    /* Content Section */
    .feed-content-section {
      margin-bottom: 30px;
    }

    .feed-title {
      font-size: 1.8em;
      font-weight: 700;
      margin: 0 0 15px 0;
      line-height: 1.3;
      color: var(--text-primary);
    }

    .feed-description {
      font-size: 1em;
      line-height: 1.6;
      margin-bottom: 25px;
      color: var(--text-secondary);
    }

    /* Info Sections */
    .feed-price-section,
    .feed-specs-section,
    .feed-deadline-section {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px;
      margin-bottom: 15px;
      background: var(--card-background);
      border-radius: var(--border-radius);
      border-left: 3px solid var(--primary-color);
    }

    .price-label,
    .specs-label,
    .deadline-label {
      font-size: 0.9em;
      color: var(--text-secondary);
    }

    .price-value,
    .specs-value,
    .deadline-value {
      font-size: 1.1em;
      font-weight: 600;
    }

    .price-value {
      color: var(--primary-color);
    }

    /* Full Content */
    .feed-detail-full-content {
      line-height: 1.7;
      margin-bottom: 30px;
      font-size: 0.95em;
      color: var(--text-secondary);
    }

    .feed-detail-full-content h2 {
      font-size: 1.4em;
      margin: 30px 0 15px;
      color: var(--primary-color);
      font-weight: 600;
    }

    .feed-detail-full-content h3 {
      font-size: 1.2em;
      margin: 25px 0 15px;
      font-weight: 500;
      color: var(--text-primary);
    }

    .feed-detail-full-content p {
      margin-bottom: 15px;
    }

    .feed-detail-full-content ul,
    .feed-detail-full-content ol {
      margin-bottom: 15px;
      padding-left: 20px;
    }

    .feed-detail-full-content li {
      margin-bottom: 8px;
    }

    .feed-detail-full-content blockquote {
      border-left: 3px solid var(--primary-color);
      padding-left: 15px;
      margin: 20px 0;
      font-style: italic;
      color: var(--text-tertiary);
    }

    .feed-detail-full-content table {
      width: 100%;
      border-collapse: collapse;
      margin: 20px 0;
      background: var(--card-background);
    }

    .feed-detail-full-content th,
    .feed-detail-full-content td {
      padding: 12px 15px;
      text-align: left;
      border-bottom: 1px solid var(--border-color);
    }

    .feed-detail-full-content th {
      background: rgba(58, 134, 255, 0.1);
      color: var(--primary-color);
      font-weight: 500;
    }

    /* Contact Section */
    .feed-contact-section {
      margin-bottom: 30px;
    }

    .section-title {
      font-size: 1.1em;
      font-weight: 600;
      margin: 0 0 15px 0;
      color: var(--primary-color);
      position: relative;
      padding-bottom: 8px;
    }

    .section-title::after {
      content: '';
      position: absolute;
      bottom: 0;
      left: 0;
      width: 40px;
      height: 2px;
      background: var(--primary-color);
    }

    .contact-methods {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    .contact-method {
      display: flex;
      align-items: center;
      gap: 12px;
      padding: 12px 15px;
      background: var(--card-background);
      border-radius: var(--border-radius);
      transition: var(--transition);
      text-decoration: none;
      color: var(--text-primary);
    }

    .contact-method:hover {
      background: rgba(255, 255, 255, 0.07);
      transform: translateX(5px);
    }

    .contact-method i {
      width: 20px;
      text-align: center;
      color: var(--primary-color);
    }

    /* Action Button */
    .feed-action-section {
      margin: 40px 0;
    }

    .feed-action-detail-button {
      width: 100%;
      border: none;
      padding: 16px;
      border-radius: var(--border-radius);
      font-weight: 600;
      font-size: 1em;
      cursor: pointer;
      transition: var(--transition);
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
    }

    .feed-action-detail-button:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
    }

    .feed-action-detail-button:active {
      transform: translateY(0);
    }

    /* Tags Section */
    .feed-tags-section {
      margin-bottom: 30px;
    }

    .tags-container {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
    }

    .tag {
      background: rgba(58, 134, 255, 0.1);
      padding: 8px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      color: var(--primary-color);
      transition: var(--transition);
    }

    .tag:hover {
      background: rgba(58, 134, 255, 0.2);
    }

    /* Responsive Design */
    @media (max-width: 1200px) {
      .feed-detail-image-container {
        height: 350px;
      }
    }

    @media (max-width: 768px) {
      .feed-title {
        font-size: 1.5em;
      }
      
      .feed-detail-info-container {
        padding: 20px;
      }
      
      .feed-provider-avatar {
        width: 50px;
        height: 50px;
      }
      
      .feed-price-section,
      .feed-specs-section,
      .feed-deadline-section {
        flex-direction: column;
        align-items: flex-start;
        gap: 5px;
      }

      .feed-detail-image-container {
        height: 300px;
      }
    }

    @media (max-width: 480px) {
      .nysa-feed-detail-header {
        padding: 12px 15px;
      }
      
      .header-title {
        font-size: 1em;
        padding: 0 10px;
      }
      
      .back-button,
      .nysa-share-button {
        width: 36px;
        height: 36px;
      }
      
      .feed-detail-image-container {
        height: 250px;
      }
      
      .feed-title {
        font-size: 1.3em;
      }

      .slider-nav {
        width: 30px;
        height: 30px;
      }
    }

    /* Animations */
    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }
  `;
  document.head.appendChild(style);
}

function closeNysaFeedDetail() {
  const feedPage = document.querySelector('.nysa-feed-detail-page');
  if (feedPage) {
    feedPage.remove();
  }
}

function hideNysaFeedOverlay() {
  const overlay = document.getElementById('nysaFeedOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function filterNysaFeed(category) {
  const nysaFeedGrid = document.getElementById('nysaFeedGrid');
  let filteredData = nysaFeedData;

  if (category !== 'All') {
    filteredData = filteredData.filter(feed => feed.category === category);
  }

  nysaFeedGrid.innerHTML = renderNysaFeedCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function performNysaFeedSearch(data, query, gridElement, filterButtons, renderFunction) {
  if (!query) {
    // If no query, show all feeds
    gridElement.innerHTML = renderFunction(data);
    return;
  }

  const lowerCaseQuery = query.toLowerCase();
  const filteredData = data.filter(feed => {
    return (
      feed.details.title.toLowerCase().includes(lowerCaseQuery) ||
      feed.details.description.toLowerCase().includes(lowerCaseQuery) ||
      feed.details.provider.toLowerCase().includes(lowerCaseQuery) ||
      feed.category.toLowerCase().includes(lowerCaseQuery) ||
      (feed.details.tags && feed.details.tags.some(tag => tag.toLowerCase().includes(lowerCaseQuery)))
    );
  });

  gridElement.innerHTML = filteredData.length > 0 
    ? renderFunction(filteredData)
    : '<div class="no-results">No results found matching your search.</div>';
}

function initializeNysaFeedVoiceSearch(inputElement, callback) {
  const voiceButton = document.getElementById('nysaFeedVoiceSearchButton');
  if (!voiceButton) return;

  let recognition;
  try {
    recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.continuous = false;
    recognition.interimResults = false;

    voiceButton.addEventListener('click', () => {
      if (voiceButton.classList.contains('active')) {
        recognition.stop();
        voiceButton.classList.remove('active');
        return;
      }

      voiceButton.classList.add('active');
      recognition.start();

      recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        inputElement.value = transcript;
        if (callback) callback(transcript);
        voiceButton.classList.remove('active');
      };

      recognition.onerror = (event) => {
        console.error('Voice recognition error', event.error);
        voiceButton.classList.remove('active');
      };

      recognition.onend = () => {
        voiceButton.classList.remove('active');
      };
    });
  } catch (e) {
    console.error('Voice recognition not supported', e);
    voiceButton.style.display = 'none';
  }
}

// Nysa Feed Interaction Functions
function handleFeedAction(feedId) {
  const feed = nysaFeedData.find(item => item.id === feedId);
  if (feed) {
    // In a real app, this would handle the appropriate action
    console.log(`Action taken on feed: ${feed.details.title} - ${feed.details.actionButton.text}`);
    showToast(`Action: ${feed.details.actionButton.text} for "${feed.details.title}"`);
    
    // Open the action URL if it exists
    if (feed.details.actionButton.url && feed.details.actionButton.url !== '#') {
      window.open(feed.details.actionButton.url, '_blank');
    }
  }
}

function shareFeed(feedId) {
  const feed = nysaFeedData.find(item => item.id === feedId);
  if (!feed) return;

  if (navigator.share) {
    navigator.share({
      title: feed.details.title,
      text: feed.details.description,
      url: feed.details.social?.website || '#',
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    // Fallback for browsers that don't support Web Share API
    const shareUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(`Check out "${feed.details.title}" on Nysa Feed`)}&url=${encodeURIComponent(feed.details.social?.website || '#')}`;
    window.open(shareUrl, '_blank');
  }
}

function showToast(message) {
  const toast = document.createElement('div');
  toast.className = 'nysa-feed-toast';
  toast.textContent = message;
  document.body.appendChild(toast);
  
  setTimeout(() => {
    toast.classList.add('show');
  }, 10);
  
  setTimeout(() => {
    toast.classList.remove('show');
    setTimeout(() => toast.remove(), 300);
  }, 3000);
  
  // Add toast styles if they don't exist
  if (!document.getElementById('nysa-feed-toast-styles')) {
    const style = document.createElement('style');
    style.id = 'nysa-feed-toast-styles';
    style.textContent = `
      .nysa-feed-toast {
        position: fixed;
        bottom: 30px;
        left: 50%;
        transform: translateX(-50%) translateY(100px);
        background: rgba(0, 0, 0, 0.8);
        color: white;
        padding: 12px 24px;
        border-radius: var(--border-radius);
        z-index: 3000;
        transition: transform 0.3s ease;
        font-size: 0.9em;
        max-width: 90%;
        text-align: center;
      }
      
      .nysa-feed-toast.show {
        transform: translateX(-50%) translateY(0);
      }
    `;
    document.head.appendChild(style);
  }
}

function scrollToTop() {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });
}

// Utility function for debouncing
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// Initialize Nysa Feed button event listener
document.addEventListener('DOMContentLoaded', function() {
  const nysaFeedButton = document.getElementById('nysaFeedButton');
  
  if (nysaFeedButton) {
    nysaFeedButton.addEventListener('click', function(e) {
      e.preventDefault();
      showNysaFeedOverlay();
    });
  }
});

const newsData = [
  // AI News
  {
    category: "AI",
    details: {
      title: "GPT-5 Release Date Leaked: What to Expect",
      image: "https://a57.foxnews.com/static.foxnews.com/foxnews.com/content/uploads/2024/05/1200/675/OPENAI-CHATGPT.jpg?ve=1&tl=1",
      description: "New leaks suggest GPT-5 will be released in Q4 2024 with multimodal capabilities surpassing human performance in many tasks.",
      author: "Sarah Techson",
      date: "May 15, 2024",
      readTime: "4 min read",
      content: `
        <h2>The Future of AI is Here</h2>
        <p>According to internal documents obtained by our sources, OpenAI is preparing to launch GPT-5 in the fourth quarter of 2024. This next-generation AI model is rumored to include:</p>
        <ul>
          <li>Multimodal processing (text, images, audio, video)</li>
          <li>Dramatically improved reasoning capabilities</li>
          <li>Context windows exceeding 1 million tokens</li>
          <li>Near-instant response times</li>
        </ul>
        <p>Industry experts predict this release will revolutionize how we interact with technology, potentially making current AI assistants obsolete overnight.</p>
        <blockquote>"This isn't just an incremental improvement - it's a paradigm shift," says Dr. Alan Turington of MIT's AI Lab.</blockquote>
        <p>The leaked documents also suggest OpenAI is working on specialized versions for healthcare, legal, and creative industries.</p>
      `,
      tags: ["Artificial Intelligence", "OpenAI", "GPT-5"],
      source: "Tech Future Daily",
      social: {
        twitter: "https://twitter.com/techfuture",
        facebook: "https://facebook.com/techfuture",
        linkedin: "https://linkedin.com/company/techfuture"
      },
      related: [
        { title: "How GPT-4 Changed Everything", url: "#" },
        { title: "The Ethics of Superintelligent AI", url: "#" }
      ]
    }
  },
  {
    category: "AI",
    details: {
      title: "Google's Gemini Outperforms Humans in Creative Writing",
      image: "https://thewallet.com.pk/wp-content/uploads/2023/12/gemini.jpeg",
      description: "In blind tests, 85% of readers preferred AI-generated stories over human-written ones, marking a milestone in creative AI.",
      author: "Mark Digitalis",
      date: "May 12, 2024",
      readTime: "6 min read",
      content: `
        <h2>The Creative Singularity</h2>
        <p>Google's Gemini AI has achieved what many thought impossible - surpassing human writers in creative storytelling. In a landmark study:</p>
        <ul>
          <li>500 participants read stories without knowing the author</li>
          <li>85% preferred AI-generated content</li>
          <li>AI stories were rated more original and engaging</li>
        </ul>
        <p>The implications for publishing, marketing, and entertainment are profound. Some key findings:</p>
        <table>
          <tr>
            <th>Metric</th>
            <th>Human</th>
            <th>AI</th>
          </tr>
          <tr>
            <td>Engagement Score</td>
            <td>72%</td>
            <td>89%</td>
          </tr>
          <tr>
            <td>Originality</td>
            <td>68%</td>
            <td>92%</td>
          </tr>
        </table>
        <p>While some authors express concern, others see opportunity. "This is just another tool," says bestselling author Jane Wordwright.</p>
      `,
      tags: ["Google", "Gemini", "Creative AI"],
      source: "Digital Creativity",
      social: {
        twitter: "https://twitter.com/digitalcreativity",
        facebook: "https://facebook.com/digitalcreativity"
      },
      related: [
        { title: "AI Wins Literary Prize", url: "#" },
        { title: "The End of Human Writers?", url: "#" }
      ]
    }
  },
  
  // Gaming News
  {
    category: "Gaming",
    details: {
      title: "GTA 6 Release Date Finally Announced",
      image: "https://images.hdqwalls.com/download/aloy-from-horizon-forbidden-west-4k-8h-1600x900.jpg",
      description: "After years of speculation, Rockstar confirms GTA 6 will launch November 17, 2025 with unprecedented scale and detail.",
      author: "Alex Console",
      date: "May 10, 2024",
      readTime: "5 min read",
      content: `
        <h2>The Most Ambitious Game Ever</h2>
        <p>Rockstar Games has officially announced Grand Theft Auto VI will release on November 17, 2025. Key features revealed:</p>
        <ul>
          <li>Massive open world covering Vice City and new locations</li>
          <li>First female protagonist in series history</li>
          <li>Real-time weather and economic systems</li>
          <li>Support for VR and next-gen consoles</li>
        </ul>
        <p>The trailer, which broke YouTube records with 100M views in 24 hours, shows stunning visuals and hints at a storyline involving cryptocurrency heists.</p>
        <div class="video-container">
          <iframe src="https://www.youtube.com/embed/trailer" frameborder="0" allowfullscreen></iframe>
        </div>
        <p>Pre-orders begin June 1st, with special editions including real-world collectibles. Analysts predict this will be the biggest entertainment launch in history.</p>
      `,
      tags: ["GTA 6", "Rockstar", "Open World"],
      source: "GameSphere",
      social: {
        twitter: "https://twitter.com/gamesphere",
        instagram: "https://instagram.com/gamesphere"
      },
      related: [
        { title: "GTA 6 Trailer Breakdown", url: "#" },
        { title: "History of GTA", url: "#" }
      ]
    }
  },
  
  // Web3 News
  {
    category: "Web3",
    details: {
      title: "Ethereum 2.0 Cuts Energy Use by 99.95%",
      image: "https://invezz.com/wp-content/uploads/2021/01/ether-coins.jpg",
      description: "The long-awaited Ethereum merge successfully completes, transitioning to proof-of-stake and dramatically reducing environmental impact.",
      author: "Crypto Max",
      date: "May 8, 2024",
      readTime: "7 min read",
      content: `
        <h2>A Greener Blockchain Future</h2>
        <p>Ethereum has successfully completed its transition to proof-of-stake consensus, achieving what developers call "The Merge." The results:</p>
        <ul>
          <li>99.95% reduction in energy consumption</li>
          <li>Faster transaction times</li>
          <li>Increased security</li>
        </ul>
        <p>This technical milestone addresses one of cryptocurrency's biggest criticisms - its environmental impact. Before and after comparison:</p>
        <table>
          <tr>
            <th>Metric</th>
            <th>Pre-Merge</th>
            <th>Post-Merge</th>
          </tr>
          <tr>
            <td>Energy Use (annual)</td>
            <td>112 TWh</td>
            <td>0.056 TWh</td>
          </tr>
          <tr>
            <td>Carbon Footprint</td>
            <td>53M tons CO2</td>
            <td>26,500 tons CO2</td>
          </tr>
        </table>
        <p>"This proves blockchain can be sustainable," says Ethereum founder Vitalik Buterin. The upgrade also paves the way for future scalability improvements.</p>
      `,
      tags: ["Ethereum", "Blockchain", "Crypto"],
      source: "Decentralized Times",
      social: {
        twitter: "https://twitter.com/decentralized",
        telegram: "https://t.me/decentralized"
      },
      related: [
        { title: "What Proof-of-Stake Means for Investors", url: "#" },
        { title: "Next Steps for Ethereum", url: "#" }
      ]
    }
  },
  
  // Add more news articles as needed
];

// Function to show the news overlay
function showNewsOverlay() {
  let overlay = document.getElementById('newsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'newsOverlay';
    overlay.className = 'news-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(newsData.map(article => article.category))];

  overlay.innerHTML = `
    <div class="news-content">
      <div class="news-header">
        <h2>Nysa News</h2>
        <button class="close-news" onclick="hideNewsOverlay()">
          <i class="fas fa-times"></i>
        </button>
      </div>
      
      <div class="news-filter-container">
        <div class="news-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterNews('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="news-grid" id="newsGrid">
        ${renderNewsCards(newsData)}
      </div>
    </div>

    <div class="news-search-container">
      <div class="input-container">
        <input type="text" id="newsSearchInput" placeholder="Search for news, topics, or authors..." />
        <button class="voice-search" id="newsVoiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const searchInput = document.getElementById('newsSearchInput');
  const newsGrid = document.getElementById('newsGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performNewsSearch(newsData, query, newsGrid, filterButtons, renderNewsCards);
  }, 300);

  searchInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  searchInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performNewsSearch(newsData, searchInput.value, newsGrid, filterButtons, renderNewsCards);
    }
  });

  initializeNewsVoiceSearch(searchInput, (query) => {
    performNewsSearch(newsData, query, newsGrid, filterButtons, renderNewsCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
    .news-overlay {
      position: fixed;
      top: 60px;
      left: 0;
      right: 0;
      bottom: 60px;
      background: #1a1a1f;
      z-index: 1000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
      overflow-y: auto;
      width: 100%;
      display: flex;
      flex-direction: column;
      scrollbar-width: none;
      -ms-overflow-style: none;
    }

    .news-overlay::-webkit-scrollbar {
      display: none;
    }

    .news-overlay.active {
      opacity: 1;
      visibility: visible;
    }

    .news-content {
      flex: 1;
      width: 100%;
      max-width: 100%;
      padding: 20px;
      margin: 0 auto;
      position: relative;
    }

    .news-header {
      padding: 16px 24px;
      background: rgba(26, 26, 31, 0.95);
      position: fixed;
      width: 100%;
      top: 0;
      left: 0;
      z-index: 30;
      border-bottom: 1px solid rgba(255, 215, 0, 0.15);
      backdrop-filter: blur(12px);
      -webkit-backdrop-filter: blur(12px);
      height: 72px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }

    .news-header h2 {
      color: var(--primary-color);
      margin: 0;
      font-size: 1.75em;
      font-weight: 600;
      letter-spacing: -0.02em;
      display: flex;
      align-items: center;
      gap: 12px;
    }

    .news-header h2::before {
      content: '';
      display: inline-block;
      width: 24px;
      height: 24px;
      background: var(--primary-color);
      mask-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M19 21l-7-5-7 5V5a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2z'%3E%3C/path%3E%3C/svg%3E");
      mask-repeat: no-repeat;
      mask-position: center;
      mask-size: contain;
    }

  .close-news {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-news:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-news:active {
  transform: scale(0.95);
}

    .news-filter-container {
      position: fixed;
      top: 70px;
      left: 0;
      width: 100vw;
      max-width: 100vw;
      z-index: 10;
      background: #1a1a1f;
      margin-left: 10px;
      padding: 10px 15px;
      box-sizing: border-box;
    }

    .news-filter-buttons {
      display: flex;
      justify-content: flex-start;
      gap: 4px;
      padding-bottom: 0px;
      width: 100vw;
      margin-left: calc(-50vw + 50%);
      overflow-x: auto;
      scrollbar-width: none;
    }

    .news-filter-buttons::-webkit-scrollbar {
      height: 0px;
    }

    .filter-button {
      margin-top: 2px;
      margin-left: 2px;
      margin-right: 5px;
      flex-shrink: 0;
      background: rgba(255, 215, 0, 0.1);
      color: var(--text-light);
      border: 1px solid rgba(255, 215, 0, 0.2);
      padding: 10px 15px;
      border-radius: 25px;
      transition: all 0.3s ease;
      cursor: pointer;
      white-space: nowrap;
      font-weight: 500;
      box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    }

    .filter-button:hover {
      background: var(--primary-color);
      color: black;
      transform: scale(1.05);
      box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }

    .filter-button.active {
      background: var(--primary-color);
      color: black;
      font-weight: 700;
      box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
    }

    .news-grid {
      margin-top: 70px;
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
      gap: 20px;
      animation: fadeInUp 0.5s ease-out;
      padding-bottom: 140px;
      width: 100%;
    }

    .news-card {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 16px;
      overflow: hidden;
      transition: all 0.3s ease;
      border: 1px solid rgba(255, 255, 255, 0.1);
      cursor: pointer;
    }

    .news-card:hover {
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
      border-color: rgba(255, 215, 0, 0.3);
    }

    .news-card-image {
      width: 100%;
      height: 200px;
      object-fit: cover;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .news-card-content {
      padding: 20px;
    }

    .news-card-category {
      display: inline-block;
      background: rgba(255, 215, 0, 0.1);
      color: var(--primary-color);
      padding: 4px 12px;
      border-radius: 20px;
      font-size: 0.8em;
      font-weight: 600;
      margin-bottom: 12px;
    }

    .news-card-title {
      font-size: 1.3em;
      font-weight: 700;
      margin: 0 0 10px 0;
      color: white;
      line-height: 1.3;
    }

    .news-card-description {
      font-size: 0.95em;
      color: rgba(255, 255, 255, 0.8);
      margin: 0 0 15px 0;
      line-height: 1.5;
    }

    .news-card-meta {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.85em;
      color: rgba(255, 255, 255, 0.6);
    }

    .news-card-author {
      font-weight: 500;
    }

    .news-card-date {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    .news-search-container {
      position: fixed;
      bottom: 0px;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 255, 255, 0.1);
      display: flex;
      gap: 12px;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      flex: 1;
      position: relative;
      display: flex;
      align-items: center;
    }

    #newsSearchInput {
      flex: 1;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      width: 100%;
      transition: border-color 0.3s ease;
    }

    #newsSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i { 
      font-size: 1.5em; 
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .news-grid {
        grid-template-columns: 1fr;
        padding: 0 10px;
        margin-top: 80px;
        gap: 15px;
      }

      .news-card {
        width: 100%;
        margin: 0;
      }

      .news-card-image {
        height: 180px;
      }

      .news-content {
        padding: 10px 0;
        padding-bottom: 200px;
      }

      #newsSearchInput {
        padding: 20px 20px;
      }
    }

    @media (min-width: 769px) {
      .news-search-container {
        padding: 24px;
        bottom: 0px;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #newsSearchInput {
        height: 80px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 8px;
      }

      .voice-search i {
        font-size: 2.1em;
      }

      .news-grid {
        padding-bottom: 180px;
      }
    }
  `;
  document.head.appendChild(style);
}

function renderNewsCards(filteredData) {
  return filteredData.map(article => `
    <div class="news-card" onclick="openNewsArticle('${article.category}', '${article.details.title.replace(/'/g, "\\'")}')">
      <img src="${article.details.image}" alt="${article.details.title}" class="news-card-image">
      <div class="news-card-content">
        <span class="news-card-category">${article.category}</span>
        <h3 class="news-card-title">${article.details.title}</h3>
        <p class="news-card-description">${article.details.description}</p>
        <div class="news-card-meta">
          <span class="news-card-author">By ${article.details.author}</span>
          <span class="news-card-date">
            <i class="far fa-calendar-alt"></i> ${article.details.date} • ${article.details.readTime}
          </span>
        </div>
      </div>
    </div>
  `).join('');
}

function openNewsArticle(category, title) {
  const article = newsData.find(item => 
    item.category === category && item.details.title === title.replace(/\\'/g, "'")
  );
  
  if (!article) return;

  const articlePage = document.createElement('div');
  articlePage.className = 'news-article-page';
  articlePage.innerHTML = `
    <div class="news-article-header">
      <button class="back-button" onclick="closeNewsArticle()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <div class="header-actions">
        <button class="share-button" onclick="shareArticle('${article.details.title.replace(/'/g, "\\'")}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="article-hero">
      <div class="hero-gradient"></div>
      <img src="${article.details.image}" alt="${article.details.title}" class="hero-image">
      <div class="hero-content">
        <div class="article-category">${article.category}</div>
        <h1>${article.details.title}</h1>
        <div class="article-meta">
          <div class="author-info">
            <span>By ${article.details.author}</span>
            <span class="meta-divider">•</span>
            <span>${article.details.date}</span>
            <span class="meta-divider">•</span>
            <span>${article.details.readTime}</span>
          </div>
        </div>
      </div>
    </div>
    
    <div class="article-content-container">
      <div class="article-content">
        ${article.details.content}
      </div>
      
      <div class="article-footer">
        <div class="tags-container">
          ${article.details.tags.map(tag => `
            <span class="tag">${tag}</span>
          `).join('')}
        </div>
        
        <div class="source-info">
          <span>Source: ${article.details.source}</span>
          <div class="social-links">
            ${article.details.social?.twitter ? `<a href="${article.details.social.twitter}" target="_blank"><i class="fab fa-twitter"></i></a>` : ''}
            ${article.details.social?.facebook ? `<a href="${article.details.social.facebook}" target="_blank"><i class="fab fa-facebook-f"></i></a>` : ''}
            ${article.details.social?.linkedin ? `<a href="${article.details.social.linkedin}" target="_blank"><i class="fab fa-linkedin-in"></i></a>` : ''}
            ${article.details.social?.instagram ? `<a href="${article.details.social.instagram}" target="_blank"><i class="fab fa-instagram"></i></a>` : ''}
          </div>
        </div>
      </div>
    </div>
  `;

  document.body.appendChild(articlePage);
  addNewsArticleStyles();
  scrollToTop();
  
  // Add scroll effect for header
  const header = document.querySelector('.news-article-header');
  const hero = document.querySelector('.article-hero');
  
  if (header && hero) {
    const heroHeight = hero.offsetHeight;
    
    window.addEventListener('scroll', () => {
      if (window.scrollY > heroHeight - 100) {
        header.classList.add('scrolled');
      } else {
        header.classList.remove('scrolled');
      }
    });
  }
}

function addNewsArticleStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .news-article-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      color: #333;
      font-family: poppins;
      line-height: 1.6;
    }

    .news-article-header {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      padding: 15px 20px;
      background: transparent;
      z-index: 100;
      display: flex;
      justify-content: space-between;
      align-items: center;
      transition: all 0.3s ease;
    }

    .news-article-header.scrolled {
      background: rgba(255, 255, 255, 0.98);
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      border-bottom: 1px solid rgba(0, 0, 0, 0.05);
    }

    .back-button {
      background: rgba(0, 0, 0, 0.7);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.3s ease;
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
    }

    .news-article-header.scrolled .back-button {
      background: rgba(0, 0, 0, 0.05);
      color: #333;
    }

    .back-button:hover {
      transform: translateX(-3px);
    }

    .header-actions {
      display: flex;
      gap: 10px;
    }

    .share-button {
      background: rgba(0, 0, 0, 0.7);
      border: none;
      color: white;
      width: 40px;
      height: 40px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      transition: all 0.3s ease;
      backdrop-filter: blur(5px);
      -webkit-backdrop-filter: blur(5px);
    }

    .news-article-header.scrolled .share-button {
      background: rgba(0, 0, 0, 0.05);
      color: #333;
    }

    .share-button:hover {
      transform: scale(1.1);
    }

    .article-hero {
      position: relative;
      width: 100%;
      height: 70vh;
      min-height: 500px;
      max-height: 800px;
      overflow: hidden;
    }

    .hero-gradient {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      height: 60%;
      background: linear-gradient(to top, rgba(0,0,0,0.8) 0%, transparent 100%);
      z-index: 2;
    }

    .hero-image {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      object-fit: cover;
      z-index: 1;
    }

    .hero-content {
      position: absolute;
      bottom: 0;
      left: 0;
      width: 100%;
      padding: 40px;
      z-index: 3;
      color: white;
      max-width: 1200px;
      margin: 0 auto;
      box-sizing: border-box;
    }

    .article-category {
      display: inline-block;
      background: rgba(255, 215, 0, 0.9);
      color: #111;
      padding: 6px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      font-weight: 700;
      text-transform: uppercase;
      letter-spacing: 0.5px;
      margin-bottom: 20px;
    }

    .hero-content h1 {
      font-size: 3em;
      font-weight: 800;
      margin: 0 0 20px 0;
      line-height: 1.2;
      text-shadow: 0 2px 10px rgba(0,0,0,0.3);
    }

    .article-meta {
      display: flex;
      align-items: center;
      font-size: 0.95em;
      color: rgba(255,255,255,0.9);
    }

    .author-info {
      display: flex;
      align-items: center;
      flex-wrap: wrap;
    }

    .meta-divider {
      margin: 0 10px;
      opacity: 0.7;
    }

    .article-content-container {
      max-width: 800px;
      margin: 0 auto;
      padding: 60px 20px;
      position: relative;
      background:  #1a1a1f;
      border-radius: 30px 30px 0 0;
      margin-top: 0px;
      z-index: 5;
    }

    .article-content {
      margin: 0px;
      font-size: 1.1em;
      line-height: 1.8;
      color: #fff;
    }

    .article-content h2 {
      font-size: 1.8em;
      margin: 1.5em 0 0.8em;
      margin-top: 0px;
      font-weight: 700;
      color: #fff;
    }

    .article-content p {
      margin-bottom: 1.5em;
    }

    .article-content ul, .article-content ol {
      margin-bottom: 1.5em;
      padding-left: 1.5em;
    }

    .article-content li {
      margin-bottom: 0.5em;
    }

    .article-content blockquote {
      border-left: 4px solid #ffd700;
      padding: 15px 20px;
      margin: 2em 0;
      background: #f9f9f9;
      font-style: italic;
      color: #555;
    }

    .article-content table {
      width: 100%;
      border-collapse: collapse;
      margin: 2em 0;
      box-shadow: 0 2px 15px rgba(0,0,0,0.05);
    }

    .article-content th, .article-content td {
      padding: 12px 15px;
      text-align: left;
      border-bottom: 1px solid #eee;
    }

    .article-content th {
      background: #f5f5f5;
      font-weight: 600;
      color: #333;
    }

    .article-content tr:hover {
      background: #fafafa;
    }

    .video-container {
      position: relative;
      padding-bottom: 56.25%;
      height: 0;
      overflow: hidden;
      margin: 2em 0;
      border-radius: 8px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    }

    .video-container iframe {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      border: none;
    }

    .article-footer {
      margin-top: 60px;
    }

    .tags-container {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 40px;
    }

    .tag {
      background: #f0f0f0;
      padding: 8px 15px;
      border-radius: 20px;
      font-size: 0.85em;
      color: #555;
      transition: all 0.3s ease;
    }

    .tag:hover {
      background: #ffd700;
      color: #111;
    }

    .source-info {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 0;
      border-top: 1px solid #eee;
      border-bottom: 1px solid #eee;
      margin-bottom: 0px;
      font-size: 0.9em;
      color: #fff;
    }

    .social-links {
      display: flex;
      gap: 15px;
    }

    .social-links a {
      color: #ffd700;
      transition: all 0.3s ease;
    }

    .social-links a:hover {
      color: #fff;
      transform: scale(1.1);
    }

    /* Responsive Design */
    @media (max-width: 768px) {
      .article-hero {
        height: 60vh;
        min-height: 400px;
      }
      
      .hero-content {
        padding: 30px 20px;
      }
      
      .hero-content h1 {
        font-size: 2.2em;
      }
      
      .article-content-container {
        padding: 40px 15px;
        margin-top: -20px;
        border-radius: 20px 20px 0 0;
      }
      
      .article-content {
        font-size: 1em;
      }
      
      .article-content h2 {
        font-size: 1.5em;
      }
    }

    @media (max-width: 480px) {
      .article-hero {
        height: 50vh;
        min-height: 300px;
      }
      
      .hero-content h1 {
        font-size: 1.8em;
      }
      
      .article-meta {
        font-size: 0.85em;
      }
      
      .article-content-container {
        padding: 30px 15px;
      }
    }
  `;
  document.head.appendChild(style);
}

function closeNewsArticle() {
  const articlePage = document.querySelector('.news-article-page');
  if (articlePage) {
    articlePage.remove();
  }
}

function shareArticle(title) {
  if (navigator.share) {
    navigator.share({
      title: title,
      text: 'Check out this interesting article!',
      url: window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    // Fallback for browsers that don't support Web Share API
    const shareUrl = `https://twitter.com/intent/tweet?text=${encodeURIComponent(title)}&url=${encodeURIComponent(window.location.href)}`;
    window.open(shareUrl, '_blank');
  }
}

function openRelatedArticle(title) {
  // In a real app, you would fetch the related article
  console.log(`Opening related article: ${title}`);
  // For demo purposes, we'll just show an alert
  alert(`This would open the article: "${title}" in a real application.`);
}

function hideNewsOverlay() {
  const overlay = document.getElementById('newsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function filterNews(category) {
  const newsGrid = document.getElementById('newsGrid');
  let filteredData = newsData;

  if (category !== 'All') {
    filteredData = filteredData.filter(article => article.category === category);
  }

  newsGrid.innerHTML = renderNewsCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function performNewsSearch(data, query, gridElement, filterButtons, renderFunction) {
  if (!query) {
    // If no query, show all news
    gridElement.innerHTML = renderFunction(data);
    return;
  }

  const lowerCaseQuery = query.toLowerCase();
  const filteredData = data.filter(article => {
    return (
      article.details.title.toLowerCase().includes(lowerCaseQuery) ||
      article.details.description.toLowerCase().includes(lowerCaseQuery) ||
      article.details.author.toLowerCase().includes(lowerCaseQuery) ||
      article.category.toLowerCase().includes(lowerCaseQuery) ||
      (article.details.tags && article.details.tags.some(tag => tag.toLowerCase().includes(lowerCaseQuery)))
    );
  });

  gridElement.innerHTML = filteredData.length > 0 
    ? renderFunction(filteredData)
    : '<div class="no-results">No news articles found matching your search.</div>';
}

function initializeNewsVoiceSearch(inputElement, callback) {
  const voiceButton = document.getElementById('newsVoiceSearchButton');
  if (!voiceButton) return;

  let recognition;
  try {
    recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
    recognition.continuous = false;
    recognition.interimResults = false;

    voiceButton.addEventListener('click', () => {
      if (voiceButton.classList.contains('active')) {
        recognition.stop();
        voiceButton.classList.remove('active');
        return;
      }

      voiceButton.classList.add('active');
      recognition.start();

      recognition.onresult = (event) => {
        const transcript = event.results[0][0].transcript;
        inputElement.value = transcript;
        if (callback) callback(transcript);
        voiceButton.classList.remove('active');
      };

      recognition.onerror = (event) => {
        console.error('Voice recognition error', event.error);
        voiceButton.classList.remove('active');
      };

      recognition.onend = () => {
        voiceButton.classList.remove('active');
      };
    });
  } catch (e) {
    console.error('Voice recognition not supported', e);
    voiceButton.style.display = 'none';
  }
}

// Utility function for debouncing
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

document.addEventListener('DOMContentLoaded', function() {
  // Get the news button element
  const newsButton = document.getElementById('newsButton');
  
  // Add click event listener
  if (newsButton) {
    newsButton.addEventListener('click', function(e) {
      e.preventDefault(); // Prevent default behavior if needed
      showNewsOverlay(); // This opens the news feed
    });
  }
});

const locatorsData = [
  // Delhi
  {
    category: "Grocery",
    metropolis: "Delhi",
    details: {
      name: "Dmart",
      cardImages: [
        "https://www.projectsmonitor.com/wp-content/uploads/2021/09/@DMartOfficiall.jpg",
        "https://www.goodreturns.in/img/1200x60x675/2020/04/dmart-1525851463-1586576977.jpg"
      ],
      storeImages: [
        "https://www.projectsmonitor.com/wp-content/uploads/2021/09/@DMartOfficiall.jpg",
        "https://www.goodreturns.in/img/1200x60x675/2020/04/dmart-1525851463-1586576977.jpg",
        "https://www.dmartindia.com/static/media/fruits-vegetables.2c1e8a1e.jpg"
      ],
      description: "India's leading supermarket chain offering groceries, household essentials, and daily necessities at highly competitive prices. Known for efficient operations and value pricing.",
      address: "12, Karol Bagh, Delhi - 110005",
      timings: "Mon-Sun: 9 AM - 10 PM",
      entryFee: "Free Entry",
      mapLink: "https://maps.example.com/dmart",
      website: "https://www.dmart.in",
      social: {
        facebook: "https://facebook.com/dmart",
        twitter: "https://twitter.com/dmart",
        instagram: "https://instagram.com/dmart"
      },
      facilities: [
        { name: "Wide Selection", description: "Thousands of products across categories" },
        { name: "Great Prices", description: "Everyday low prices on all items" },
        { name: "Clean Stores", description: "Well-organized and hygienic shopping environment" }
      ],
      services: [
        {
          name: "Fortune Sunflower Oil",
          type: "product",
          image: "https://kiasumart.com/wp-content/uploads/2023/11/Fortune-1l-2-1.jpg",
          mrp: "₹220",
          price: "₹199",
          quantity: "1L",
          stock: "In Stock",
          description: "100% pure sunflower oil for healthy cooking"
        },
        {
          name: "Tata Salt",
          type: "product",
          image: "https://5.imimg.com/data5/SELLER/Default/2023/7/329417195/WP/SH/PZ/27715606/tata-namak-1000x1000.jpg",
          mrp: "₹25",
          price: "₹18",
          quantity: "1kg",
          stock: "In Stock",
          description: "Iodized salt for daily cooking"
        }
      ],
      promos: [
        { name: "Weekly Offers", description: "Special discounts every week", code: "DMART10" }
      ],
      support: {
        email: "orders@dmart.com",
        phone: "+91-1234567890",
        whatsapp: "+91-7869809022",
        primaryContact: "whatsapp" // or "email"
      },
      rating: "4.3",
      reviews: "5000+",
      established: "2002",
      tags: ["Discounts", "Essentials", "Bulk Purchase"]
    }
  },
  {
    category: "Food",
    metropolis: "Delhi",
    details: {
      name: "Burger King",
      cardImages: [
        "https://staticg.sportskeeda.com/editor/2022/12/c6e19-16716485336805-1920.jpg",
        "http://www.ourcities.in/wp-content/uploads/2020/05/Reliance-Digital-Brookefields-Plaza-Coimbatore.jpg"
      ],
      storeImages: [
        "https://www.burgerking.in/static/media/store-interior.1a9f8a4e.jpg",
        "https://www.burgerking.in/static/media/burger-closeup.2c1e8a1e.jpg",
        "https://www.burgerking.in/static/media/dining-area.3d1e8a1e.jpg"
      ],
      description: "Global fast food chain famous for flame-grilled burgers, fries and shakes. Home of the iconic Whopper sandwich with vegetarian options available.",
      address: "25, Connaught Place, Delhi - 110001",
      timings: "10 AM - 11 PM (All Days)",
      entryFee: "No Cover Charge",
      mapLink: "https://maps.example.com/burgerking",
      website: "https://www.burgerking.in",
      social: {
        facebook: "https://facebook.com/burgerking",
        twitter: "https://twitter.com/burgerking",
        instagram: "https://instagram.com/burgerking"
      },
      facilities: [
        { name: "Vegetarian Options", description: "Wide range of vegetarian meals" },
        { name: "Family Friendly", description: "Kid's meals and play areas" },
        { name: "Late Night", description: "Open till midnight" }
      ],
      services: [
        {
          name: "Whopper Meal",
          type: "menu",
          image: "https://3.bp.blogspot.com/-yXFrzno9rWw/V1Bh6DB-u5I/AAAAAAAAvgA/VEBQ0Irou8gfshSPW3KgO2v6djrE4zREgCLcB/s1600/burger-king-2-for-10-dollars-whopper-meal.jpg",
          price: "₹199",
          description: "Signature Whopper burger with fries and drink",
          variants: [
            { name: "Regular", price: "₹199" },
            { name: "Large", price: "₹249" }
          ]
        },
        {
          name: "Chicken Fries",
          type: "menu",
          image: "http://static3.businessinsider.com/image/53e8db59ecad04a96e35c05e/burger-king-is-bringing-back-chicken-fries.jpg",
          price: "₹129",
          description: "Crispy chicken strips with dipping sauce"
        }
      ],
      promos: [
        { name: "Combo Deal", description: "15% off combo meals", code: "BURGER15" }
      ],
      support: {
        email: "orders@burgerking.com",
        phone: "+91-9876543210",
        whatsapp: "+91-9876543210",
        primaryContact: "whatsapp"
      },
      rating: "4.1",
      reviews: "2500+",
      established: "2014",
      tags: ["Fast Food", "American", "Takeaway"]
    }
  },
  // Bangalore
  {
    category: "Electronics",
    metropolis: "Bangalore",
    details: {
      name: "Reliance Digital Koramangala",
      cardImages: [
        "http://www.ourcities.in/wp-content/uploads/2020/05/Reliance-Digital-Brookefields-Plaza-Coimbatore.jpg",
        "https://www.reliancedigital.in/static/media/electronics-display.2c1e8a1e.jpg"
      ],
      storeImages: [
        "https://www.reliancedigital.in/static/media/store-interior.1a9f8a4e.jpg",
        "https://www.reliancedigital.in/static/media/tv-section.3d1e8a1e.jpg",
        "https://www.reliancedigital.in/static/media/appliance-section.4d1e8a1e.jpg"
      ],
      description: "Premium electronics store in Koramangala offering latest smartphones, laptops, TVs and home appliances with expert guidance.",
      address: "5th Block, Koramangala, Bangalore - 560034",
      timings: "10 AM - 9 PM (All Days)",
      entryFee: "Free Entry",
      mapLink: "https://maps.example.com/reliancedigital-koramangala",
      website: "https://www.reliancedigital.in",
      social: {
        facebook: "https://facebook.com/reliancedigital",
        twitter: "https://twitter.com/reliancedigital",
        instagram: "https://instagram.com/reliancedigital"
      },
      facilities: [
        { name: "Demo Units", description: "Try before you buy" },
        { name: "EMI Options", description: "Flexible payment plans" },
        { name: "Gift Cards", description: "Perfect present solution" }
      ],
      services: [
        {
          name: "Samsung Galaxy S21",
          type: "product",
          image: "https://storage.comprasmartphone.com/smartphones/samsung-galaxy-s25-ultra.png",
          mrp: "₹54,999",
          price: "₹49,999",
          description: "Flagship smartphone with 120Hz AMOLED display",
          variants: [
            { name: "128GB", price: "₹49,999" },
            { name: "256GB", price: "₹54,999" }
          ]
        }
      ],
      promos: [
        { name: "Festive Season", description: "No-cost EMI available", code: "NOCOSTEMI" }
      ],
      support: {
        email: "orders@reliancedigital.com",
        phone: "+91-9876543213",
        whatsapp: "+91-9876543213",
        primaryContact: "email"
      },
      rating: "4.5",
      reviews: "2000+",
      established: "2018",
      tags: ["Electronics", "Gadgets", "Appliances"]
    }
  },
  // Additional stores can be added in the same format
];

// State management for overlays
const overlayState = {
  currentOverlay: null,
  navListeners: new Map()
};

// Global cart state
let cartState = {
  items: [],
  store: null,
  total: 0,
  discountedTotal: 0,  // Add this to track discounted total
  appliedCoupon: null, // Add this to track applied coupon
  directOrderItems: null
};

// Function to initialize all overlay event listeners
function initializeOverlaySystem() {
  // Clear existing listeners
  overlayState.navListeners.forEach((listener, element) => {
    element.removeEventListener('click', listener);
  });
  overlayState.navListeners.clear();

  // Add listeners for each nav item
  const navItems = {
    'brand': {
      handler: showLocatorsOverlay,
      type: 'locators'
    },
    'services': {
      handler: showProvidersOverlay,
      type: 'providers'
    },
    'places': {
      handler: showPlacesOverlay,
      type: 'places'
    }
  };

  Object.entries(navItems).forEach(([page, { handler, type }]) => {
    const navItem = document.querySelector(`.nav-item[data-page="${page}"]`);
    if (navItem) {
      const listener = async (e) => {
        e.preventDefault();
        
        // If there's a current overlay and it's different from the one being opened
        if (overlayState.currentOverlay && overlayState.currentOverlay !== type) {
          // Hide current overlay first
          hideCurrentOverlay();
          
          // Wait for the hide animation to complete
          await new Promise(resolve => setTimeout(resolve, 300));
        }
        
        // Update state and show new overlay
        overlayState.currentOverlay = type;
        handler();
      };
      
      navItem.addEventListener('click', listener);
      overlayState.navListeners.set(navItem, listener);
    }
  });
}

function showLocatorsOverlay() {
  let overlay = document.getElementById('locatorsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'locatorsOverlay';
    overlay.className = 'locators-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(locatorsData.map(locator => locator.category))];

  overlay.innerHTML = `
    <div class="locators-content">
      <div class="locators-header">
        <div class="header-title-container">
          <h2>Stores</h2>
          <div class="header-subtitle">Find local businesses and shopping</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleLocatorSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-locators" onclick="hideLocatorsOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="locatorSortOptionsContainer">
        <div class="sort-option" onclick="sortLocators('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortLocators('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortLocators('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="locators-filter-container">
        <div class="locators-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterLocators('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="locators-grid" id="locatorsGrid">
        ${renderLocatorCards(locatorsData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="locatorsExploreInput" placeholder="Search for stores, restaurants..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('locatorsExploreInput');
  const locatorsGrid = document.getElementById('locatorsGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  function performSearch(data, query, resultsContainer, filterButtons, renderFunction) {
    if (!query.trim()) {
      resultsContainer.innerHTML = renderFunction(data);
      resetFilterButtons();
      return;
    }

    // Parse the query to extract all relevant information
    const searchParams = parseSearchQuery(query.toLowerCase());
    
    // Filter results based on the parsed parameters
    let filteredResults = data.filter(item => {
      return isItemMatching(item, searchParams);
    });

    // Sort results based on relevance
    filteredResults.sort((a, b) => {
      return calculateRelevance(b, searchParams) - calculateRelevance(a, searchParams);
    });

    // If we're searching for specific products, show product-level matches
    if (searchParams.productName) {
      filteredResults = highlightProductMatches(filteredResults, searchParams);
    }

    resultsContainer.innerHTML = filteredResults.length > 0 
      ? renderFunction(filteredResults)
      : `<div class="no-results">No results found for "${query}"</div>`;

    // Update filter buttons based on search results
    updateFilterButtonsForSearch(filteredResults);
  }

  function updateFilterButtonsForSearch(filteredResults) {
    if (filteredResults.length === 0) {
      resetFilterButtons();
      return;
    }

    // Get unique categories from filtered results
    const resultCategories = [...new Set(filteredResults.map(item => item.category))];
    
    const filterButtons = document.querySelectorAll('.filter-button');
    
    if (resultCategories.length === 1) {
      // If all results are from one category, activate that filter button
      filterButtons.forEach(button => {
        const buttonCategory = button.getAttribute('data-category');
        if (buttonCategory === resultCategories[0]) {
          button.classList.add('active');
        } else {
          button.classList.remove('active');
        }
      });
    } else {
      // If results are from multiple categories, keep "All" active
      resetFilterButtons();
    }
  }

  function resetFilterButtons() {
    const filterButtons = document.querySelectorAll('.filter-button');
    filterButtons.forEach(button => {
      if (button.getAttribute('data-category') === 'All') {
        button.classList.add('active');
      } else {
        button.classList.remove('active');
      }
    });
  }

  function filterLocators(category) {
    const exploreInput = document.getElementById('locatorsExploreInput');
    let filteredData = locatorsData;

    // If there's a search query, we need to maintain those filtered results
    if (exploreInput && exploreInput.value.trim()) {
      const searchParams = parseSearchQuery(exploreInput.value.toLowerCase());
      filteredData = filteredData.filter(item => isItemMatching(item, searchParams));
    }

    if (category !== 'All') {
      filteredData = filteredData.filter(locator => locator.category === category);
    }

    locatorsGrid.innerHTML = renderLocatorCards(filteredData);

    const filterButtons = document.querySelectorAll('.filter-button');
    filterButtons.forEach(button => {
      if (button.getAttribute('data-category') === category) {
        button.classList.add('active');
      } else {
        button.classList.remove('active');
      }
    });
  }

  function parseSearchQuery(query) {
    // Initialize search parameters
    const params = {
      keywords: '',
      location: null,
      area: null,
      pincode: null,
      productName: null,
      priceRange: null,
      type: null,
      minPrice: null,
      maxPrice: null,
      exactPrice: null
    };

    // Extract pincode (6-digit number)
    const pincodeMatch = query.match(/\b\d{6}\b/);
    if (pincodeMatch) {
      params.pincode = pincodeMatch[0];
      query = query.replace(pincodeMatch[0], '').trim();
    }

    // Extract price-related information
    const pricePhrases = [
      { regex: /under\s*₹?\s*(\d+)/i, type: 'max', key: 'maxPrice' },
      { regex: /over\s*₹?\s*(\d+)/i, type: 'min', key: 'minPrice' },
      { regex: /around\s*₹?\s*(\d+)/i, type: 'exact', key: 'exactPrice' },
      { regex: /₹?\s*(\d+)\s*to\s*₹?\s*(\d+)/i, type: 'range', key: 'priceRange' },
      { regex: /₹?\s*(\d+)/g, type: 'exact', key: 'exactPrice' }
    ];

    for (const phrase of pricePhrases) {
      const match = query.match(phrase.regex);
      if (match) {
        if (phrase.type === 'range' && match[1] && match[2]) {
          params.minPrice = parseInt(match[1]);
          params.maxPrice = parseInt(match[2]);
        } else if (match[1]) {
          params[phrase.key] = parseInt(match[1]);
        }
        query = query.replace(phrase.regex, '').trim();
        break; // Only process one price phrase
      }
    }

    // Extract location (anything after location indicators)
    const locationIndicators = ['in', 'near', 'at', 'from', 'around', 'close to', 'within'];
    for (const indicator of locationIndicators) {
      const pattern = new RegExp(`${indicator}\\s+([^\\d]+)(?:\\s+\\d+|$)`);
      const match = query.match(pattern);
      if (match) {
        params.location = match[1].trim();
        query = query.replace(pattern, '').trim();
        break;
      }
    }

    // If no explicit location, check for area names or city names
    if (!params.location && !params.pincode) {
      const allAreas = getAllKnownAreas();
      for (const areaName of allAreas) {
        if (query.includes(areaName.toLowerCase())) {
          params.area = areaName;
          query = query.replace(areaName.toLowerCase(), '').trim();
          break;
        }
      }

      // If no area found, check if last word is a city name
      if (!params.area) {
        const allCities = [...new Set(locatorsData.map(item => item.metropolis.toLowerCase()))];
        const words = query.split(/\s+/);
        const possibleCity = words[words.length - 1];
        
        if (allCities.includes(possibleCity)) {
          params.location = possibleCity;
          query = words.slice(0, -1).join(' ').trim();
        }
      }
    }

    // Check for product-specific searches
    const productPhrases = [
      { regex: /(lowest|cheapest)\s+price\s+(.*)/i, type: 'product' },
      { regex: /best\s+(.*)\s+in/i, type: 'product' },
      { regex: /where\s+to\s+buy\s+(.*)/i, type: 'product' }
    ];

    for (const phrase of productPhrases) {
      const match = query.match(phrase.regex);
      if (match && match[1] && match[2]) {
        params.productName = match[2].trim();
        params.type = 'product';
        query = query.replace(phrase.regex, '').trim();
        break;
      }
    }

    // If no specific product phrase, look for product names in services
    if (!params.productName) {
      const allProducts = getAllProductNames();
      for (const product of allProducts) {
        if (query.includes(product.toLowerCase())) {
          params.productName = product;
          break;
        }
      }
    }

    // Check for type (product, menu, etc.)
    const typeIndicators = {
      'product': ['product', 'item', 'thing'],
      'menu': ['menu', 'food', 'dish', 'meal', 'burger', 'pizza']
    };

    for (const [type, indicators] of Object.entries(typeIndicators)) {
      for (const indicator of indicators) {
        if (query.includes(indicator)) {
          params.type = type;
          query = query.replace(indicator, '').trim();
          break;
        }
      }
      if (params.type) break;
    }

    // The remaining query becomes the keywords
    params.keywords = query.replace(/[^\w\s]/g, ' ').replace(/\s+/g, ' ').trim();

    return params;
  }

  function getAllKnownAreas() {
    // Extract all unique area names from addresses
    const areas = new Set();
    locatorsData.forEach(item => {
      const address = item.details.address;
      // Extract area names (assuming format like "12, Karol Bagh, Delhi - 110005")
      const areaMatch = address.match(/[,\-]\s*([^,\-\d]+)(?:\s*[,\-]|\s*\d|$)/);
      if (areaMatch && areaMatch[1]) {
        areas.add(areaMatch[1].trim().toLowerCase());
      }
    });
    return Array.from(areas);
  }

  function getAllProductNames() {
    const products = new Set();
    locatorsData.forEach(item => {
      if (item.details.services) {
        item.details.services.forEach(service => {
          products.add(service.name.toLowerCase());
        });
      }
    });
    return Array.from(products);
  }

  function isItemMatching(item, params) {
    // 1. Check pincode match first (most specific)
    if (params.pincode) {
      const itemPincode = extractPincode(item.details.address);
      if (!itemPincode || itemPincode !== params.pincode) {
        return false;
      }
    }
    
    // 2. Check area match (if specified)
    if (params.area) {
      const itemAddress = item.details.address.toLowerCase();
      if (!itemAddress.includes(params.area.toLowerCase())) {
        return false;
      }
    }
    
    // 3. Check location match (city level)
    if (params.location) {
      const itemLocation = item.metropolis.toLowerCase();
      if (!itemLocation.includes(params.location.toLowerCase())) {
        return false;
      }
    }

    // 4. Check price range if specified
    if (params.minPrice || params.maxPrice || params.exactPrice) {
      if (item.details.services) {
        const hasMatchingPrice = item.details.services.some(service => {
          const servicePrice = parseFloat(service.price.replace(/[^\d.]/g, ''));
          
          if (params.exactPrice) {
            return servicePrice === params.exactPrice;
          }
          
          if (params.minPrice && params.maxPrice) {
            return servicePrice >= params.minPrice && servicePrice <= params.maxPrice;
          }
          
          if (params.minPrice) {
            return servicePrice >= params.minPrice;
          }
          
          if (params.maxPrice) {
            return servicePrice <= params.maxPrice;
          }
          
          return false;
        });
        
        if (!hasMatchingPrice) {
          return false;
        }
      } else {
        return false;
      }
    }

    // 5. Check for specific product matches
    if (params.productName) {
      if (!item.details.services || !item.details.services.some(service => 
        service.name.toLowerCase().includes(params.productName.toLowerCase()))) {
        return false;
      }
    }

    // 6. Check type if specified
    if (params.type) {
      if (!item.details.services || !item.details.services.some(service => 
        service.type === params.type)) {
        return false;
      }
    }
    
    // 7. Check keyword matches if no specific product search
    if (!params.productName && params.keywords) {
      const searchTerms = params.keywords.split(/\s+/);
      
      // Check if all search terms match in any field
      return searchTerms.every(term => {
        if (term.length < 2) return true; // Ignore single letters
        
        return (
          item.details.name.toLowerCase().includes(term) ||
          item.category.toLowerCase().includes(term) ||
          (item.details.tags && item.details.tags.some(tag => 
            tag.toLowerCase().includes(term))) ||
          item.details.description.toLowerCase().includes(term)
        );
      });
    }
    
    return true;
  }

  function extractPincode(address) {
    const pincodeMatch = address.match(/\b\d{6}\b/);
    return pincodeMatch ? pincodeMatch[0] : null;
  }

  function calculateRelevance(item, params) {
    let relevance = 0;
    
    // Boost exact name matches
    if (params.keywords && item.details.name.toLowerCase().includes(params.keywords)) {
      relevance += 100;
    }
    
    // Boost category matches
    if (params.keywords && item.category.toLowerCase().includes(params.keywords)) {
      relevance += 30;
    }
    
    // Boost tag matches
    if (params.keywords && item.details.tags) {
      const tagMatches = item.details.tags.filter(tag => 
        tag.toLowerCase().includes(params.keywords)).length;
      relevance += tagMatches * 20;
    }
    
    // Boost description matches
    if (params.keywords && item.details.description.toLowerCase().includes(params.keywords)) {
      relevance += 10;
    }
    
    // Extra boost for pincode matches (most specific)
    if (params.pincode) {
      const itemPincode = extractPincode(item.details.address);
      if (itemPincode === params.pincode) {
        relevance += 150;
      }
    }
    
    // Boost for area matches
    if (params.area) {
      const itemAddress = item.details.address.toLowerCase();
      if (itemAddress.includes(params.area.toLowerCase())) {
        relevance += 120;
      }
    }
    
    // Boost for location matches
    if (params.location) {
      const itemLocation = item.metropolis.toLowerCase();
      if (itemLocation.includes(params.location.toLowerCase())) {
        relevance += 80;
      }
    }

    // Boost for product matches
    if (params.productName && item.details.services) {
      const productMatches = item.details.services.filter(service => 
        service.name.toLowerCase().includes(params.productName.toLowerCase())).length;
      relevance += productMatches * 50;
    }

    // Boost for price matches
    if ((params.minPrice || params.maxPrice || params.exactPrice) && item.details.services) {
      const priceMatches = item.details.services.filter(service => {
        const servicePrice = parseFloat(service.price.replace(/[^\d.]/g, ''));
        
        if (params.exactPrice) {
          return servicePrice === params.exactPrice;
        }
        
        if (params.minPrice && params.maxPrice) {
          return servicePrice >= params.minPrice && servicePrice <= params.maxPrice;
        }
        
        if (params.minPrice) {
          return servicePrice >= params.minPrice;
        }
        
        if (params.maxPrice) {
          return servicePrice <= params.maxPrice;
        }
        
        return false;
      }).length;
      
      relevance += priceMatches * 40;
    }

    // Boost for type matches
    if (params.type && item.details.services) {
      const typeMatches = item.details.services.filter(service => 
        service.type === params.type).length;
      relevance += typeMatches * 30;
    }
    
    // Slight boost for higher rated items
    relevance += parseFloat(item.details.rating || 0);
    
    return relevance;
  }

  function highlightProductMatches(results, params) {
    return results.map(item => {
      if (!item.details.services) return item;
      
      // Create a copy of the item to avoid modifying the original
      const itemCopy = JSON.parse(JSON.stringify(item));
      
      // Filter services to only include matching products
      if (params.productName) {
        itemCopy.details.services = itemCopy.details.services.filter(service => 
          service.name.toLowerCase().includes(params.productName.toLowerCase()));
      }
      
      // Filter by price if specified
      if (params.minPrice || params.maxPrice || params.exactPrice) {
        itemCopy.details.services = itemCopy.details.services.filter(service => {
          const servicePrice = parseFloat(service.price.replace(/[^\d.]/g, ''));
          
          if (params.exactPrice) {
            return servicePrice === params.exactPrice;
          }
          
          if (params.minPrice && params.maxPrice) {
            return servicePrice >= params.minPrice && servicePrice <= params.maxPrice;
          }
          
          if (params.minPrice) {
            return servicePrice >= params.minPrice;
          }
          
          if (params.maxPrice) {
            return servicePrice <= params.maxPrice;
          }
          
          return true;
        });
      }
      
      // Add a highlight flag to indicate this is a product-level match
      if (itemCopy.details.services.length > 0) {
        itemCopy.productMatch = true;
        itemCopy.matchedServices = itemCopy.details.services;
      }
      
      return itemCopy;
    }).filter(item => item.productMatch);
  }
     
  const debouncedSearch = debounce((query) => {
    performSearch(locatorsData, query, locatorsGrid, filterButtons, renderLocatorCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(locatorsData, exploreInput.value, locatorsGrid, filterButtons, renderLocatorCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(locatorsData, query, locatorsGrid, filterButtons, renderLocatorCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
.locators-overlay {
  font-family: poppins;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.locators-overlay::-webkit-scrollbar {
  display: none;
}

.locators-overlay.active {
  opacity: 1;
  visibility: visible;
}

.locators-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.locators-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.locators-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-locators {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-locators:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-locators:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.button-text {
  font-weight: 500;
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.locators-filter-container {
  position: fixed;
  top: 64px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.locators-filter-buttons {
  margin-top: 10px;
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.locators-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.locators-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 12px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-size: 16px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.locators-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
  width: 100%;
}

.locator-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  height: 100%;
}

.locator-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.locator-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.locator-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.locator-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.locator-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.locator-name {
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.locator-category {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.locator-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.locator-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.locator-rating .count {
  color: #666;
  font-size: 14px;
}

.locator-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 100%;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.locator-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.locator-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .locators-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .locators-grid {
    grid-template-columns: 2fr;
    padding: 0 12px;
    margin-top: 90px;
    gap: 16px;
  }
  
  .locator-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .locator-card-content {
    padding: 14px;
  }
  
  .locator-name {
    font-size: 16px;
  }
  
  .locator-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .locator-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .locator-card-content {
    padding: 12px;
  }
  
  .locator-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .locators-filter-buttons {
    margin-top: 10px;
  }
  
  .locators-filter-buttons {
    padding-bottom: 2px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .locators-grid {
    margin-top: 130px;
  }
  
  .locator-card {
    border-radius: 10px;
  }
}

.locator-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .locator-card,
  .locator-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.locator-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .locator-card {
    background: #fff;
  }
  
  .locator-name {
    color: #000;
  }
  
  .locator-category,
  .locator-description,
  .locator-rating .count {
    color: #000;
  }
  
  .tag {
    background: #000;
    color: #ddd;
  }
  
  .tag:hover {
    background: #444;
  }
  
  .locator-card-actions {
    border-top: 1px solid #333;
  }
  
  .action-button {
    color: #000;
  }
  
  .action-button i {
    color: #000;
  }
  
  .action-button:first-child {
    border-right: 1px solid #333;
  }
  
  .action-button:hover {
    background: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#locatorsExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#locatorsExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #locatorsExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #locatorsExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .locators-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .locators-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .locator-card {
    width: 100%;
    margin: 0;
  }

  .locators-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .locators-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #locatorsExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}

.no-results {
  grid-column: 1 / -1;
  text-align: center;
  padding: 40px 20px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 1.1em;
}

.no-results:before {
  content: "😕";
  font-size: 2em;
  display: block;
  margin-bottom: 15px;
}

  `;
  document.head.appendChild(style);
}

function renderLocatorCards(filteredData) {
  return filteredData.map(locator => {
    const tagsHTML = locator.details.tags && locator.details.tags.length > 0 
      ? `<div class="tag-container">
          ${locator.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    const actionButtonsHTML = `
      <a href="${locator.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
        <i class="fas fa-map-marker-alt"></i> Map
      </a>
      <button class="action-button" onclick="openStorePage('${locator.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
      <button class="action-button" onclick="showChatSupport('${locator.details.name}'); event.stopPropagation()">
        <i class="fas fa-comments"></i> Chat
      </button>
    `;
    
    return `
      <div class="locator-card" onclick="openStorePage('${locator.details.name}')">
        <div class="image-carousel">
          ${locator.details.cardImages.map((image, index) => `
            <div class="carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${locator.details.name}" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="carousel-pagination">
            ${locator.details.cardImages.map((_, index) => `
              <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        
        <div class="locator-card-content">
          <div class="locator-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="locator-name">${locator.details.name}</h3>
              <div class="locator-rating">
                <span class="star">${locator.details.rating || '4.0'} ★</span>
              </div>
            </div>
            
            <div class="locator-category">
              <span>${locator.category} • ${locator.metropolis}</span>
            </div>
            
            <div class="locator-info-grid">
              <div class="info-item">
                <div class="info-icon">
                  <i class="fas fa-map-marker-alt"></i>
                </div>
                <div class="info-content">
                  <span class="info-label">Location & Hours</span>
                  <span class="info-value">${locator.details.address}</span>
                  <span class="info-value" style="margin-top: 4px;">${locator.details.timings}</span>
                </div>
              </div>
            </div>
            
            ${tagsHTML}
          </div>
        </div>

        <div class="locator-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openStorePage(storeName) {
  const store = locatorsData.find(loc => loc.details.name === storeName);
  if (!store) return;

  // Set the current store in cart state
  cartState.store = store;

  const storePage = document.createElement('div');
  storePage.className = 'store-page';
  storePage.innerHTML = `
    <div class="store-header">
      <button class="back-button" onclick="closeStorePage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${store.details.name}</h2>
      <div class="header-actions">
        <button class="cart-button" onclick="showCart()">
          <i class="fas fa-shopping-cart"></i>
          ${cartState.items.length > 0 ? `<span class="cart-count">${cartState.items.length}</span>` : ''}
        </button>
        <button class="share-button" onclick="shareStore('${store.details.name}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="store-content">
      <!-- Hero Section with Store Image Carousel -->
      <div class="store-hero">
        <div class="store-image-carousel">
          ${store.details.storeImages.map((image, index) => `
            <div class="store-carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${store.details.name}" class="store-image" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="store-carousel-pagination">
            ${store.details.storeImages.map((_, index) => `
              <div class="store-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentStoreSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="store-carousel-control prev" onclick="changeStoreCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="store-carousel-control next" onclick="changeStoreCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        <div class="store-basic-info">
          <div class="store-rating">
            <i class="fas fa-star"></i> ${store.details.rating} (${store.details.reviews})
          </div>
          <div class="store-category">
            ${store.category}
          </div>
          <div class="store-timings">
            <i class="fas fa-clock"></i> ${store.details.timings}
          </div>
        </div>
      </div>
      
      <!-- Store Description Section -->
      <div class="store-description-section">
        <h3>About</h3>
        <p>${store.details.description}</p>
        <div class="established-since">
          <i class="fas fa-calendar-alt"></i> Established: ${store.details.established}
        </div>
      </div>
      
      <!-- Enhanced Services Section -->
      <div class="store-services-section">
        <h3>${getServicesSectionTitle(store.category)}</h3>
        <div class="services-container">
          ${renderServices(store.details.services, store.category)}
        </div>
      </div>
      
      <!-- Facilities Section -->
      <div class="store-facilities-section">
        <h3>Facilities</h3>
        <div class="facilities-grid">
          ${store.details.facilities?.map(facility => `
            <div class="facility-item">
              <div class="facility-icon">
                <i class="fas fa-${getFacilityIcon(facility.name)}"></i>
              </div>
              <div class="facility-info">
                <h4>${facility.name}</h4>
                <p>${facility.description}</p>
              </div>
            </div>
          `).join('') || '<p>No facilities information available</p>'}
        </div>
      </div>
      
      <!-- Promotions Section -->
      <div class="store-promos-section">
        <h3>Special Offers</h3>
        <div class="promos-grid">
          ${store.details.promos?.map(promo => `
            <div class="promo-item">
              <div class="promo-info">
                <h4>${promo.name}</h4>
                <p>${promo.description}</p>
              </div>
              <div class="promo-code">
                <span>${promo.code}</span>
                <button onclick="copyToClipboard('${promo.code}')">Copy</button>
              </div>
            </div>
          `).join('') || '<p>No current promotions</p>'}
        </div>
      </div>
      
      <!-- Contact Section -->
      <div class="store-contact-section">
        <h3>Contact</h3>
        <div class="contact-methods">
          ${store.details.support?.phone ? `
            <a href="tel:${store.details.support.phone}" class="contact-item">
              <i class="fas fa-phone"></i> ${store.details.support.phone}
            </a>
          ` : ''}
          ${store.details.support?.whatsapp ? `
            <a href="https://wa.me/${store.details.support.whatsapp}" target="_blank" class="contact-item">
              <i class="fab fa-whatsapp"></i> WhatsApp
            </a>
          ` : ''}
          ${store.details.support?.email ? `
            <a href="mailto:${store.details.support.email}" class="contact-item">
              <i class="fas fa-envelope"></i> ${store.details.support.email}
            </a>
          ` : ''}
          ${store.details.website ? `
            <a href="${store.details.website}" target="_blank" class="contact-item">
              <i class="fas fa-globe"></i> Visit Website
            </a>
          ` : ''}
        </div>
        
        <div class="social-links">
          ${store.details.social?.facebook ? `
            <a href="${store.details.social.facebook}" target="_blank" class="social-link">
              <i class="fab fa-facebook-f"></i>
            </a>
          ` : ''}
          ${store.details.social?.twitter ? `
            <a href="${store.details.social.twitter}" target="_blank" class="social-link">
              <i class="fab fa-twitter"></i>
            </a>
          ` : ''}
          ${store.details.social?.instagram ? `
            <a href="${store.details.social.instagram}" target="_blank" class="social-link">
              <i class="fab fa-instagram"></i>
            </a>
          ` : ''}
        </div>
      </div>
      
      <!-- Store Action Buttons -->
      <div class="store-actions">
        <a href="${store.details.mapLink}" target="_blank" class="primary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
        <button class="secondary-action" onclick="showChatSupport('${store.details.name}')">
          <i class="fas fa-comment-alt"></i> Contact Store
        </button>
      </div>
    </div>
    
    <!-- Cart Overlay -->
    <div class="cart-overlay" id="cartOverlay">
      <div class="cart-container">
        <div class="cart-header">
          <h3>Your Cart</h3>
          <button class="close-cart" onclick="hideCart()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        
        <div class="cart-items" id="cartItems">
          ${renderCartItems()}
        </div>
        
        <div class="cart-summary">
          <div class="cart-total">
            <span>Total:</span>
            <span id="cartTotalAmount">₹${cartState.total.toFixed(2)}</span>
          </div>
          <div class="cart-actions">
            <button class="clear-cart" onclick="clearCart()">
              <i class="fas fa-trash"></i> Clear Cart
            </button>
            <button class="checkout-btn" onclick="showCheckoutForm()">
              <i class="fas fa-credit-card"></i> Checkout
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Order Form Overlay -->
    <div class="order-form-overlay" id="orderFormOverlay">
      <div class="order-form-container">
        <button class="close-order-form" onclick="closeOrderForm()">
          <i class="fas fa-times"></i>
        </button>
        <h3>Checkout</h3>
        <form id="orderForm">
          <div class="form-group">
            <label for="customerName">Full Name</label>
            <input type="text" id="customerName" required>
          </div>
          <div class="form-group">
            <label for="customerPhone">Phone Number</label>
            <input type="tel" id="customerPhone" required>
          </div>
          <div class="form-group">
            <label for="customerAddress">Delivery Address</label>
            <textarea id="customerAddress" rows="3" required></textarea>
          </div>
          <div class="form-group">
            <label for="deliveryTime">Preferred Delivery/Pickup Time</label>
            <input type="datetime-local" id="deliveryTime" required>
          </div>
          <div class="form-group">
            <label for="specialInstructions">Special Instructions</label>
            <textarea id="specialInstructions" rows="2"></textarea>
          </div>
          <div class="form-group">
            <label for="paymentMethod">Payment Method</label>
            <select id="paymentMethod" required>
              <option value="">Select payment method</option>
              <option value="Cash on Delivery">Cash on Delivery</option>
              <option value="Online Payment">Online Payment</option>
              <option value="Card on Delivery">Card on Delivery</option>
            </select>
          </div>
          
          <div class="order-summary">
            <h4>Order Summary</h4>
            <div id="orderSummaryItems">
              ${renderOrderSummaryItems()}
            </div>
            <div class="order-total">
              <span>Total:</span>
              <span id="orderTotal">₹${cartState.total.toFixed(2)}</span>
            </div>
          </div>
          
          <div class="coupon-section">
            <div class="form-group">
              <label for="couponCode">Coupon Code</label>
              <div class="coupon-input">
                <input type="text" id="couponCode" placeholder="Enter coupon code">
                <button type="button" class="apply-coupon" onclick="applyCoupon()">Apply</button>
              </div>
              <div id="couponMessage" class="coupon-message"></div>
            </div>
          </div>
          
          <button type="submit" class="submit-order">
            <i class="fas fa-paper-plane"></i> Place Order
          </button>
        </form>
      </div>
    </div>
    
    <!-- Order Confirmation Modal -->
    <div class="confirmation-modal" id="confirmationModal">
      <div class="confirmation-content">
        <div class="confirmation-icon">
          <i class="fas fa-check-circle"></i>
        </div>
        <h3>Order Confirmed!</h3>
        <p id="confirmationMessage">Your order has been placed successfully.</p>
        <div class="confirmation-actions">
          <button class="view-receipt" onclick="viewOrderReceipt()">
            <i class="fas fa-receipt"></i> View Receipt
          </button>
          <button class="close-confirmation" onclick="closeConfirmationModal()">
            <i class="fas fa-times"></i> Close
          </button>
        </div>
      </div>
    </div>
    
    <!-- Toast Notification -->
    <div class="toast"></div>
  `;

  document.body.appendChild(storePage);
  addStorePageStyles();
  initializeTabs();
  initializeOrderForm(store);
  initializeStoreCarousel();
  initializeSearch();
  scrollToTop();
}

// Helper functions for the store page
function getServicesSectionTitle(category) {
  const titles = {
    "Grocery": "Products",
    "Food": "Menu",
    "Electronics": "Products",
    "Medicines": "Medicines & Health Products",
    "Laundry": "Services",
    "Fashion": "Collections",
    "Furniture": "Products"
  };
  return titles[category] || "Products & Services";
}

function renderServices(services, category) {
  if (!services || services.length === 0) {
    return '<p class="no-services">No services information available</p>';
  }

  return services.map(service => {
    if (category === "Grocery" || category === "Electronics") {
      return `
        <div class="product-item" data-name="${service.name}" data-price="${service.price.replace('₹','')}">
          <div class="product-image">
            <img src="${service.image}" alt="${service.name}">
          </div>
          <div class="product-details">
            <h4>${service.name}</h4>
            <p class="product-description">${service.description}</p>
            <div class="product-pricing">
              <span class="product-price">${service.price}</span>
              ${service.mrp ? `<span class="product-mrp"><del>${service.mrp}</del></span>` : ''}
            </div>
            <div class="product-meta">
              ${service.quantity ? `<span class="product-quantity"><i class="fas fa-weight"></i> ${service.quantity}</span>` : ''}
              <span class="product-stock ${service.stock === 'In Stock' ? 'in-stock' : 'out-of-stock'}">
                <i class="fas ${service.stock === 'In Stock' ? 'fa-check-circle' : 'fa-times-circle'}"></i> ${service.stock}
              </span>
            </div>
            ${renderVariants(service.variants)}
            <div class="product-actions">
              <button class="add-to-cart" onclick="addToCart(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-cart-plus"></i> Add to Cart
              </button>
              <button class="buy-now" onclick="showOrderForm(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-bolt"></i> Buy Now
              </button>
            </div>
          </div>
        </div>
      `;
    } else if (category === "Food") {
      return `
        <div class="fare-item" data-name="${service.name}" data-price="${service.price.replace('₹','')}">
          <div class="fare-image">
            <img src="${service.image}" alt="${service.name}">
          </div>
          <div class="fare-details">
            <h4>${service.name}</h4>
            <p class="fare-description">${service.description}</p>
            ${renderVariants(service.variants)}
            <div class="fare-actions">
              <button class="add-to-cart" onclick="addToCart(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-cart-plus"></i> Add to Cart
              </button>
              <button class="order-now" onclick="showOrderForm(event, '${service.name}', ${service.price.replace('₹','')})">
                <i class="fas fa-utensils"></i> Order Now
              </button>
            </div>
          </div>
          <div class="fare-price">
            ${service.price}
          </div>
        </div>
      `;
    } else {
      // Default service item for other categories
      return `
        <div class="service-item">
          <h4>${service.name}</h4>
          <div class="service-price">${service.price}</div>
          <p>${service.description}</p>
          <button class="book-service" onclick="showOrderForm(event, '${service.name}', ${service.price.replace('₹','')})">
            <i class="fas fa-calendar-check"></i> Book Now
          </button>
        </div>
      `;
    }
  }).join('');
}

function renderVariants(variants) {
  if (!variants || variants.length === 0) return '';
  
  return `
    <div class="variants-container">
      <select class="variant-select" onchange="updatePrice(this)">
        ${variants.map(variant => `
          <option value="${variant.price.replace('₹','')}" data-name="${variant.name}">
            ${variant.name} - ${variant.price}
          </option>
        `).join('')}
      </select>
    </div>
  `;
}

function initializeOrderForm() {
  const orderForm = document.getElementById('orderForm');
  if (!orderForm) return;

  orderForm.addEventListener('submit', function(e) {
    e.preventDefault();
    
    // Get form values
    const customerName = document.getElementById('customerName').value;
    const customerPhone = document.getElementById('customerPhone').value;
    const customerAddress = document.getElementById('customerAddress').value;
    const deliveryTime = document.getElementById('deliveryTime').value;
    const specialInstructions = document.getElementById('specialInstructions').value;
    const paymentMethod = document.getElementById('paymentMethod').value;
    
    // Format delivery time
    let formattedDeliveryTime = '';
    if (deliveryTime) {
      const deliveryDate = new Date(deliveryTime);
      formattedDeliveryTime = deliveryDate.toLocaleString('en-IN', {
        weekday: 'short',
        day: 'numeric',
        month: 'short',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      });
    }
    
    // Determine which items to include in the order
    const orderItems = cartState.items.length > 0 ? 
      cartState.items : 
      (cartState.directOrderItems || []);
    
    // Calculate subtotal (before discount)
    const subtotal = orderItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    
    // Use discounted total if coupon was applied, otherwise use regular total
    const orderTotal = cartState.appliedCoupon ? 
      cartState.discountedTotal : 
      subtotal;
    
    // Create order details message
    let orderDetails = `*NEW ORDER NOTIFICATION*\n\n`;
    orderDetails += `*Store:* ${cartState.store.details.name}\n`;
    orderDetails += `*Order Time:* ${new Date().toLocaleString('en-IN')}\n\n`;
    
    orderDetails += `*Customer Details:*\n`;
    orderDetails += `👤 Name: ${customerName}\n`;
    orderDetails += `📞 Phone: ${customerPhone}\n`;
    orderDetails += `🏠 Address: ${customerAddress}\n`;
    if (formattedDeliveryTime) {
      orderDetails += `⏰ Delivery Time: ${formattedDeliveryTime}\n`;
    }
    if (specialInstructions) {
      orderDetails += `📝 Special Instructions: ${specialInstructions}\n`;
    }
    orderDetails += `💳 Payment Method: ${paymentMethod}\n\n`;
    
    orderDetails += `*Order Items:*\n`;
    orderDetails += orderItems.map(item => 
      `- ${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}: ₹${(item.price * item.quantity).toFixed(2)}`
    ).join('\n');
    
    // Add discount information if coupon was applied
    if (cartState.appliedCoupon) {
      orderDetails += `\n\n*Discount Applied:*\n`;
      orderDetails += `Coupon Code: ${cartState.appliedCoupon.code}\n`;
      orderDetails += `Discount: ${(cartState.appliedCoupon.discount * 100)}%\n`;
      orderDetails += `Discount Amount: -₹${cartState.appliedCoupon.discountAmount.toFixed(2)}\n`;
      orderDetails += `\n*Subtotal:* ₹${subtotal.toFixed(2)}`;
      orderDetails += `\n*Discount:* -₹${cartState.appliedCoupon.discountAmount.toFixed(2)}`;
    }
    
    orderDetails += `\n*Order Total:* ₹${orderTotal.toFixed(2)}`;
    
    // Send order based on store's preferred contact method
    if (cartState.store.details.support.primaryContact === "whatsapp" && cartState.store.details.support.whatsapp) {
      const whatsappUrl = `https://wa.me/${cartState.store.details.support.whatsapp}?text=${encodeURIComponent(orderDetails)}`;
      window.open(whatsappUrl, '_blank');
    } else if (cartState.store.details.support.primaryContact === "email" && cartState.store.details.support.email) {
      const subject = `New Order from ${customerName} - ${cartState.store.details.name}`;
      const body = orderDetails.replace(/\*/g, '');
      const mailtoUrl = `mailto:${cartState.store.details.support.email}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
      window.location.href = mailtoUrl;
    }
    
    // Show confirmation with the correct total
    showOrderConfirmation(customerName, orderTotal.toFixed(2));
    
    // Clear cart/direct order and close form
    clearCart();
    delete cartState.directOrderItems;
    closeOrderForm();
  });
}

function applyCoupon() {
  const couponCode = document.getElementById('couponCode').value;
  const couponMessage = document.getElementById('couponMessage');
  
  if (!couponCode) {
    couponMessage.textContent = 'Please enter a coupon code';
    couponMessage.className = 'coupon-message error';
    return;
  }
  
  // Get the current total (either from cart or direct order)
  let currentTotal;
  if (cartState.items.length > 0) {
    currentTotal = cartState.total;
  } else if (cartState.directOrderItems) {
    currentTotal = cartState.directOrderItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  } else {
    couponMessage.textContent = 'No items to apply coupon to';
    couponMessage.className = 'coupon-message error';
    return;
  }

  // Valid coupons with their discount percentages
  const validCoupons = {
    'SAVE10': { discount: 0.1, message: '10% discount applied!' },
    'FREESHIP': { discount: 0, message: 'Free shipping applied!' },
    'WELCOME20': { discount: 0.2, message: '20% discount applied!' }
  };
  
  if (validCoupons[couponCode]) {
    const coupon = validCoupons[couponCode];
    const discountAmount = currentTotal * coupon.discount;
    const newTotal = currentTotal - discountAmount;
    
    // Update cart state with discount information
    cartState.discountedTotal = newTotal;
    cartState.appliedCoupon = {
      code: couponCode,
      discount: coupon.discount,
      discountAmount: discountAmount
    };
    
    // Update UI
    couponMessage.textContent = coupon.message;
    couponMessage.className = 'coupon-message success';
    
    // Update order total display
    document.getElementById('orderTotal').textContent = `₹${newTotal.toFixed(2)}`;
  } else {
    couponMessage.textContent = 'Invalid coupon code';
    couponMessage.className = 'coupon-message error';
    // Reset discount if invalid coupon
    cartState.discountedTotal = currentTotal;
    cartState.appliedCoupon = null;
    document.getElementById('orderTotal').textContent = `₹${currentTotal.toFixed(2)}`;
  }
}

// Order Confirmation Functions
function showOrderConfirmation(customerName, orderTotal) {
  const confirmationModal = document.getElementById('confirmationModal');
  const confirmationMessage = document.getElementById('confirmationMessage');
  
  if (confirmationModal && confirmationMessage) {
    confirmationMessage.textContent = `Thank you, ${customerName}! Your order of ₹${orderTotal} has been placed successfully.`;
    confirmationModal.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function closeConfirmationModal() {
  const confirmationModal = document.getElementById('confirmationModal');
  if (confirmationModal) {
    confirmationModal.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function viewOrderReceipt() {
  // In a real app, this would generate or show a detailed receipt
  alert('Receipt would be displayed here in a real implementation.');
  closeConfirmationModal();
}

// Helper Functions
function showToast(message) {
  let toast = document.querySelector('.toast');
  if (!toast) {
    toast = document.createElement('div');
    toast.className = 'toast';
    document.body.appendChild(toast);
  }
  
  toast.textContent = message;
  toast.classList.add('show');
  
  setTimeout(() => {
    toast.classList.remove('show');
  }, 3000);
}

let cartItems = [];

// Cart Management Functions
function renderCartItems() {
  if (cartState.items.length === 0) {
    return `
      <div class="empty-cart">
        <i class="fas fa-shopping-cart"></i>
        <p>Your cart is empty</p>
        <button onclick="hideCart()">Continue Shopping</button>
      </div>
    `;
  }

  return cartState.items.map((item, index) => `
    <div class="cart-item" data-index="${index}">
      <div class="cart-item-image">
        <img src="${item.image}" alt="${item.name}">
      </div>
      <div class="cart-item-details">
        <h4>${item.name}</h4>
        ${item.variant ? `<p class="variant">${item.variant}</p>` : ''}
        <div class="cart-item-price">₹${item.price.toFixed(2)}</div>
        <div class="cart-item-quantity">
          <button class="quantity-btn minus" onclick="updateQuantity(${index}, -1)">−</button>
          <span class="quantity">${item.quantity}</span>
          <button class="quantity-btn plus" onclick="updateQuantity(${index}, 1)">+</button>
        </div>
      </div>
      <button class="remove-item" onclick="removeFromCart(${index})">
        <i class="fas fa-times"></i>
      </button>
    </div>
  `).join('');
}

function renderOrderSummaryItems() {
  return cartState.items.map(item => `
    <div class="order-item">
      <span>${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}</span>
      <span>₹${(item.price * item.quantity).toFixed(2)}</span>
    </div>
  `).join('');
}

function addToCart(event, itemName, basePrice) {
  event.stopPropagation();
  
  const itemElement = event.target.closest('[data-name]');
  const variantSelect = itemElement?.querySelector('.variant-select');
  
  let variantName = null;
  let price = parseFloat(basePrice);
  let image = itemElement.querySelector('img')?.src || '';
  
  // Handle variants if they exist
  if (variantSelect) {
    const selectedOption = variantSelect.options[variantSelect.selectedIndex];
    variantName = selectedOption.dataset.name;
    price = parseFloat(selectedOption.value);
  }
  
  // Check if item already exists in cart
  const existingItemIndex = cartState.items.findIndex(item => 
    item.name === itemName && item.variant === variantName
  );
  
  if (existingItemIndex >= 0) {
    // Update quantity if item exists
    cartState.items[existingItemIndex].quantity += 1;
  } else {
    // Add new item to cart
    cartState.items.push({
      name: itemName,
      variant: variantName,
      price: price,
      quantity: 1,
      image: image
    });
  }
  
  // Update cart total
  updateCartTotal();
  
  // Update cart UI
  updateCartUI();
  
  // Show success message
  showToast(`${itemName} ${variantName ? `(${variantName})` : ''} added to cart`);
}

function removeFromCart(index) {
  cartState.items.splice(index, 1);
  updateCartTotal();
  updateCartUI();
  
  if (cartState.items.length === 0) {
    hideCart();
  }
}

function updateQuantity(index, change) {
  const newQuantity = cartState.items[index].quantity + change;
  
  if (newQuantity < 1) {
    removeFromCart(index);
    return;
  }
  
  cartState.items[index].quantity = newQuantity;
  updateCartTotal();
  updateCartUI();
}

function clearCart() {
  cartState.items = [];
  cartState.total = 0;
  cartState.discountedTotal = 0;
  cartState.appliedCoupon = null;
  updateCartUI();
  hideCart();
}

function updateCartTotal() {
  cartState.total = cartState.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  // Reapply coupon discount if one was applied
  if (cartState.appliedCoupon) {
    const discountAmount = cartState.total * cartState.appliedCoupon.discount;
    cartState.discountedTotal = cartState.total - discountAmount;
    cartState.appliedCoupon.discountAmount = discountAmount;
  } else {
    cartState.discountedTotal = cartState.total;
  }
}

function updateCartUI() {
  const cartOverlay = document.getElementById('cartOverlay');
  if (!cartOverlay) return;
  
  // Update cart items list
  const cartItemsContainer = document.getElementById('cartItems');
  if (cartItemsContainer) {
    cartItemsContainer.innerHTML = renderCartItems();
  }
  
  // Update cart total (show discounted total if available)
  const cartTotalElement = document.getElementById('cartTotalAmount');
  if (cartTotalElement) {
    const displayTotal = cartState.appliedCoupon ? cartState.discountedTotal : cartState.total;
    cartTotalElement.textContent = `₹${displayTotal.toFixed(2)}`;
  }
  
  // Update cart count in header
  const cartButton = document.querySelector('.cart-button');
  if (cartButton) {
    const cartCount = cartButton.querySelector('.cart-count');
    
    if (cartState.items.length > 0) {
      if (!cartCount) {
        const countElement = document.createElement('span');
        countElement.className = 'cart-count';
        countElement.textContent = cartState.items.length;
        cartButton.appendChild(countElement);
      } else {
        cartCount.textContent = cartState.items.length;
      }
    } else if (cartCount) {
      cartCount.remove();
    }
  }
  
  // Update order summary in checkout form
  const orderSummaryItems = document.getElementById('orderSummaryItems');
  const orderTotal = document.getElementById('orderTotal');
  
  if (orderSummaryItems && orderTotal) {
    orderSummaryItems.innerHTML = renderOrderSummaryItems();
    const displayTotal = cartState.appliedCoupon ? cartState.discountedTotal : cartState.total;
    orderTotal.textContent = `₹${displayTotal.toFixed(2)}`;
  }
}

// Cart Visibility Functions
function showCart() {
  const cartOverlay = document.getElementById('cartOverlay');
  if (cartOverlay) {
    cartOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function hideCart() {
  const cartOverlay = document.getElementById('cartOverlay');
  if (cartOverlay) {
    cartOverlay.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function showCheckoutForm() {
  if (cartState.items.length === 0) {
    showToast('Your cart is empty');
    return;
  }
  
  hideCart();
  
  const orderFormOverlay = document.getElementById('orderFormOverlay');
  if (orderFormOverlay) {
    orderFormOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function showOrderForm(event, itemName, itemPrice) {
  event.stopPropagation();
  const itemElement = event.target.closest('[data-name]');
  const variantSelect = itemElement?.querySelector('.variant-select');
  
  let items = [];
  if (variantSelect) {
    const selectedOption = variantSelect.options[variantSelect.selectedIndex];
    items.push({
      name: `${itemName} (${selectedOption.dataset.name})`,
      price: parseFloat(selectedOption.value),
      quantity: 1,
      image: itemElement.querySelector('img')?.src || ''
    });
  } else {
    items.push({
      name: itemName,
      price: itemPrice,
      quantity: 1,
      image: itemElement.querySelector('img')?.src || ''
    });
  }

  // Store the direct order items temporarily
  cartState.directOrderItems = items;
  // Reset any previous coupon
  cartState.appliedCoupon = null;
  cartState.discountedTotal = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  openOrderForm(items);
}

function openOrderForm(items) {
  const orderFormOverlay = document.getElementById('orderFormOverlay') || 
    document.querySelector('.order-form-overlay');
  
  if (!orderFormOverlay) return;

  orderFormOverlay.classList.add('active');
  
  const orderSummary = orderFormOverlay.querySelector('#orderSummaryItems');
  let total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  
  // Reset coupon message
  const couponMessage = orderFormOverlay.querySelector('#couponMessage');
  if (couponMessage) {
    couponMessage.textContent = '';
    couponMessage.className = 'coupon-message';
  }
  
  // Reset coupon code input
  const couponCodeInput = orderFormOverlay.querySelector('#couponCode');
  if (couponCodeInput) {
    couponCodeInput.value = '';
  }
  
  orderSummary.innerHTML = items.map(item => {
    return `
      <div class="order-item">
        <span>${item.name} × ${item.quantity}</span>
        <span>₹${(item.price * item.quantity).toFixed(2)}</span>
      </div>
    `;
  }).join('');
  
  orderFormOverlay.querySelector('#orderTotal').textContent = `₹${total.toFixed(2)}`;
}

function closeOrderForm() {
  const orderFormOverlay = document.getElementById('orderFormOverlay');
  if (orderFormOverlay) {
    orderFormOverlay.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function closeStorePage() {
  const storePage = document.querySelector('.store-page');
  if (storePage) {
    storePage.remove();
  }
}

function addStorePageStyles() {
  const style = document.createElement('style');
  style.textContent = `
.store-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #1a1a1f;
  z-index: 2000;
  overflow-y: auto;
  color: #fff;
  font-family: poppins;
}

.store-header {
  position: sticky;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  z-index: 10;
  backdrop-filter: blur(12px);
}

.back-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.5em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.store-header h2 {
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  text-align: center;
  flex: 1;
  padding: 0 16px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header-actions {
  display: flex;
  gap: 8px;
}

.share-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.share-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.store-content {
  padding: 0 0 100px;
  width: 100%;
  margin: 0 auto;
}

.store-hero {
  position: relative;
  padding: 20px;
}

.store-image-container {
  width: 100%;
  height: 200px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
}

.store-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.store-image:hover {
  transform: scale(1.02);
}

.store-basic-info {
  display: flex;
  gap: 12px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.store-rating,
.store-category,
.store-timings {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 0.9em;
  font-weight: 500;
}

.store-rating {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
}

.store-category {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.store-timings {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.established-since {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-top: 12px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.store-description-section {
  padding: 0 20px 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-description-section h3 {
  margin: 0 0 12px;
  font-size: 1.2em;
  font-weight: 600;
}

.store-description-section p {
  margin: 0;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
}

.store-services-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-services-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.services-grid {
  display: grid;
  gap: 16px;
}

.service-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.service-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.service-item h4 {
  margin: 0 0 8px;
  font-size: 1.1em;
  font-weight: 600;
}

.service-price {
  color: #ffd700;
  font-weight: 600;
  margin-bottom: 8px;
}

.service-item p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.store-facilities-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-facilities-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.facilities-grid {
  display: grid;
  gap: 16px;
}

.facility-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.facility-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.facility-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-size: 1.2em;
}

.facility-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.facility-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.store-promos-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-promos-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.promos-grid {
  display: grid;
  gap: 16px;
}

.promo-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.promo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.promo-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.promo-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.promo-code {
  display: flex;
  align-items: center;
  gap: 8px;
}

.promo-code span {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  font-weight: 600;
}

.promo-code button {
  background: rgba(255, 215, 0, 0.1);
  border: none;
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.promo-code button:hover {
  background: rgba(255, 215, 0, 0.2);
}

.store-contact-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.store-contact-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.contact-methods {
  display: grid;
  gap: 12px;
  margin-bottom: 20px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  color: #fff;
  text-decoration: none;
  transition: background 0.3s ease;
}

.contact-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.social-links {
  display: flex;
  gap: 16px;
  margin-top: 20px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
  color: #fff;
  font-size: 1.2em;
  transition: transform 0.3s ease, background 0.3s ease;
}

.social-link:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.1);
}

.store-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

@media (min-width: 768px) {
  .store-header {
    padding: 20px 30px;
  }
  
  .store-header h2 {
    font-size: 1.4em;
  }
  
  .store-content {
    max-width: 90%;
    margin: 0 auto;
  }
  
  .store-hero {
    padding: 30px;
  }
  
  .store-image-container {
    height: 280px;
    border-radius: 20px;
  }
  
  .store-basic-info {
    gap: 16px;
    margin-top: 24px;
  }
  
  .store-rating,
  .store-category,
  .store-timings {
    padding: 10px 16px;
    font-size: 1em;
  }
  
  .store-description-section,
  .store-services-section,
  .store-facilities-section,
  .store-promos-section,
  .store-contact-section {
    padding: 0 30px 30px;
  }
  
  .store-description-section h3,
  .store-services-section h3,
  .store-facilities-section h3,
  .store-promos-section h3,
  .store-contact-section h3 {
    font-size: 1.3em;
    margin-bottom: 16px;
  }
  
  .store-description-section p {
    font-size: 1em;
    line-height: 1.6;
  }
  
  .services-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .facilities-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .store-actions {
    padding: 20px 30px;
    max-width: 90%;
    margin: 0 auto;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 12px 12px 0 0;
  }
  
  .primary-action,
  .secondary-action {
    padding: 14px;
    font-size: 1em;
  }
}

@media (min-width: 1024px) {
  .store-content {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 0 120px;
  }
  
  .store-header {
    padding: 22px 32px;
  }
  
  .store-header h2 {
    font-size: 1.5em;
  }
  
  .store-hero {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    padding: 30px;
    align-items: center;
  }
  
  .store-image-container {
    height: 350px;
    border-radius: 20px;
  }
  
  .store-hero-content {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .store-name {
    font-size: 1.8em;
    font-weight: 600;
    margin: 0;
  }
  
  .store-basic-info {
    margin-top: 0;
  }
  
  .store-description-section {
    padding: 0 30px 30px;
    max-width: 85%;
    margin: 0 auto;
  }
  
  .store-description-section h3 {
    font-size: 1.5em;
  }
  
  .store-description-section p {
    font-size: 1.05em;
    line-height: 1.7;
  }
  
  .store-services-section,
  .store-facilities-section,
  .store-promos-section,
  .store-contact-section {
    padding: 30px;
  }
  
  .store-services-section h3,
  .store-facilities-section h3,
  .store-promos-section h3,
  .store-contact-section h3 {
    font-size: 1.5em;
  }
  
  .services-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .social-links {
    gap: 18px;
  }
  
  .social-link {
    width: 44px;
    height: 44px;
    font-size: 1.3em;
  }
  
  .store-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
}

@media (min-width: 1440px) {
  .store-content {
    max-width: 1000px;
  }
  
  .store-hero {
    grid-template-columns: 3fr 2fr;
  }
  
  .store-image-container {
    height: 400px;
  }
  
  .store-name {
    font-size: 2em;
  }
  
  .store-description-section {
    max-width: 80%;
  }
  
  .services-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .store-actions {
    max-width: 800px;
    left: 50%;
    transform: translateX(-50%);
  }
}

.store-image-carousel {
  position: relative;
  width: 100%;
  height: 280px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  margin-bottom: 16px;
}

.store-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.store-carousel-item.active {
  opacity: 1;
}

.store-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.store-carousel-item.active img:hover {
  transform: scale(1.02);
}

.store-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.7;
  transition: all 0.3s ease;
  z-index: 5;
}

.store-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
  opacity: 1;
  transform: translateY(-50%) scale(1.1);
}

.store-carousel-control.prev {
  left: 16px;
}

.store-carousel-control.next {
  right: 16px;
}

.store-carousel-pagination {
  position: absolute;
  bottom: 16px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.store-pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
}

.store-pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
}

@media (min-width: 768px) {
  .store-image-carousel {
    height: 350px;
    border-radius: 20px;
  }
  
  .store-carousel-control {
    width: 48px;
    height: 48px;
    font-size: 20px;
  }
}

@media (min-width: 1024px) {
  .store-image-carousel {
    height: 400px;
  }
}

    .services-container {
      display: grid;
      gap: 20px;
    }
    
    .product-item {
      font-family: poppins;
      display: flex;
      background: #fff;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      transition: transform 0.3s ease;
    }
    
    .product-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 16px rgba(0,0,0,0.15);
    }

     .fare-item {
      font-family: poppins;
      display: flex;
      background: #FFD700;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      transition: transform 0.3s ease;
    }

     .fare-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 8px 16px rgba(0,0,0,0.15);
      background: #FFD700;
    }
    
    .product-image, .fare-image {
      width: 150px;
      height: 150px;
      flex-shrink: 0;
    }
    
    .product-image img, .fare-image img {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    
    .product-details, .fare-details {
      flex: 1;
      padding: 16px;
    }
    
    .product-details h4, .fare-details h4 {
      margin: 0 0 8px;
      color: #000;
      font-size: 1.1em;
    }
    
    .product-description, .fare-description {
      color: #000;
      font-size: 0.9em;
      margin: 0 0 12px;
    }
    
    .product-pricing {
      display: flex;
      align-items: center;
      gap: 10px;
      margin-bottom: 8px;
    }
    
    .product-price {
      font-weight: bold;
      color: #2e7d32;
      font-size: 1.1em;
    }
    
    .product-mrp {
      color: #999;
      font-size: 0.9em;
    }
    
    .product-meta {
      display: flex;
      gap: 15px;
      margin-bottom: 12px;
      font-size: 0.85em;
    }
    
    .product-quantity {
      color: #555;
    }
    
    .product-stock.in-stock {
      color: #2e7d32;
    }
    
    .product-stock.out-of-stock {
      color: #c62828;
    }
    
    .variants-container {
      margin-bottom: 12px;
    }
    
    .variant-select {
      width: 100%;
      padding: 8px;
      border-radius: 8px;
      border: 1px solid #ddd;
      background: #f9f9f9;
    }
    
    .product-actions, .fare-actions {
      display: flex;
      gap: 10px;
    }
    
    .add-to-cart, .buy-now, .order-now, .book-service {
      font-family: poppins;
      flex: 1;
      padding: 8px 12px;
      border: none;
      border-radius: 8px;
      font-weight: 500;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
      transition: all 0.2s ease;
    }
    
    .add-to-cart {
      background: #f5f5f5;
      color: #333;
    }
    
    .add-to-cart:hover {
      background: #e0e0e0;
    }
    
    .buy-now, .book-service {
      background: #ffd700;
      color: #000;
    }
    
    .buy-now:hover, .book-service:hover {
      background: #ffc400;
    }
    
     .order-now {
      font-family: poppins;
      background: #ed2939;
      color: #fff;
    }
    
     .order-now:hover {
      background: #4169E1	;
    }

    .fare-price {
      width: 80px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-weight: bold;
      font-size: 1.2em;
      background: #f5f5f5;
      color: #333;
    }
    
/* Cart Overlay Styles */
.cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.cart-overlay.active {
  opacity: 1;
  visibility: visible;
}

.cart-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 80vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.cart-header {
  padding: 20px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cart-header h3 {
  margin: 0;
  font-size: 1.3em;
  color: #333;
}

.close-cart {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.empty-cart {
  text-align: center;
  padding: 30px 0;
}

.empty-cart i {
  font-size: 3em;
  color: #ccc;
  margin-bottom: 15px;
}

.empty-cart p {
  margin: 10px 0 20px;
  color: #666;
}

.empty-cart button {
  background: var(--primary-color);
  color: #000;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
}

.cart-item {
  display: flex;
  padding: 15px 0;
  border-bottom: 1px solid #f5f5f5;
  position: relative;
}

.cart-item-image {
  width: 80px;
  height: 80px;
  flex-shrink: 0;
  margin-right: 15px;
}

.cart-item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.cart-item-details {
  flex: 1;
}

.cart-item-details h4 {
  margin: 0 0 5px;
  font-size: 1em;
  color: #333;
}

.cart-item-details .variant {
  margin: 0 0 8px;
  font-size: 0.85em;
  color: #666;
}

.cart-item-price {
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.cart-item-quantity {
    color: #333;
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-btn {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  border: 1px solid #ddd;
  background: #fff;
  font-size: 1em;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.quantity-btn:hover {
  background: #f5f5f5;
}

.quantity {
  min-width: 20px;
  text-align: center;
}

.remove-item {
  position: absolute;
  top: 15px;
  right: 0;
  background: none;
  border: none;
  color: #999;
  cursor: pointer;
  font-size: 1em;
}

.remove-item:hover {
  color: #ff5252;
}

.cart-summary {
  padding: 20px;
  border-top: 1px solid #eee;
}

.cart-total {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  font-size: 1.1em;
  font-weight: bold;
}

.cart-actions {
  display: flex;
  gap: 10px;
}

.clear-cart, .checkout-btn {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.clear-cart {
  background: #f5f5f5;
  color: #333;
  border: none;
}

.clear-cart:hover {
  background: #e0e0e0;
}

.checkout-btn {
  background: var(--primary-color);
  color: #000;
  border: none;
}

.checkout-btn:hover {
  background: #ffc400;
}

      .cart-button { 
          display: flex; 
          align-items: center; 
          justify-content: center; 
          padding: 15px; 
          background: none; 
          border: none; 
          color: var(--primary-color); 
          cursor: pointer; 
          transition: background-color 0.3s ease; 
          border-radius: 4px; 
          width: 40px; 
          height: 40px; 
      }
      
      .cart-button:hover { 
          background: rgba(255, 215, 0, 0.1); 
      }
      
      .cart-button i { 
          font-size: 18px; 
      }

/* Order Form Styles */
.order-form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.order-form-overlay.active {
  opacity: 1;
  visibility: visible;
}

.order-form-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 24px;
  position: relative;
}

.order-form-container h3 {
  margin: 0 0 20px;
  text-align: center;
  color: #333;
}

.close-order-form {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #333;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1em;
}

.form-group textarea {
  resize: vertical;
  min-height: 80px;
}

.coupon-input {
  display: flex;
  gap: 10px;
}

.coupon-input input {
  flex: 1;
}

.apply-coupon {
  background: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 0 15px;
  cursor: pointer;
}

.apply-coupon:hover {
  background: #e0e0e0;
}

.coupon-message {
  margin-top: 8px;
  font-size: 0.9em;
}

.coupon-message.success {
  color: #2e7d32;
}

.coupon-message.error {
  color: #c62828;
}

.order-summary {
  margin: 20px 0;
  padding: 16px;
  background: #f9f9f9;
  border-radius: 8px;
}

.order-summary h4 {
  margin: 0 0 12px;
  color: #333;
}

.order-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid #eee;
  color: #333;
}

.order-total {
  display: flex;
  justify-content: space-between;
  margin-top: 12px;
  font-weight: bold;
  font-size: 1.1em;
  color: #333;
}

.submit-order {
  width: 100%;
  padding: 14px;
  background: var(--primary-color);
  color: #000;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  font-size: 1em;
  cursor: pointer;
  transition: background 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.submit-order:hover {
  background: #ffc400;
}

/* Confirmation Modal Styles */
.confirmation-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 4000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.confirmation-modal.active {
  opacity: 1;
  visibility: visible;
}

.confirmation-content {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 400px;
  padding: 30px;
  text-align: center;
}

.confirmation-icon {
  font-size: 4em;
  color: #4caf50;
  margin-bottom: 20px;
}

.confirmation-content h3 {
  margin: 0 0 15px;
  color: #333;
}

.confirmation-content p {
  margin: 0 0 25px;
  color: #666;
  line-height: 1.5;
}

.confirmation-actions {
  display: flex;
  gap: 10px;
}

.view-receipt, .close-confirmation {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.view-receipt {
  background: #4caf50;
  color: #fff;
  border: none;
}

.view-receipt:hover {
  background: #3d8b40;
}

.close-confirmation {
  background: #f5f5f5;
  color: #333;
  border: none;
}

.close-confirmation:hover {
  background: #e0e0e0;
}

/* Responsive Styles */
@media (max-width: 480px) {
  .cart-container {
    width: 95%;
  }
  
  .cart-item {
    flex-direction: column;
  }
  
  .cart-item-image {
    width: 100%;
    height: 150px;
    margin-right: 0;
    margin-bottom: 10px;
  }
  
  .cart-actions {
    flex-direction: column;
  }
  
  .order-form-container {
    padding: 16px;
  }
  
  .confirmation-actions {
    flex-direction: column;
  }
}
    
    /* Toast notification */
    .toast {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background: #333;
      color: #fff;
      padding: 12px 24px;
      border-radius: 8px;
      z-index: 4000;
      opacity: 0;
      visibility: hidden;
      transition: all 0.3s ease;
    }
    
    .toast.show {
      opacity: 1;
      visibility: visible;
    }
    
    @media (max-width: 768px) {
      .product-item, .fare-item {
        flex-direction: column;
      }
      
      .product-image, .fare-image {
        width: 100%;
        height: 180px;
      }
      
      .fare-price {
        width: 100%;
        padding: 12px;
      }
    }

  `;
  document.head.appendChild(style);
}

function showToast(message) {
  let toast = document.querySelector('.toast');
  if (!toast) {
    toast = document.createElement('div');
    toast.className = 'toast';
    document.body.appendChild(toast);
  }
  
  toast.textContent = message;
  toast.classList.add('show');
  
  setTimeout(() => {
    toast.classList.remove('show');
  }, 3000);
}

function updatePrice(selectElement) {
  const price = selectElement.value;
  const itemElement = selectElement.closest('[data-price]');
  if (itemElement) {
    itemElement.dataset.price = price;
    const priceDisplay = itemElement.querySelector('.product-price, .fare-price');
    if (priceDisplay) {
      priceDisplay.textContent = `₹${price}`;
    }
  }
}

function initializeTabs() {
  // You can add tab functionality if needed for different service sections
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

function getFacilityIcon(facilityName) {
  const icons = {
    'Wide Selection': 'shopping-basket',
    'Great Prices': 'tags',
    'Clean Stores': 'broom',
    'Vegetarian Options': 'leaf',
    'Family Friendly': 'users',
    'Late Night': 'moon',
    'Expert Advice': 'user-tie',
    'Installation': 'tools',
    'EMI Options': 'credit-card',
    'Parking': 'parking',
    'AC Shopping': 'snowflake',
    'Wheelchair Access': 'wheelchair',
    'Drive-Thru': 'car',
    'Free WiFi': 'wifi'
  };
  
  return icons[facilityName] || 'check-circle';
}

function filterLocators(category) {
  const locatorsGrid = document.getElementById('locatorsGrid');
  let filteredData = locatorsData;

  if (category !== 'All') {
    filteredData = filteredData.filter(locator => locator.category === category);
  }

  locatorsGrid.innerHTML = renderLocatorCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function sortLocators(criteria) {
  const locatorsGrid = document.getElementById('locatorsGrid');
  let sortedData = [...locatorsData];

  switch(criteria) {
    case 'rating':
      sortedData.sort((a, b) => parseFloat(b.details.rating) - parseFloat(a.details.rating));
      break;
    case 'name':
      sortedData.sort((a, b) => a.details.name.localeCompare(b.details.name));
      break;
    case 'distance':
      // This would require location data to be implemented
      break;
  }

  locatorsGrid.innerHTML = renderLocatorCards(sortedData);
  toggleLocatorSortOptions();
}

function toggleLocatorSortOptions() {
  const sortOptions = document.getElementById('locatorSortOptionsContainer');
  sortOptions.classList.toggle('active');
}

function hideLocatorsOverlay() {
  const overlay = document.getElementById('locatorsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
    }, 300);
  }
}

function shareStore(storeName) {
  const store = locatorsData.find(loc => loc.details.name === storeName);
  if (!store) return;

  if (navigator.share) {
    navigator.share({
      title: store.details.name,
      text: `Check out ${store.details.name} - ${store.details.description}`,
      url: store.details.website || window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    const shareUrl = store.details.website || window.location.href;
    const shareText = `Check out ${store.details.name}: ${shareUrl}`;
    prompt('Copy this link to share:', shareText);
  }
}

function openFullScreen(imageUrl) {
  event.stopPropagation();
  const fullScreenDiv = document.createElement('div');
  fullScreenDiv.className = 'full-screen-image';
  fullScreenDiv.innerHTML = `
    <img src="${imageUrl}" alt="Full screen">
    <button class="close-full-screen" onclick="document.querySelector('.full-screen-image').remove()">
      <i class="fas fa-times"></i>
    </button>
  `;
  document.body.appendChild(fullScreenDiv);
  
  setTimeout(() => {
    fullScreenDiv.classList.add('active');
    const img = fullScreenDiv.querySelector('img');
    if (img) {
      img.style.opacity = '1';
      img.style.transform = 'scale(1)';
    }
  }, 10);
}

function changeCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentSlide(index) {
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

// Add store carousel functionality
function initializeStoreCarousel() {
  const carousel = document.querySelector('.store-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.store-carousel-item');
  const dots = carousel.querySelectorAll('.store-pagination-dot');
  let currentIndex = 0;

  function showSlide(index) {
    items.forEach((item, i) => {
      item.classList.remove('active');
      dots[i].classList.remove('active');
      if (i === index) {
        item.classList.add('active');
        dots[i].classList.add('active');
      }
    });
    currentIndex = index;
  }

  function nextSlide() {
    const newIndex = (currentIndex + 1) % items.length;
    showSlide(newIndex);
  }

  function prevSlide() {
    const newIndex = (currentIndex - 1 + items.length) % items.length;
    showSlide(newIndex);
  }

  // Auto-rotate every 5 seconds
  let interval = setInterval(nextSlide, 5000);

  // Pause on hover
  carousel.addEventListener('mouseenter', () => {
    clearInterval(interval);
  });

  carousel.addEventListener('mouseleave', () => {
    interval = setInterval(nextSlide, 5000);
  });
}

function changeStoreCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.store-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.store-carousel-item');
  const dots = carousel.querySelectorAll('.store-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentStoreSlide(index) {
  const carousel = event.target.closest('.store-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.store-carousel-item');
  const dots = carousel.querySelectorAll('.store-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function copyToClipboard(text) {
  event.stopPropagation();
  navigator.clipboard.writeText(text).then(() => {
    const button = event.target.closest('button');
    if (button) {
      const originalText = button.textContent;
      button.textContent = 'Copied!';
      setTimeout(() => {
        button.textContent = originalText;
      }, 2000);
    }
  }).catch(err => {
    console.error('Failed to copy text: ', err);
  });
}

function showChatSupport(storeName) {
  event.stopPropagation();
  alert(`Chat support for ${storeName} would open here in a real implementation.`);
}

const publicPlacesData = [
  // Delhi - Park
  {
    category: "Park",
    metropolis: "Delhi",
    details: {
      name: "Lodhi Garden",
      cardImages: [
        "https://www.tripsavvy.com/thmb/IfH8eeAQCSljDTMWfMK8JWv45OE=/960x0/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-180344996-a67b6e54b64f450ab849d0a1fb15d99c.jpg",
        "https://www.gosahin.com/go/p/h/1564857104_lodhi-garden-delhi3.jpg"
      ],
      placeImages: [
        "https://www.tripsavvy.com/thmb/IfH8eeAQCSljDTMWfMK8JWv45OE=/960x0/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-180344996-a67b6e54b64f450ab849d0a1fb15d99c.jpg",
        "https://www.gosahin.com/go/p/h/1564857104_lodhi-garden-delhi3.jpg",
        "https://www.delhitourism.gov.in/dttdc/explore/lodhi_garden_1.jpg",
        "https://www.delhitourism.gov.in/dttdc/explore/lodhi_garden_2.jpg"
      ],
      description: "A serene park with historical monuments and lush greenery.",
      address: "Lodhi Road, Delhi",
      timings: "Mon-Sun: 9 AM - 10 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/lodhi-garden",
      website: "https://www.delhitourism.gov.in/lodhi-garden",
      social: {
        facebook: "https://facebook.com/lodhigarden",
        twitter: "https://twitter.com/lodhigarden",
        instagram: "https://instagram.com/lodhigarden"
      },
      facilities: [
        { name: "Walking Trails", description: "Well-maintained walking paths" },
        { name: "Picnic Spots", description: "Designated areas for picnics" }
      ],
      events: [
        { name: "Yoga Sessions", date: "Every Sunday, 7 AM" }
      ],
      support: {
        email: "info@lodhigarden.com",
        phone: "+91-1234567890"
      },
      rating: "4.5",
      reviews: "1200+",
      established: "1936",
      tags: ["Historical", "Nature", "Free Entry"],
      categoryInsights: {
        activities: ["Morning Walks", "Bird Watching", "Photography"],
        historicalMonuments: ["Mohammed Shah's Tomb", "Sikander Lodi's Tomb"],
        bestTimeToVisit: "6-8 AM for walks, 4-6 PM for photography",
        parkingAvailability: "Available (₹50 for 2 hours)",
        guidedTours: "Heritage walks every Saturday at 8 AM",
        accessibility: "Wheelchair accessible pathways",
        petPolicy: "Allowed on leash",
        notableFeatures: ["Rose Garden", "Bonsai Section", "Herb Garden"]
      }
    }
  },
  
  // Delhi - School
  {
    category: "School",
    metropolis: "Delhi",
    details: {
      name: "Delhi Public School, RK Puram",
      cardImages: [
        "https://example.com/dps-rkp1.jpg",
        "https://example.com/dps-rkp2.jpg"
      ],
      placeImages: [
        "https://example.com/dps-rkp1.jpg",
        "https://example.com/dps-rkp2.jpg",
        "https://example.com/dps-rkp3.jpg",
        "https://example.com/dps-rkp4.jpg"
      ],
      description: "Premier educational institution with excellent academic record.",
      address: "Dr. S. Radhakrishnan Marg, RK Puram, Delhi",
      timings: "Mon-Fri: 8 AM - 2 PM",
      entryFee: "N/A",
      mapLink: "https://maps.example.com/dps-rkp",
      website: "https://www.dpsrkp.net",
      social: {
        facebook: "https://facebook.com/dpsrkp",
        twitter: "https://twitter.com/dpsrkp",
        instagram: "https://instagram.com/dpsrkp"
      },
      facilities: [
        { name: "Library", description: "Extensive collection of books" },
        { name: "Sports Complex", description: "Cricket, football, basketball facilities" }
      ],
      events: [
        { name: "Annual Day", date: "15th December, 2023" }
      ],
      support: {
        email: "info@dpsrkp.net",
        phone: "+91-1126185888"
      },
      rating: "4.7",
      reviews: "500+",
      established: "1972",
      tags: ["Education", "CBSE", "Premium School"],
      categoryInsights: {
        admissionStatus: "Open (Till 31st March 2024)",
        feeStructure: {
          nursery: "₹85,000 annually",
          primary: "₹95,000 annually",
          secondary: "₹1,10,000 annually"
        },
        admissionForm: "https://www.dpsrkp.net/admissions/form.pdf",
        curriculum: "CBSE",
        classesOffered: "Nursery to Class 12",
        entranceExam: "Entrance test for Grade 1 onwards",
        facilities: ["Smart Classrooms", "Science Labs", "Library", "Sports Complex"],
        faculty: "150+ teachers with 70% postgraduates",
        scholarships: "Merit-based scholarships available",
        extracurricular: ["Annual Sports Day", "Science Fair", "Music Competition"],
        results: "100% pass rate in 2023 board exams",
        transport: "School buses available across Delhi",
        uniform: "Available at school store (₹2,500 complete set)"
      }
    }
  },

  // Delhi - Hospital
  {
    category: "Hospital",
    metropolis: "Delhi",
    details: {
      name: "AIIMS",
      cardImages: [
        "https://example.com/aiims1.jpg",
        "https://example.com/aiims2.jpg"
      ],
      placeImages: [
        "https://example.com/aiims1.jpg",
        "https://example.com/aiims2.jpg",
        "https://example.com/aiims3.jpg",
        "https://example.com/aiims4.jpg"
      ],
      description: "Premier medical institution and hospital in India.",
      address: "Ansari Nagar, Delhi",
      timings: "24/7",
      entryFee: "Free for OPD (charges may apply for treatments)",
      mapLink: "https://maps.example.com/aiims-delhi",
      website: "https://www.aiims.edu",
      social: {
        facebook: "https://facebook.com/aiims",
        twitter: "https://twitter.com/aiims",
        instagram: "https://instagram.com/aiims"
      },
      facilities: [
        { name: "Emergency Services", description: "24/7 emergency care" },
        { name: "Specialty Clinics", description: "All major specialties available" }
      ],
      events: [
        { name: "Health Camp", date: "Quarterly" }
      ],
      support: {
        email: "info@aiims.edu",
        phone: "+91-1126589111"
      },
      rating: "4.6",
      reviews: "5000+",
      established: "1956",
      tags: ["Healthcare", "Government", "Multi-specialty"],
      categoryInsights: {
        departments: ["Cardiology", "Neurology", "Oncology", "Pediatrics", "Orthopedics"],
        consultationCharges: {
          opd: "₹100 (General), ₹500 (Specialist)",
          ipd: "₹1,000 per day (General Ward)"
        },
        appointment: "Online booking available at portal.aiims.edu",
        emergency: "24/7 emergency services with ambulance",
        healthPackages: [
          "Full Body Checkup: ₹5,000",
          "Cardiac Screening: ₹7,500",
          "Diabetes Profile: ₹3,200"
        ],
        insurance: ["All major insurers accepted"],
        doctors: "500+ specialists on staff",
        bedAvailability: {
          general: "250 beds",
          icu: "50 beds",
          ventilator: "30 units"
        },
        pharmacy: "24/7 pharmacy on premises",
        diagnosticServices: ["MRI", "CT Scan", "Ultrasound", "X-Ray"],
        visitingHours: "4 PM - 6 PM daily"
      }
    }
  },

  // Delhi - Gym
  {
    category: "Gym",
    metropolis: "Delhi",
    details: {
      name: "Talwalkars Fitness Center",
      cardImages: [
        "https://example.com/talwalkars1.jpg",
        "https://example.com/talwalkars2.jpg"
      ],
      placeImages: [
        "https://example.com/talwalkars1.jpg",
        "https://example.com/talwalkars2.jpg",
        "https://example.com/talwalkars3.jpg",
        "https://example.com/talwalkars4.jpg"
      ],
      description: "Premium fitness center with modern equipment and trained trainers.",
      address: "Hauz Khas, Delhi",
      timings: "Mon-Sun: 6 AM - 10 PM",
      entryFee: "Membership required",
      mapLink: "https://maps.example.com/talwalkars-hk",
      website: "https://www.talwalkars.net",
      social: {
        facebook: "https://facebook.com/talwalkars",
        twitter: "https://twitter.com/talwalkars",
        instagram: "https://instagram.com/talwalkars"
      },
      facilities: [
        { name: "Cardio Section", description: "Treadmills, ellipticals, etc." },
        { name: "Weight Training", description: "Free weights and machines" }
      ],
      events: [
        { name: "Fitness Challenge", date: "Monthly" }
      ],
      support: {
        email: "hauzkhas@talwalkars.net",
        phone: "+91-1126567890"
      },
      rating: "4.5",
      reviews: "1200+",
      established: "2010",
      tags: ["Fitness", "Health", "Premium Gym"],
      categoryInsights: {
        membershipPlans: {
          basic: "₹2,500/month",
          premium: "₹4,000/month",
          annual: "₹35,000/year",
          couple: "₹6,000/month"
        },
        trainers: [
          "Rahul Sharma (Certified Personal Trainer - 10 years experience)",
          "Priya Patel (Yoga Specialist - RYT 500 certified)"
        ],
        classSchedule: {
          zumba: "Mon, Wed, Fri - 7 AM & 6 PM",
          yoga: "Tue, Thu, Sat - 8 AM & 7 PM",
          kickboxing: "Mon, Thu - 5 PM",
          pilates: "Wed, Fri - 9 AM"
        },
        trialSession: "1 free trial session available",
        equipment: "Latest cardio and strength machines from Technogym",
        personalTraining: "₹1,500/session (package discounts available)",
        nutritionConsultation: "Available with certified dietician (₹800/session)",
        lockerFacility: "Available (bring your own lock)",
        towelService: "Provided for premium members",
        cancellationPolicy: "30-day notice for membership cancellation"
      }
    }
  },

  // Delhi - Wedding Venue
  {
    category: "Wedding Venue",
    metropolis: "Delhi",
    details: {
      name: "The Ashok Hotel",
      cardImages: [
        "https://example.com/ashok1.jpg",
        "https://example.com/ashok2.jpg"
      ],
      placeImages: [
        "https://example.com/ashok1.jpg",
        "https://example.com/ashok2.jpg",
        "https://example.com/ashok3.jpg",
        "https://example.com/ashok4.jpg"
      ],
      description: "Luxurious heritage hotel with grand wedding venues.",
      address: "50-B, Chanakyapuri, Delhi",
      timings: "24/7",
      entryFee: "Booking required",
      mapLink: "https://maps.example.com/ashok-hotel",
      website: "https://www.theashok.com",
      social: {
        facebook: "https://facebook.com/theashok",
        twitter: "https://twitter.com/theashok",
        instagram: "https://instagram.com/theashok"
      },
      facilities: [
        { name: "Banquet Halls", description: "Multiple sizes available" },
        { name: "Catering", description: "In-house catering services" }
      ],
      events: [
        { name: "Wedding Expo", date: "Annually in November" }
      ],
      support: {
        email: "events@theashok.com",
        phone: "+91-1126110101"
      },
      rating: "4.4",
      reviews: "800+",
      established: "1956",
      tags: ["Luxury", "Heritage", "Grand Weddings"],
      categoryInsights: {
        venueCapacity: {
          "Grand Ballroom": "500 guests",
          "Garden Lawn": "1000 guests",
          "Royal Suite": "150 guests"
        },
        packageRates: {
          basic: "₹2,500 per plate (min. 200 guests)",
          premium: "₹3,500 per plate (min. 300 guests)",
          royal: "₹5,000 per plate (min. 150 guests)"
        },
        bookingPolicy: "50% advance to block dates",
        decorationOptions: ["Floral", "Theme-based", "Traditional"],
        cateringOptions: ["Vegetarian", "Non-vegetarian", "Jain", "Continental"],
        accommodation: "200 rooms available for wedding guests",
        parking: "Valet parking for 200 cars",
        cancellationPolicy: "90 days for full refund",
        preferredVendors: {
          photographers: ["Wedding Moments", "Frame by Frame"],
          makeupArtists: ["Glam Squad", "Bridal Beauty"]
        },
        soundPolicy: "Music must end by 11 PM",
        weddingPlanner: "Dedicated planner assigned for each booking"
      }
    }
  },

  // Delhi - Movie Theater
  {
    category: "Movie Theater",
    metropolis: "Delhi",
    details: {
      name: "PVR Select Citywalk",
      cardImages: [
        "https://example.com/pvr1.jpg",
        "https://example.com/pvr2.jpg"
      ],
      placeImages: [
        "https://example.com/pvr1.jpg",
        "https://example.com/pvr2.jpg",
        "https://example.com/pvr3.jpg",
        "https://example.com/pvr4.jpg"
      ],
      description: "Premium multiplex with latest movies and comfortable seating.",
      address: "Select Citywalk Mall, Saket, Delhi",
      timings: "10 AM - 1 AM",
      entryFee: "Ticket prices vary",
      mapLink: "https://maps.example.com/pvr-saket",
      website: "https://www.pvrcinemas.com",
      social: {
        facebook: "https://facebook.com/pvrcinemas",
        twitter: "https://twitter.com/pvrcinemas",
        instagram: "https://instagram.com/pvrcinemas"
      },
      facilities: [
        { name: "Dolby Atmos", description: "Premium sound system" },
        { name: "Food Court", description: "Snacks and beverages available" }
      ],
      events: [
        { name: "Film Festival", date: "Annually in September" }
      ],
      support: {
        email: "customercare@pvrcinemas.com",
        phone: "+91-1124567890"
      },
      rating: "4.4",
      reviews: "5000+",
      established: "2007",
      tags: ["Movies", "Entertainment", "Premium"],
      categoryInsights: {
        currentMovies: [
          {
            title: "Avengers: Endgame",
            timings: ["10:30 AM", "2:00 PM", "6:30 PM", "10:00 PM"],
            screenType: "IMAX 3D"
          },
          {
            title: "The Lion King",
            timings: ["11:00 AM", "3:00 PM", "7:30 PM"],
            screenType: "4DX"
          }
        ],
        ticketPricing: {
          standard: "₹250-₹400",
          premium: "₹500-₹800",
          recliner: "₹1,200"
        },
        foodMenu: {
          combos: [
            "Popcorn + Pepsi: ₹300",
            "Burger + Fries + Coke: ₹450"
          ],
          alacarte: {
            "Popcorn (Large)": "₹200",
            "Nachos with Cheese": "₹250"
          }
        },
        screenTypes: ["IMAX", "4DX", "PXL", "Director's Cut"],
        seatMap: "Available on booking page",
        offers: [
          "Monday Magic: 20% off on all tickets",
          "Student Discount: 15% off with valid ID"
        ],
        parking: "Validated parking for 3 hours",
        accessibility: "Wheelchair accessible screens available",
        membership: "PVR Privilege Card offers 10% discount"
      }
    }
  },

  // Delhi - Hotel & Resort
  {
    category: "Hotel & Resort",
    metropolis: "Delhi",
    details: {
      name: "The Taj Mahal Hotel",
      cardImages: [
        "https://example.com/taj1.jpg",
        "https://example.com/taj2.jpg"
      ],
      placeImages: [
        "https://example.com/taj1.jpg",
        "https://example.com/taj2.jpg",
        "https://example.com/taj3.jpg",
        "https://example.com/taj4.jpg"
      ],
      description: "Iconic luxury hotel with world-class amenities.",
      address: "1, Mansingh Road, Delhi",
      timings: "24/7",
      entryFee: "Room rates apply",
      mapLink: "https://maps.example.com/taj-mahal-hotel",
      website: "https://www.tajhotels.com",
      social: {
        facebook: "https://facebook.com/tajhotels",
        twitter: "https://twitter.com/tajhotels",
        instagram: "https://instagram.com/tajhotels"
      },
      facilities: [
        { name: "Spa", description: "Luxury spa services" },
        { name: "Fine Dining", description: "Multiple restaurants" }
      ],
      events: [],
      support: {
        email: "reservations@tajhotels.com",
        phone: "+91-1123026162"
      },
      rating: "4.8",
      reviews: "2500+",
      established: "1978",
      tags: ["Luxury", "5-Star", "Iconic"],
      categoryInsights: {
        roomTypes: [
          {
            name: "Deluxe Room",
            price: "₹15,000/night",
            capacity: "2 adults"
          },
          {
            name: "Executive Suite",
            price: "₹25,000/night",
            capacity: "2 adults + 1 child"
          },
          {
            name: "Presidential Suite",
            price: "₹1,00,000/night",
            capacity: "4 adults"
          }
        ],
        diningOptions: [
          "Indian (Bukhara - famous for dal makhani)",
          "Continental (The Grill Room)",
          "Chinese (House of Ming)"
        ],
        spaServices: [
          "Ayurvedic Massage (60 mins - ₹5,000)",
          "Swedish Massage (90 mins - ₹7,500)"
        ],
        bookingPolicy: "1 night deposit required",
        cancellationPolicy: "48 hours for full refund",
        checkInOut: "2 PM / 12 PM",
        amenities: [
          "24-hour room service",
          "Free WiFi",
          "Swimming pool",
          "Business center"
        ],
        nearbyAttractions: [
          "India Gate (1 km)",
          "Connaught Place (3 km)"
        ],
        transport: "Airport pickup available (₹2,500)",
        petPolicy: "Small pets allowed (₹2,000 cleaning fee)"
      }
    }
  },

  // Delhi - Shopping Mall
  {
    category: "Shopping Mall",
    metropolis: "Delhi",
    details: {
      name: "DLF Promenade",
      cardImages: [
        "https://example.com/promenade1.jpg",
        "https://example.com/promenade2.jpg"
      ],
      placeImages: [
        "https://example.com/promenade1.jpg",
        "https://example.com/promenade2.jpg",
        "https://example.com/promenade3.jpg",
        "https://example.com/promenade4.jpg"
      ],
      description: "Upscale shopping mall with international brands and dining options.",
      address: "Vasant Kunj, Delhi",
      timings: "11 AM - 10 PM",
      entryFee: "Free",
      mapLink: "https://maps.example.com/dlf-promenade",
      website: "https://www.dlfpromenade.com",
      social: {
        facebook: "https://facebook.com/dlfpromenade",
        twitter: "https://twitter.com/dlfpromenade",
        instagram: "https://instagram.com/dlfpromenade"
      },
      facilities: [
        { name: "Valet Parking", description: "Available at all entrances" },
        { name: "Kids Play Area", description: "Supervised play zone" }
      ],
      events: [
        { name: "Winter Sale", date: "15th December - 15th January" }
      ],
      support: {
        email: "info@dlfpromenade.com",
        phone: "+91-1123456789"
      },
      rating: "4.3",
      reviews: "3200+",
      established: "2008",
      tags: ["Shopping", "Luxury", "Dining"],
      categoryInsights: {
        storeDirectory: [
          {
            category: "Fashion",
            brands: ["Zara", "H&M", "Marks & Spencer"]
          },
          {
            category: "Electronics",
            brands: ["Apple Store", "Samsung", "Bose"]
          }
        ],
        foodCourt: [
          "McDonald's",
          "KFC",
          "Haldiram's",
          "Subway"
        ],
        fineDining: [
          "Olive Bar & Kitchen",
          "The Big Chill Cafe"
        ],
        floorPlan: {
          "LG Floor": "Supermarket & Parking",
          "Ground Floor": "Luxury Brands",
          "First Floor": "Fashion & Accessories",
          "Second Floor": "Electronics & Home",
          "Third Floor": "Food Court & Cinema"
        },
        ongoingOffers: [
          "10% off on first purchase with mall app",
          "Free parking for 4 hours with ₹5,000 purchase"
        ],
        eventsCalendar: [
          "Live Music: Every Friday 7 PM",
          "Kids Workshop: Every Sunday 11 AM"
        ],
        services: [
          "Wheelchair available at concierge",
          "Stroller rentals",
          "Gift wrapping"
        ],
        parkingRates: [
          "First 2 hours: Free",
          "Additional hours: ₹50 per hour"
        ],
        cinema: "PVR Cinemas - 6 screens",
        loyaltyProgram: "DLF Privilege Card - Earn points on purchases"
      }
    }
  },

  // Delhi - Museum
  {
    category: "Museum",
    metropolis: "Delhi",
    details: {
      name: "National Museum",
      cardImages: [
        "https://example.com/national-museum1.jpg",
        "https://example.com/national-museum2.jpg"
      ],
      placeImages: [
        "https://example.com/national-museum1.jpg",
        "https://example.com/national-museum2.jpg",
        "https://example.com/national-museum3.jpg",
        "https://example.com/national-museum4.jpg"
      ],
      description: "Explore India's rich history and cultural heritage.",
      address: "Janpath, Delhi",
      timings: "Mon-Sun: 7 AM - 11 PM",
      entryFee: "$5",
      mapLink: "https://maps.example.com/national-museum",
      website: "https://www.nationalmuseumindia.gov.in",
      social: {
        facebook: "https://facebook.com/nationalmuseum",
        twitter: "https://twitter.com/nationalmuseum",
        instagram: "https://instagram.com/nationalmuseum"
      },
      facilities: [
        { name: "Guided Tours", description: "Available in multiple languages" },
        { name: "Cafeteria", description: "Serves snacks and beverages" }
      ],
      events: [
        { name: "Art Exhibition", date: "15th October, 2023" }
      ],
      support: {
        email: "info@nationalmuseum.com",
        phone: "+91-9876543210"
      },
      rating: "4.3",
      reviews: "850+",
      established: "1949",
      tags: ["Historical", "Art", "Cultural"],
      categoryInsights: {
        collections: [
          "Harappan Civilization Gallery",
          "Buddhist Art Section",
          "Coin Gallery"
        ],
        guidedTourTimings: [
          "English: 10:30 AM, 2:30 PM",
          "Hindi: 11:30 AM, 3:30 PM"
        ],
        ticketPricing: {
          indianAdults: "₹20",
          foreignTourists: "₹650",
          children: "Free (below 12 years)",
          cameraFee: "₹300 (no flash allowed)"
        },
        currentExhibitions: [
          "Mughal Miniature Paintings (Till 30th Nov)",
          "Ancient Indian Textiles (Till 15th Dec)"
        ],
        accessibility: [
          "Wheelchair accessible",
          "Braille guides available"
        ],
        photographyPolicy: "Allowed without flash (₹300 fee)",
        nearbyAttractions: [
          "India Gate (1 km)",
          "Connaught Place (2 km)"
        ],
        library: "Reference library open to researchers",
        museumShop: "Sells replicas and books",
        averageVisitDuration: "2-3 hours"
      }
    }
  },

  // Delhi - Airport
  {
    category: "Airport",
    metropolis: "Delhi",
    details: {
      name: "Indira Gandhi International Airport",
      cardImages: [
        "https://example.com/igia1.jpg",
        "https://example.com/igia2.jpg"
      ],
      placeImages: [
        "https://example.com/igia1.jpg",
        "https://example.com/igia2.jpg",
        "https://example.com/igia3.jpg",
        "https://example.com/igia4.jpg"
      ],
      description: "India's largest and busiest international airport.",
      address: "Airport Road, Delhi",
      timings: "24/7",
      entryFee: "Free (ticket required for flights)",
      mapLink: "https://maps.example.com/delhi-airport",
      website: "https://www.newdelhiairport.in",
      social: {
        facebook: "https://facebook.com/delhiairport",
        twitter: "https://twitter.com/delhiairport",
        instagram: "https://instagram.com/delhiairport"
      },
      facilities: [
        { name: "Lounges", description: "Premium lounges available" },
        { name: "Shopping", description: "Duty-free and retail stores" }
      ],
      events: [],
      support: {
        email: "feedback@delhiairport.com",
        phone: "+91-1125602000"
      },
      rating: "4.5",
      reviews: "15000+",
      established: "1962",
      tags: ["International", "Transport", "Aviation"],
      categoryInsights: {
        terminals: [
          {
            name: "Terminal 3",
            airlines: "All international & some domestic flights",
            checkIn: "3 hours before departure (international)"
          },
          {
            name: "Terminal 2",
            airlines: "Domestic flights (IndiGo, SpiceJet)",
            checkIn: "2 hours before departure"
          }
        ],
        transportation: [
          "Delhi Metro Airport Express Line (every 10 mins)",
          "Prepaid taxi counter (24/7)",
          "Bus services to city center"
        ],
        lounges: [
          "Plaza Premium Lounge (T3 - ₹2,500 for 3 hours)",
          "Air India Maharaja Lounge (T3 - Business class access)"
        ],
        shopping: [
          "Duty-free shops (international departures)",
          "Indian handicrafts & souvenirs"
        ],
        dining: [
          "Food Court (T3 - Indian & international cuisine)",
          "Cafés (Starbucks, Costa Coffee)"
        ],
        services: [
          "Currency exchange (24/7)",
          "Baggage wrapping",
          "Left luggage facility"
        ],
        parking: [
          "Short-term: ₹150 first hour, ₹100 subsequent hours",
          "Long-term: ₹500 per day"
        ],
        wifi: "Free for 45 minutes (requires mobile verification)",
        hotels: [
          "Aerocity Hotels (5 min drive)",
          "Transit hotel inside T3"
        ],
        flightInfo: "Real-time displays throughout terminals"
      }
    }
  }
  // Additional places can be added following the same structure
];

function showPlacesOverlay() {
  let overlay = document.getElementById('placesOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'placesOverlay';
    overlay.className = 'places-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(publicPlacesData.map(place => place.category))];

  overlay.innerHTML = `
    <div class="places-content">
      <div class="places-header">
        <div class="header-title-container">
          <h2>Public Places</h2>
          <div class="header-subtitle">Discover attractions</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-places" onclick="hidePlacesOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="sortOptionsContainer">
        <div class="sort-option" onclick="sortPlaces('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortPlaces('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortPlaces('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="places-filter-container">
        <div class="places-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterPlaces('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="places-grid" id="placesGrid">
        ${renderPlaceCards(publicPlacesData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="placesExploreInput" placeholder="Search for parks, museums..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('placesExploreInput');
  const placesGrid = document.getElementById('placesGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(publicPlacesData, query, placesGrid, filterButtons, renderPlaceCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(publicPlacesData, exploreInput.value, placesGrid, filterButtons, renderPlaceCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(publicPlacesData, query, placesGrid, filterButtons, renderPlaceCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);


  const style = document.createElement('style');
  style.textContent = `
.places-overlay {
  font-family: poppins;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.places-overlay::-webkit-scrollbar {
  display: none;
}

.places-overlay.active {
  opacity: 1;
  visibility: visible;
}

.places-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.places-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.places-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-places {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-places:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-places:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.button-text {
  font-weight: 500;
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.places-filter-container {
  position: fixed;
  top: 64px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.places-filter-buttons {
  margin-top: 10px;
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.places-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.places-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 12px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-size: 16px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.places-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
  width: 100%;
}

.place-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
  height: 100%;
}

.place-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.place-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.place-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.place-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.place-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.place-name {
  font-family: poppins;
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.place-category {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.place-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.place-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.place-rating .count {
  color: #666;
  font-size: 14px;
}

.place-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 100%;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.place-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.place-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .places-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .places-grid {
    grid-template-columns: 2fr;
    padding: 0 12px;
    margin-top: 90px;
    gap: 16px;
  }
  
  .place-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .place-card-content {
    padding: 14px;
  }
  
  .place-name {
    font-size: 16px;
  }
  
  .place-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .place-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .place-card-content {
    padding: 12px;
  }
  
  .place-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .places-filter-buttons {
    margin-top: 10px;
  }
  
  .places-filter-buttons {
    padding-bottom: 2px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .places-grid {
    margin-top: 130px;
  }
  
  .place-card {
    border-radius: 10px;
  }
}

.place-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .place-card,
  .place-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.place-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .place-card {
    background: #fff;
  }
  
  .place-name {
    color: #000;
  }
  
  .place-category,
  .place-description,
  .place-rating .count {
    color: #000;
  }
  
  .tag {
    background: #000;
    color: #ddd;
  }
  
  .tag:hover {
    background: #444;
  }
  
  .place-card-actions {
    border-top: 1px solid #333;
  }
  
  .action-button {
    color: #000;
  }
  
  .action-button i {
    color: #000;
  }
  
  .action-button:first-child {
    border-right: 1px solid #333;
  }
  
  .action-button:hover {
    background: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#placesExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#placesExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #placesExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #placesExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .places-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .places-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .place-card {
    width: 100%;
    margin: 0;
  }

  .places-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .places-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #placesExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}
  `;
  document.head.appendChild(style);
}


function renderPlaceCards(filteredData) {
  return filteredData.map(place => {
    const tagsHTML = place.details.tags && place.details.tags.length > 0 
      ? `<div class="tag-container">
          ${place.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    const actionButtonsHTML = `
      <a href="${place.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
        <i class="fas fa-map-marker-alt"></i> Map
      </a>
      <button class="action-button" onclick="openPlacePage('${place.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
    `;
    
    return `
      <div class="place-card" onclick="openPlacePage('${place.details.name}')">
        <div class="image-carousel">
          ${place.details.cardImages.map((image, index) => `
            <div class="carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${place.details.name}" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="carousel-pagination">
            ${place.details.cardImages.map((_, index) => `
              <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        
        <div class="place-card-content">
          <div class="place-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="place-name">${place.details.name}</h3>
              <div class="place-rating">
                <span class="star">${place.details.rating || '4.0'} ★</span>
              </div>
            </div>
            
            <div class="place-category">
              <span>${place.category} • ${place.metropolis}</span>
            </div>
            
            <div class="place-info-grid">
              <div class="info-item">
                <div class="info-icon">
                  <i class="fas fa-map-marker-alt"></i>
                </div>
                <div class="info-content">
                  <span class="info-label">Location & Hours</span>
                  <span class="info-value">${place.details.address}</span>
                  <span class="info-value" style="margin-top: 4px;">${place.details.timings}</span>
                </div>
              </div>
            </div>
            
            ${tagsHTML}
          </div>
        </div>

        <div class="place-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openPlacePage(placeName) {
  const place = publicPlacesData.find(pl => pl.details.name === placeName);
  if (!place) return;

  const placePage = document.createElement('div');
  placePage.className = 'place-page';
  
  // Main page structure
  placePage.innerHTML = `
    <div class="place-header">
      <button class="back-button" onclick="closePlacePage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${escapeHTML(place.details.name)}</h2>
      <div class="header-actions">
        <button class="share-button" onclick="shareLocation('${escapeHTML(place.details.name)}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="place-content">
      <!-- Hero Section -->
      <div class="place-hero">
        <div class="place-image-carousel">
          ${place.details.placeImages.map((image, index) => `
            <div class="place-carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${escapeHTML(place.details.name)}" class="place-image" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="place-carousel-pagination">
            ${place.details.placeImages.map((_, index) => `
              <div class="place-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentPlaceSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="place-carousel-control prev" onclick="changePlaceCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="place-carousel-control next" onclick="changePlaceCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        <div class="place-basic-info">
          <div class="place-star">
            <i class="fas fa-star"></i> ${place.details.rating} (${place.details.reviews})
          </div>
          <div class="place-type">
            ${place.category}
          </div>
          <div class="place-timings">
            <i class="fas fa-clock"></i> ${place.details.timings}
          </div>
        </div>
      </div>
      
      <!-- Description Section -->
      <div class="place-description-section">
        <h3>About</h3>
        <p>${escapeHTML(place.details.description)}</p>
        ${place.details.established ? `
          <div class="established-since">
            <i class="fas fa-calendar-alt"></i> Established: ${place.details.established}
          </div>
        ` : ''}
      </div>
      
      <!-- Location Section -->
      <div class="place-location-section">
        <h3>Location</h3>
        <div class="locations-grid">
          <div class="location-card">
            <div class="location-info">
              <h4>${escapeHTML(place.details.name)}</h4>
              <p class="location-address">
                <i class="fas fa-map-marker-alt"></i> ${escapeHTML(place.details.address)}
              </p>
              <p class="location-timings">
                <i class="fas fa-clock"></i> ${place.details.timings}
              </p>
            </div>
            <a href="${place.details.mapLink}" target="_blank" class="map-button">
              <i class="fas fa-map"></i> View Map
            </a>
          </div>
        </div>
      </div>
      
      <!-- Facilities Section -->
      <div class="place-facilities-section">
        <h3>Facilities</h3>
        <div class="facilities-grid">
          ${place.details.facilities?.map(facility => `
            <div class="facility-item">
              <div class="facility-icon">
                <i class="fas fa-${getFacilityIcon(facility.name)}"></i>
              </div>
              <div class="facility-info">
                <h4>${escapeHTML(facility.name)}</h4>
                <p>${escapeHTML(facility.description)}</p>
              </div>
            </div>
          `).join('') || '<p>No facilities information available</p>'}
        </div>
      </div>
      
      <!-- Category Specific Insights Section -->
      <div class="category-insights-section">
        <h3>${getCategoryInsightsTitle(place.category)}</h3>
        ${renderCategoryInsights(place)}
      </div>
      
      <!-- Events Section -->
      ${place.details.events?.length > 0 ? `
      <div class="place-events-section">
        <h3>Upcoming Events</h3>
        <div class="events-grid">
          ${place.details.events.map(event => `
            <div class="event-item">
              <div class="event-date">
                <span class="event-day">${getDayFromDate(event.date)}</span>
                <span class="event-month">${getMonthFromDate(event.date)}</span>
              </div>
              <div class="event-info">
                <h4>${escapeHTML(event.name)}</h4>
                <p>${escapeHTML(event.date)}</p>
              </div>
            </div>
          `).join('')}
        </div>
      </div>
      ` : ''}
      
      <!-- Contact Section -->
      <div class="place-contact-section">
        <h3>Contact</h3>
        <div class="contact-methods">
          ${place.details.support?.phone ? `
            <a href="tel:${place.details.support.phone}" class="contact-item">
              <i class="fas fa-phone"></i> ${place.details.support.phone}
            </a>
          ` : ''}
          ${place.details.support?.email ? `
            <a href="mailto:${place.details.support.email}" class="contact-item">
              <i class="fas fa-envelope"></i> ${place.details.support.email}
            </a>
          ` : ''}
          ${place.details.website ? `
            <a href="${place.details.website}" target="_blank" class="contact-item">
              <i class="fas fa-globe"></i> Visit Website
            </a>
          ` : ''}
        </div>
        
        <div class="social-links">
          ${place.details.social?.facebook ? `
            <a href="${place.details.social.facebook}" target="_blank" class="social-link">
              <i class="fab fa-facebook-f"></i>
            </a>
          ` : ''}
          ${place.details.social?.twitter ? `
            <a href="${place.details.social.twitter}" target="_blank" class="social-link">
              <i class="fab fa-twitter"></i>
            </a>
          ` : ''}
          ${place.details.social?.instagram ? `
            <a href="${place.details.social.instagram}" target="_blank" class="social-link">
              <i class="fab fa-instagram"></i>
            </a>
          ` : ''}
        </div>
      </div>
      
      <!-- Action Buttons -->
      <div class="place-actions">
        <a href="${place.details.mapLink}" target="_blank" class="primary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
        <button class="secondary-action" onclick="showChatSupport('${escapeHTML(place.details.name)}')">
          <i class="fas fa-comment-alt"></i> Contact Place
        </button>
      </div>
    </div>
  `;

  document.body.appendChild(placePage);
  addPlacePageStyles();
  initializePlaceCarousel();
  initializeTabs();
  initializeSearch();
  scrollToTop();
}

// Helper functions for place page
function getFacilityIcon(facilityName) {
  const icons = {
    'Parking': 'parking',
    'Wi-Fi': 'wifi',
    'Restrooms': 'restroom',
    'Food Stalls': 'utensils',
    'Guided Tours': 'map-marked-alt',
    'Accessibility': 'wheelchair',
    'Souvenir Shop': 'shopping-bag',
    'First Aid': 'first-aid',
    'Drinking Water': 'tint',
    'Picnic Area': 'umbrella-beach',
    'Library': 'book',
    'Sports Complex': 'running',
    'Emergency Services': 'ambulance',
    'Specialty Clinics': 'stethoscope',
    'Cardio Section': 'heartbeat',
    'Weight Training': 'dumbbell',
    'Banquet Halls': 'glass-cheers',
    'Catering': 'utensils',
    'Dolby Atmos': 'volume-up',
    'Food Court': 'hamburger',
    'Spa': 'spa',
    'Fine Dining': 'wine-glass-alt',
    'Valet Parking': 'parking',
    'Kids Play Area': 'child'
  };
  return icons[facilityName] || 'check-circle';
}

function getCategoryInsightsTitle(category) {
  const titles = {
    'Park': 'Activities & Features',
    'School': 'Admissions & Academic Info',
    'Hospital': 'Medical Services & Facilities',
    'Gym': 'Membership & Training Plans',
    'Wedding Venue': 'Event Hosting Info',
    'Movie Theater': 'Now Showing & Booking',
    'Hotel & Resort': 'Bookings & Packages',
    'Shopping Mall': 'Mall Directory & Offers',
    'Museum': 'Exhibitions & Collections',
    'Airport': 'Traveler Services'
  };
  return titles[category] || 'Additional Information';
}

function renderCategoryInsights(place) {
  if (!place.details.categoryInsights) return '<p>No additional information available</p>';
  
  const insights = place.details.categoryInsights;
  let html = '<div class="insights-grid">';
  
  // School specific template
  if (place.category === 'School') {
    html += `
      <div class="insight-item">
        <h4>Admission Status</h4>
        <p>${insights.admissionStatus || 'Not specified'}</p>
      </div>
      <div class="insight-item">
        <h4>Fee Structure</h4>
        ${Object.entries(insights.feeStructure || {}).map(([grade, fee]) => `
          <p><strong>${grade}:</strong> ${fee}</p>
        `).join('')}
      </div>
      <div class="insight-item">
        <h4>Curriculum</h4>
        <p>${insights.curriculum || 'Not specified'}</p>
      </div>
      <div class="insight-item">
        <h4>Classes Offered</h4>
        <p>${insights.classesOffered || 'Not specified'}</p>
      </div>
      ${insights.entranceExam ? `
      <div class="insight-item">
        <h4>Entrance Exam</h4>
        <p>${insights.entranceExam}</p>
      </div>
      ` : ''}
      ${insights.facilities?.length > 0 ? `
      <div class="insight-item">
        <h4>Facilities</h4>
        <ul>${insights.facilities.map(f => `<li>${f}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Hospital specific template
  else if (place.category === 'Hospital') {
    html += `
      <div class="insight-item">
        <h4>Departments</h4>
        <ul>${(insights.departments || []).map(dept => `<li>${dept}</li>`).join('')}</ul>
      </div>
      <div class="insight-item">
        <h4>Consultation Charges</h4>
        ${Object.entries(insights.consultationCharges || {}).map(([type, charge]) => `
          <p><strong>${type}:</strong> ${charge}</p>
        `).join('')}
      </div>
      <div class="insight-item">
        <h4>Emergency Services</h4>
        <p>${insights.emergency || '24/7 emergency services'}</p>
      </div>
      ${insights.healthPackages?.length > 0 ? `
      <div class="insight-item">
        <h4>Health Packages</h4>
        <ul>${insights.healthPackages.map(pkg => `<li>${pkg}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Gym specific template
  else if (place.category === 'Gym') {
    html += `
      <div class="insight-item">
        <h4>Membership Plans</h4>
        ${Object.entries(insights.membershipPlans || {}).map(([plan, price]) => `
          <p><strong>${plan}:</strong> ${price}</p>
        `).join('')}
      </div>
      ${insights.trainers?.length > 0 ? `
      <div class="insight-item">
        <h4>Trainers</h4>
        <ul>${insights.trainers.map(trainer => `<li>${trainer}</li>`).join('')}</ul>
      </div>
      ` : ''}
      <div class="insight-item">
        <h4>Class Schedule</h4>
        ${Object.entries(insights.classSchedule || {}).map(([cls, time]) => `
          <p><strong>${cls}:</strong> ${time}</p>
        `).join('')}
      </div>
      ${insights.personalTraining ? `
      <div class="insight-item">
        <h4>Personal Training</h4>
        <p>${insights.personalTraining}</p>
      </div>
      ` : ''}
    `;
  }
  // Wedding Venue specific template
  else if (place.category === 'Wedding Venue') {
    html += `
      <div class="insight-item">
        <h4>Venue Capacity</h4>
        ${Object.entries(insights.venueCapacity || {}).map(([venue, capacity]) => `
          <p><strong>${venue}:</strong> ${capacity}</p>
        `).join('')}
      </div>
      <div class="insight-item">
        <h4>Package Rates</h4>
        ${Object.entries(insights.packageRates || {}).map(([pkg, rate]) => `
          <p><strong>${pkg}:</strong> ${rate}</p>
        `).join('')}
      </div>
      ${insights.cateringOptions?.length > 0 ? `
      <div class="insight-item">
        <h4>Catering Options</h4>
        <ul>${insights.cateringOptions.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Movie Theater specific template
  else if (place.category === 'Movie Theater') {
    html += `
      ${insights.currentMovies?.length > 0 ? `
      <div class="insight-item">
        <h4>Now Showing</h4>
        ${insights.currentMovies.map(movie => `
          <p><strong>${movie.title}</strong> (${movie.screenType})</p>
          <p>${movie.timings.join(', ')}</p>
        `).join('')}
      </div>
      ` : ''}
      <div class="insight-item">
        <h4>Ticket Pricing</h4>
        ${Object.entries(insights.ticketPricing || {}).map(([type, price]) => `
          <p><strong>${type}:</strong> ${price}</p>
        `).join('')}
      </div>
      ${insights.foodMenu?.combos?.length > 0 ? `
      <div class="insight-item">
        <h4>Food Combos</h4>
        <ul>${insights.foodMenu.combos.map(combo => `<li>${combo}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Hotel specific template
  else if (place.category === 'Hotel & Resort') {
    html += `
      ${insights.roomTypes?.length > 0 ? `
      <div class="insight-item">
        <h4>Room Types</h4>
        ${insights.roomTypes.map(room => `
          <p><strong>${room.name}:</strong> ${room.price} (${room.capacity})</p>
        `).join('')}
      </div>
      ` : ''}
      ${insights.diningOptions?.length > 0 ? `
      <div class="insight-item">
        <h4>Dining Options</h4>
        <ul>${insights.diningOptions.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.spaServices?.length > 0 ? `
      <div class="insight-item">
        <h4>Spa Services</h4>
        <ul>${insights.spaServices.map(service => `<li>${service}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Shopping Mall specific template
  else if (place.category === 'Shopping Mall') {
    html += `
      ${insights.storeDirectory?.length > 0 ? `
      <div class="insight-item">
        <h4>Store Directory</h4>
        ${insights.storeDirectory.map(category => `
          <p><strong>${category.category}:</strong> ${category.brands.join(', ')}</p>
        `).join('')}
      </div>
      ` : ''}
      ${insights.foodCourt?.length > 0 ? `
      <div class="insight-item">
        <h4>Food Court</h4>
        <ul>${insights.foodCourt.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.ongoingOffers?.length > 0 ? `
      <div class="insight-item">
        <h4>Ongoing Offers</h4>
        <ul>${insights.ongoingOffers.map(offer => `<li>${offer}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Museum specific template
  else if (place.category === 'Museum') {
    html += `
      ${insights.collections?.length > 0 ? `
      <div class="insight-item">
        <h4>Collections</h4>
        <ul>${insights.collections.map(collection => `<li>${collection}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.guidedTourTimings?.length > 0 ? `
      <div class="insight-item">
        <h4>Guided Tour Timings</h4>
        <ul>${insights.guidedTourTimings.map(tour => `<li>${tour}</li>`).join('')}</ul>
      </div>
      ` : ''}
      <div class="insight-item">
        <h4>Ticket Pricing</h4>
        ${Object.entries(insights.ticketPricing || {}).map(([type, price]) => `
          <p><strong>${type}:</strong> ${price}</p>
        `).join('')}
      </div>
    `;
  }
  // Airport specific template
  else if (place.category === 'Airport') {
    html += `
      ${insights.terminals?.length > 0 ? `
      <div class="insight-item">
        <h4>Terminals</h4>
        ${insights.terminals.map(terminal => `
          <p><strong>${terminal.name}:</strong> ${terminal.airlines}</p>
          <p>Check-in: ${terminal.checkIn}</p>
        `).join('')}
      </div>
      ` : ''}
      ${insights.transportation?.length > 0 ? `
      <div class="insight-item">
        <h4>Transportation</h4>
        <ul>${insights.transportation.map(option => `<li>${option}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.services?.length > 0 ? `
      <div class="insight-item">
        <h4>Services</h4>
        <ul>${insights.services.map(service => `<li>${service}</li>`).join('')}</ul>
      </div>
      ` : ''}
    `;
  }
  // Park specific template
  else if (place.category === 'Park') {
    html += `
      ${insights.activities?.length > 0 ? `
      <div class="insight-item">
        <h4>Activities</h4>
        <ul>${insights.activities.map(activity => `<li>${activity}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.historicalMonuments?.length > 0 ? `
      <div class="insight-item">
        <h4>Historical Monuments</h4>
        <ul>${insights.historicalMonuments.map(monument => `<li>${monument}</li>`).join('')}</ul>
      </div>
      ` : ''}
      ${insights.bestTimeToVisit ? `
      <div class="insight-item">
        <h4>Best Time to Visit</h4>
        <p>${insights.bestTimeToVisit}</p>
      </div>
      ` : ''}
    `;
  }
  // Generic template for other categories
  else {
    html += `
      ${Object.entries(insights).map(([key, value]) => {
        if (Array.isArray(value)) {
          return `
            <div class="insight-item">
              <h4>${key.replace(/([A-Z])/g, ' $1').replace(/^./, str => str.toUpperCase())}</h4>
              <ul>${value.map(item => `<li>${item}</li>`).join('')}</ul>
            </div>
          `;
        } else if (typeof value === 'object' && value !== null) {
          return `
            <div class="insight-item">
              <h4>${key.replace(/([A-Z])/g, ' $1').replace(/^./, str => str.toUpperCase())}</h4>
              ${Object.entries(value).map(([subKey, subValue]) => `
                <p><strong>${subKey}:</strong> ${subValue}</p>
              `).join('')}
            </div>
          `;
        } else {
          return `
            <div class="insight-item">
              <h4>${key.replace(/([A-Z])/g, ' $1').replace(/^./, str => str.toUpperCase())}</h4>
              <p>${value}</p>
            </div>
          `;
        }
      }).join('')}
    `;
  }
  
  html += '</div>';
  return html;
}

function getDayFromDate(dateString) {
  // Simple implementation - extract day number
  const dayMatch = dateString.match(/(\d+)(?:th|st|nd|rd)/);
  return dayMatch ? dayMatch[1] : dateString.split(' ')[0] || '';
}

function getMonthFromDate(dateString) {
  // Simple implementation - extract month name
  const monthMatch = dateString.match(/(January|February|March|April|May|June|July|August|September|October|November|December)/);
  return monthMatch ? monthMatch[1].substring(0, 3) : dateString.split(' ')[1] || '';
}

function escapeHTML(str) {
  if (!str) return '';
  return str.toString()
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#039;');
}

// Initialize place carousel similar to store carousel
function initializePlaceCarousel() {
  const carousel = document.querySelector('.place-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.place-carousel-item');
  const dots = carousel.querySelectorAll('.place-pagination-dot');
  let currentIndex = 0;

  function showSlide(index) {
    items.forEach((item, i) => {
      item.classList.remove('active');
      dots[i].classList.remove('active');
      if (i === index) {
        item.classList.add('active');
        dots[i].classList.add('active');
      }
    });
    currentIndex = index;
  }

  function nextSlide() {
    const newIndex = (currentIndex + 1) % items.length;
    showSlide(newIndex);
  }

  function prevSlide() {
    const newIndex = (currentIndex - 1 + items.length) % items.length;
    showSlide(newIndex);
  }

  // Auto-rotate every 5 seconds
  let interval = setInterval(nextSlide, 5000);

  // Pause on hover
  carousel.addEventListener('mouseenter', () => {
    clearInterval(interval);
  });

  carousel.addEventListener('mouseleave', () => {
    interval = setInterval(nextSlide, 5000);
  });
}

function changePlaceCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.place-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.place-carousel-item');
  const dots = carousel.querySelectorAll('.place-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentPlaceSlide(index) {
  const carousel = event.target.closest('.place-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.place-carousel-item');
  const dots = carousel.querySelectorAll('.place-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function closePlacePage() {
  const placePage = document.querySelector('.place-page');
  if (placePage) {
    placePage.remove();
  }
}

function addPlacePageStyles() {
  const style = document.createElement('style');
  style.textContent = `
/* Place Page Container */
.place-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #1a1a1f;
  z-index: 2000;
  overflow-y: auto;
  color: #fff;
  font-family: 'poppins';
}

/* Place Header */
.place-header {
  position: sticky;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  z-index: 10;
  backdrop-filter: blur(12px);
}

.back-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.5em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.place-header h2 {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  text-align: center;
  flex: 1;
  padding: 0 16px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header-actions {
  display: flex;
  gap: 8px;
}

.share-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.share-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

/* Place Content */
.place-content {
  padding: 0 0 100px;
  width: 100%;
  margin: 0 auto;
}

/* Hero Section */
.place-hero {
  position: relative;
  padding: 20px;
}

.place-image-carousel {
  position: relative;
  width: 100%;
  height: 280px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  margin-bottom: 16px;
}

.place-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.place-carousel-item.active {
  opacity: 1;
}

.place-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.place-carousel-item.active img:hover {
  transform: scale(1.02);
}

.place-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.7;
  transition: all 0.3s ease;
  z-index: 5;
}

.place-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
  opacity: 1;
  transform: translateY(-50%) scale(1.1);
}

.place-carousel-control.prev {
  left: 16px;
}

.place-carousel-control.next {
  right: 16px;
}

.place-carousel-pagination {
  position: absolute;
  bottom: 16px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.place-pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
}

.place-pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
}

.place-basic-info {
  display: flex;
  gap: 12px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.place-star,
.place-type,
.place-timings {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 0.9em;
  font-weight: 500;
}

.place-star {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
}

.place-type {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.place-timings {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.established-since {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-top: 12px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

/* Description Section */
.place-description-section {
  padding: 0 20px 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-description-section h3 {
  margin: 0 0 12px;
  font-size: 1.2em;
  font-weight: 600;
}

.place-description-section p {
  margin: 0;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
}

/* Location Section */
.place-location-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-location-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.locations-grid {
  display: grid;
  gap: 16px;
}

.location-card {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.location-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.location-info h4 {
  margin: 0 0 8px;
  font-size: 1.1em;
  font-weight: 600;
}

.location-address,
.location-timings {
  margin: 4px 0;
  display: flex;
  align-items: center;
  gap: 8px;
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.9em;
}

.map-button {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 10px 16px;
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
  border-radius: 8px;
  text-decoration: none;
  font-size: 0.9em;
  font-weight: 500;
  transition: background 0.3s ease;
  margin-top: 8px;
  justify-content: center;
}

.map-button:hover {
  background: rgba(255, 215, 0, 0.2);
}

/* Facilities Section */
.place-facilities-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-facilities-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.facilities-grid {
  display: grid;
  gap: 16px;
}

.facility-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.facility-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.facility-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-size: 1.2em;
}

.facility-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.facility-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

/* Events Section */
.place-events-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-events-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.events-grid {
  display: grid;
  gap: 16px;
}

.event-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.event-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.event-date {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-weight: 600;
}

.event-day {
  font-size: 1.2em;
  line-height: 1;
}

.event-month {
  font-size: 0.8em;
  line-height: 1;
  text-transform: uppercase;
}

.event-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.event-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

/* Contact Section */
.place-contact-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.place-contact-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.contact-methods {
  display: grid;
  gap: 12px;
  margin-bottom: 20px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  color: #fff;
  text-decoration: none;
  transition: background 0.3s ease;
}

.contact-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.social-links {
  display: flex;
  gap: 16px;
  margin-top: 20px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
  color: #fff;
  font-size: 1.2em;
  transition: transform 0.3s ease, background 0.3s ease;
}

.social-link:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.1);
}

/* Action Buttons */
.place-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

/* Responsive Styles */
@media (min-width: 768px) {
  .place-header {
    padding: 20px 30px;
  }
  
  .place-header h2 {
    font-size: 1.4em;
  }
  
  .place-content {
    max-width: 90%;
    margin: 0 auto;
  }
  
  .place-hero {
    padding: 30px;
  }
  
  .place-image-carousel {
    height: 350px;
    border-radius: 20px;
  }
  
  .place-basic-info {
    gap: 16px;
    margin-top: 24px;
  }
  
  .place-star,
  .place-type,
  .place-timings {
    padding: 10px 16px;
    font-size: 1em;
  }
  
  .place-description-section,
  .place-location-section,
  .place-facilities-section,
  .place-events-section,
  .place-contact-section {
    padding: 0 30px 30px;
  }
  
  .place-description-section h3,
  .place-location-section h3,
  .place-facilities-section h3,
  .place-events-section h3,
  .place-contact-section h3 {
    font-size: 1.3em;
    margin-bottom: 16px;
  }
  
  .place-description-section p {
    font-size: 1em;
    line-height: 1.6;
  }
  
  .locations-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .facilities-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .events-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
  }
  
  .place-actions {
    padding: 20px 30px;
    max-width: 90%;
    margin: 0 auto;
    left: 50%;
    transform: translateX(-50%);
    border-radius: 12px 12px 0 0;
  }
  
  .primary-action,
  .secondary-action {
    padding: 14px;
    font-size: 1em;
  }
}

@media (min-width: 1024px) {
  .place-content {
    max-width: 900px;
    margin: 0 auto;
    padding: 0 0 120px;
  }
  
  .place-header {
    padding: 22px 32px;
  }
  
  .place-header h2 {
    font-size: 1.5em;
  }
  
  .place-hero {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    padding: 30px;
    align-items: center;
  }
  
  .place-image-carousel {
    height: 400px;
    border-radius: 20px;
  }
  
  .place-hero-content {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }
  
  .place-name {
    font-size: 1.8em;
    font-weight: 600;
    margin: 0;
  }
  
  .place-basic-info {
    margin-top: 0;
  }
  
  .place-description-section {
    padding: 0 30px 30px;
    max-width: 85%;
    margin: 0 auto;
  }
  
  .place-description-section h3 {
    font-size: 1.5em;
  }
  
  .place-description-section p {
    font-size: 1.05em;
    line-height: 1.7;
  }
  
  .place-location-section,
  .place-facilities-section,
  .place-events-section,
  .place-contact-section {
    padding: 30px;
  }
  
  .place-location-section h3,
  .place-facilities-section h3,
  .place-events-section h3,
  .place-contact-section h3 {
    font-size: 1.5em;
  }
  
  .locations-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .events-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .social-links {
    gap: 18px;
  }
  
  .social-link {
    width: 44px;
    height: 44px;
    font-size: 1.3em;
  }
  
  .place-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
}

@media (min-width: 1440px) {
  .place-content {
    max-width: 1000px;
  }
  
  .place-hero {
    grid-template-columns: 3fr 2fr;
  }
  
  .place-image-carousel {
    height: 450px;
  }
  
  .place-name {
    font-size: 2em;
  }
  
  .place-description-section {
    max-width: 80%;
  }
  
  .locations-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .facilities-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .events-grid {
    grid-template-columns: repeat(3, 1fr);
  }
  
  .place-actions {
    max-width: 800px;
    left: 50%;
    transform: translateX(-50%);
  }
}

    /* Category Insights Section */
    .category-insights-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .category-insights-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
      color: var(--primary-color, #ffd700);
    }
    
    .insights-grid {
      display: grid;
      gap: 16px;
    }
    
    .insight-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      padding: 16px;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    
    .insight-item:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
    }
    
    .insight-item h4 {
      margin: 0 0 8px;
      font-size: 1em;
      font-weight: 600;
      color: #fff;
    }
    
    .insight-item p, .insight-item li {
      margin: 4px 0;
      color: rgba(255, 255, 255, 0.8);
      font-size: 0.9em;
      line-height: 1.5;
    }
    
    .insight-item ul {
      padding-left: 20px;
      margin: 8px 0;
    }
    
    @media (min-width: 768px) {
      .insights-grid {
        grid-template-columns: repeat(2, 1fr);
        gap: 20px;
      }
      
      .category-insights-section {
        padding: 30px;
      }
    }
    
    @media (min-width: 1024px) {
      .insights-grid {
        grid-template-columns: repeat(3, 1fr);
      }
    }

  `;
  document.head.appendChild(style);
}

// Helper functions for place page
function getFacilityIcon(facilityName) {
  const icons = {
    'Parking': 'parking',
    'Wi-Fi': 'wifi',
    'Restrooms': 'restroom',
    'Food Stalls': 'utensils',
    'Guided Tours': 'map-marked-alt',
    'Accessibility': 'wheelchair',
    'Souvenir Shop': 'shopping-bag',
    'First Aid': 'first-aid',
    'Drinking Water': 'tint',
    'Picnic Area': 'umbrella-beach'
  };
  
  return icons[facilityName] || 'check-circle';
}

function getDayFromDate(dateString) {
  // Extract day from date string (simple implementation)
  const parts = dateString.split(/[\s,]+/);
  return parts[0] || '';
}

function getMonthFromDate(dateString) {
  // Extract month from date string (simple implementation)
  const parts = dateString.split(/[\s,]+/);
  return parts[1] || '';
}

// Helper function to escape HTML special characters
function escapeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

// Function to filter places based on category
function filterPlaces(category) {
  const placesGrid = document.getElementById('placesGrid');
  let filteredData = publicPlacesData;

  if (category !== 'All') {
    filteredData = filteredData.filter(place => place.category === category);
  }

  placesGrid.innerHTML = renderPlaceCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

// Additional placeholder functions (you might want to implement these)
function selectPlace(category) {
  console.log(`Selected place in category: ${category}`);
}

function selectPlace(category) {
  // Optional: Add any specific action when a place card is clicked
  console.log(`Selected place in category: ${category}`);
}

const serviceProvidersData = [
  // Delhi - Plumber
  {
    category: "Plumber",
    metropolis: "Delhi",
    details: {
      name: "Delhi Plumbing Services",
      cardImages: [
        "https://www.serviceonwheel.com/uploads/service/834431670584630.jpg",
        "https://example.com/plumber-card2.jpg"
      ],
      providerImages: [
        "https://5.imimg.com/data5/PX/TS/AC/SELLER-8453614/plumber-500x500.jpg",
        "https://example.com/plumber2.jpg",
        "https://example.com/plumber3.jpg"
      ],
      description: "Professional plumbing services for homes and businesses with 24/7 emergency support.",
      address: "12, Karol Bagh, Delhi - 110005",
      timings: "24/7 Emergency Service",
      entryFee: "Visiting Fees: ₹500",
      mapLink: "https://maps.example.com/plumber",
      website: "https://www.delhiplumbing.com",
      social: {
        facebook: "https://facebook.com/delhiplumbing",
        twitter: "https://twitter.com/delhiplumbing",
        instagram: "https://instagram.com/delhiplumbing"
      },
      facilities: [
        { name: "Emergency Service", description: "Available 24 hours, 7 days a week" },
        { name: "Guarantee", description: "1-year guarantee on all repairs" },
        { name: "Equipment", description: "Modern tools and diagnostic equipment" }
      ],
      services: [
        { 
          name: "Pipe Repair", 
          price: "₹800-₹2000", 
          image: "https://example.com/pipe-repair.jpg",
          description: "Fix for leaking or burst pipes",
          variants: [
            { name: "Basic Repair", price: "₹800" },
            { name: "Standard Repair", price: "₹1200" },
            { name: "Complex Repair", price: "₹2000" }
          ]
        },
        { 
          name: "Drain Cleaning", 
          price: "₹500-₹1500", 
          image: "https://example.com/drain-cleaning.jpg",
          description: "Unclog sinks, showers, and toilets",
          variants: [
            { name: "Basic Cleaning", price: "₹500" },
            { name: "Standard Cleaning", price: "₹800" },
            { name: "Deep Cleaning", price: "₹1500" }
          ]
        },
        { 
          name: "Water Heater", 
          price: "₹2000-₹5000", 
          image: "https://example.com/water-heater.jpg",
          description: "Installation and repair",
          variants: [
            { name: "Repair", price: "₹2000" },
            { name: "Standard Installation", price: "₹3500" },
            { name: "Premium Installation", price: "₹5000" }
          ]
        }
      ],
      promos: [
        { name: "First Time Customer", description: "10% off your first service", code: "PLUMB10" }
      ],
      support: {
        email: "support@delhiplumbing.com",
        phone: "+91-9876543210",
        whatsapp: "+91-7869809022",
        primaryContact: "whatsapp"
      },
      rating: "4.8",
      reviews: "1200+",
      established: "2010",
      tags: ["24/7 Service", "Licensed", "Free Estimates"]
    }
  },
  // Other service providers...
];

// Global booking state
// Global booking state
let bookingState = {
  items: [],
  provider: null,
  total: 0,
  discountedTotal: 0,  // Add this line
  directBookingItems: null,
  appliedCoupon: null  // Add this to track applied coupon
};

function showProvidersOverlay() {
  let overlay = document.getElementById('providersOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'providersOverlay';
    overlay.className = 'providers-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(serviceProvidersData.map(provider => provider.category))];

  overlay.innerHTML = `
    <div class="providers-content">
      <div class="providers-header">
        <div class="header-title-container">
          <h2>Service Providers</h2>
          <div class="header-subtitle">Find trusted professionals</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-providers" onclick="hideProvidersOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="sortOptionsContainer">
        <div class="sort-option" onclick="sortProviders('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortProviders('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortProviders('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="providers-filter-container">
        <div class="providers-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterProviders('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="providers-grid" id="providersGrid">
        ${renderProviderCards(serviceProvidersData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="providersExploreInput" placeholder="Search for plumbers, electricians..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  const exploreInput = document.getElementById('providersExploreInput');
  const providersGrid = document.getElementById('providersGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(serviceProvidersData, query, providersGrid, filterButtons, renderProviderCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(serviceProvidersData, exploreInput.value, providersGrid, filterButtons, renderProviderCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(serviceProvidersData, query, providersGrid, filterButtons, renderProviderCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
.providers-overlay {
  font-family: poppins;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.providers-overlay::-webkit-scrollbar {
  display: none;
}

.providers-overlay.active {
  opacity: 1;
  visibility: visible;
}

.providers-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.providers-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.providers-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-providers {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-providers:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-providers:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.button-text {
  font-weight: 500;
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.providers-filter-container {
  position: fixed;
  top: 64px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.providers-filter-buttons {
  margin-top: 10px;
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.providers-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.providers-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 12px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-size: 16px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.providers-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
  width: 100%;
}

.provider-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
  height: 100%;
}

.provider-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.provider-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.provider-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.provider-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.provider-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.provider-name {
  font-family: poppins;
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.provider-category {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.provider-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.provider-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.provider-rating .count {
  color: #666;
  font-size: 14px;
}

.provider-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 100%;
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.provider-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.provider-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .providers-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .providers-grid {
    grid-template-columns: 2fr;
    padding: 0 12px;
    margin-top: 90px;
    gap: 16px;
  }
  
  .provider-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .provider-card-content {
    padding: 14px;
  }
  
  .provider-name {
    font-size: 16px;
  }
  
  .provider-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .provider-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .provider-card-content {
    padding: 12px;
  }
  
  .provider-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .providers-filter-buttons {
    margin-top: 10px;
  }
  
  .providers-filter-buttons {
    padding-bottom: 2px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .providers-grid {
    margin-top: 130px;
  }
  
  .provider-card {
    border-radius: 10px;
  }
}

.provider-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .provider-card,
  .provider-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.provider-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .provider-card {
    background: #fff;
  }
  
  .provider-name {
    color: #000;
  }
  
  .provider-category,
  .provider-description,
  .provider-rating .count {
    color: #000;
  }
  
  .tag {
    background: #000;
    color: #ddd;
  }
  
  .tag:hover {
    background: #444;
  }
  
  .provider-card-actions {
    border-top: 1px solid #333;
  }
  
  .action-button {
    color: #000;
  }
  
  .action-button i {
    color: #000;
  }
  
  .action-button:first-child {
    border-right: 1px solid #333;
  }
  
  .action-button:hover {
    background: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#providersExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#providersExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #providersExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #providersExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .providers-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .providers-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .provider-card {
    width: 100%;
    margin: 0;
  }

  .providers-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .providers-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #providersExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}
  `;
  document.head.appendChild(style);
}

function renderProviderCards(filteredData) {
  return filteredData.map(provider => {
    const tagsHTML = provider.details.tags && provider.details.tags.length > 0 
      ? `<div class="tag-container">
          ${provider.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    const actionButtonsHTML = `
      <a href="${provider.details.mapLink}" target="_blank" class="action-button" onclick="event.stopPropagation()">
        <i class="fas fa-map-marker-alt"></i> Map
      </a>
      <button class="action-button" onclick="openProviderPage('${provider.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
      <button class="action-button" onclick="showChatSupport('${provider.details.name}'); event.stopPropagation()">
        <i class="fas fa-comments"></i> Chat
      </button>
    `;
    
    return `
      <div class="provider-card" onclick="openProviderPage('${provider.details.name}')">
        <div class="image-carousel">
          ${provider.details.cardImages.map((image, index) => `
            <div class="carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${provider.details.name}" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="carousel-pagination">
            ${provider.details.cardImages.map((_, index) => `
              <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        
        <div class="provider-card-content">
          <div class="provider-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="provider-name">${provider.details.name}</h3>
              <div class="provider-rating">
                <span class="star">${provider.details.rating || '4.0'} ★</span>
              </div>
            </div>
            
            <div class="provider-category">
              <span>${provider.category} • ${provider.metropolis}</span>
            </div>
            
            <div class="provider-info-grid">
              <div class="info-item">
                <div class="info-icon">
                  <i class="fas fa-map-marker-alt"></i>
                </div>
                <div class="info-content">
                  <span class="info-label">Location & Hours</span>
                  <span class="info-value">${provider.details.address}</span>
                  <span class="info-value" style="margin-top: 4px;">${provider.details.timings}</span>
                </div>
              </div>
            </div>
            
            ${tagsHTML}
          </div>
        </div>

        <div class="provider-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openProviderPage(providerName) {
  const provider = serviceProvidersData.find(prov => prov.details.name === providerName);
  if (!provider) return;

  // Set the current provider in booking state
  bookingState.provider = provider;

  const providerPage = document.createElement('div');
  providerPage.className = 'provider-page';
  providerPage.innerHTML = `
    <div class="provider-header">
      <button class="back-button" onclick="closeProviderPage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${provider.details.name}</h2>
      <div class="header-actions">
        <button class="cart-button" onclick="showBookingCart()">
          <i class="fas fa-shopping-cart"></i>
          ${bookingState.items.length > 0 ? `<span class="cart-count">${bookingState.items.length}</span>` : ''}
        </button>
        <button class="share-button" onclick="shareProvider('${provider.details.name}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
    
    <div class="provider-content">
      <!-- Hero Section with Provider Image Carousel -->
      <div class="provider-hero">
        <div class="provider-image-carousel">
          ${provider.details.providerImages.map((image, index) => `
            <div class="provider-carousel-item ${index === 0 ? 'active' : ''}">
              <img src="${image}" alt="${provider.details.name}" class="provider-image" onclick="openFullScreen('${image}')">
            </div>
          `).join('')}
          <div class="provider-carousel-pagination">
            ${provider.details.providerImages.map((_, index) => `
              <div class="provider-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentProviderSlide(${index + 1}); event.stopPropagation()"></div>
            `).join('')}
          </div>
          <button class="provider-carousel-control prev" onclick="changeProviderCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
          <button class="provider-carousel-control next" onclick="changeProviderCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
        </div>
        <div class="provider-basic-info">
          <div class="provider-star">
            <i class="fas fa-star"></i> ${provider.details.rating} (${provider.details.reviews})
          </div>
          <div class="provider-type">
            ${provider.category}
          </div>
          <div class="provider-timings">
            <i class="fas fa-clock"></i> ${provider.details.timings}
          </div>
          <div class="visiting-fees">
            <i class="fas fa-money-bill-wave"></i>${provider.details.entryFee}
          </div>
        </div>
      </div>
      
      <!-- Provider Description -->
      <div class="provider-description-section">
        <h3>About</h3>
        <p>${provider.details.description}</p>
        <div class="established-since">
          <i class="fas fa-calendar-alt"></i> Established: ${provider.details.established}
        </div>
      </div>
      
      <!-- Services Section -->
    <div class="provider-services-section">
      <h3>Services</h3>
      <div class="services-container">
        ${renderProviderServices(provider.details.services)}
      </div>
      </div>
      
      <!-- Facilities Section -->
      <div class="provider-facilities-section">
        <h3>Why Choose Us</h3>
        <div class="facilities-grid">
          ${provider.details.facilities?.map(facility => `
            <div class="facility-item">
              <div class="facility-icon">
                <i class="fas fa-${getFacilityIcon(facility.name)}"></i>
              </div>
              <div class="facility-info">
                <h4>${facility.name}</h4>
                <p>${facility.description}</p>
              </div>
            </div>
          `).join('') || '<p>No facilities information available</p>'}
        </div>
      </div>
      
      <!-- Promotions Section -->
      <div class="provider-promos-section">
        <h3>Special Offers</h3>
        <div class="promos-grid">
          ${provider.details.promos?.map(promo => `
            <div class="promo-item">
              <div class="promo-info">
                <h4>${promo.name}</h4>
                <p>${promo.description}</p>
              </div>
              <div class="promo-code">
                <span>${promo.code}</span>
                <button onclick="copyToClipboard('${promo.code}')">Copy</button>
              </div>
            </div>
          `).join('') || '<p>No current promotions</p>'}
        </div>
      </div>
      
      <!-- Contact Section -->
      <div class="provider-contact-section">
        <h3>Contact</h3>
        <div class="contact-methods">
          ${provider.details.support?.phone ? `
            <a href="tel:${provider.details.support.phone}" class="contact-item">
              <i class="fas fa-phone"></i> ${provider.details.support.phone}
            </a>
          ` : ''}
          ${provider.details.support?.whatsapp ? `
            <a href="https://wa.me/${provider.details.support.whatsapp}" target="_blank" class="contact-item">
              <i class="fab fa-whatsapp"></i> WhatsApp
            </a>
          ` : ''}
          ${provider.details.support?.email ? `
            <a href="mailto:${provider.details.support.email}" class="contact-item">
              <i class="fas fa-envelope"></i> ${provider.details.support.email}
            </a>
          ` : ''}
          ${provider.details.website ? `
            <a href="${provider.details.website}" target="_blank" class="contact-item">
              <i class="fas fa-globe"></i> Visit Website
            </a>
          ` : ''}
        </div>
        
        <div class="social-links">
          ${provider.details.social?.facebook ? `
            <a href="${provider.details.social.facebook}" target="_blank" class="social-link">
              <i class="fab fa-facebook-f"></i>
            </a>
          ` : ''}
          ${provider.details.social?.twitter ? `
            <a href="${provider.details.social.twitter}" target="_blank" class="social-link">
              <i class="fab fa-twitter"></i>
            </a>
          ` : ''}
          ${provider.details.social?.instagram ? `
            <a href="${provider.details.social.instagram}" target="_blank" class="social-link">
              <i class="fab fa-instagram"></i>
            </a>
          ` : ''}
        </div>
      </div>
      
      <!-- Provider Action Buttons -->
      <div class="provider-actions">
        <a href="${provider.details.mapLink}" target="_blank" class="primary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
        <button class="secondary-action" onclick="showChatSupport('${provider.details.name}')">
          <i class="fas fa-comment-alt"></i> Contact
        </button>
      </div>
    </div>
    
    <!-- Booking Cart Overlay -->
    <div class="booking-cart-overlay" id="bookingCartOverlay">
      <div class="booking-cart-container">
        <div class="booking-cart-header">
          <h3>Your Booking Cart</h3>
          <button class="close-booking-cart" onclick="hideBookingCart()">
            <i class="fas fa-times"></i>
          </button>
        </div>
        
        <div class="booking-cart-items" id="bookingCartItems">
          ${renderBookingCartItems()}
        </div>
        
        <div class="booking-cart-summary">
          <div class="booking-cart-total">
            <span>Total:</span>
            <span id="bookingCartTotalAmount">₹${bookingState.total.toFixed(2)}</span>
          </div>
          <div class="booking-cart-actions">
            <button class="clear-booking-cart" onclick="clearBookingCart()">
              <i class="fas fa-trash"></i> Clear Cart
            </button>
            <button class="checkout-booking-btn" onclick="showBookingForm()">
              <i class="fas fa-calendar-check"></i> Book Now
            </button>
          </div>
        </div>
      </div>
    </div>
    
    <!-- Booking Form Overlay -->
    <div class="booking-form-overlay" id="bookingFormOverlay">
      <div class="booking-form-container">
        <button class="close-booking-form" onclick="closeBookingForm()">
          <i class="fas fa-times"></i>
        </button>
        <h3>Book Services</h3>
        <form id="bookingForm">
          <div class="form-group">
            <label for="customerName">Full Name</label>
            <input type="text" id="customerName" required>
          </div>
          <div class="form-group">
            <label for="customerPhone">Phone Number</label>
            <input type="tel" id="customerPhone" required>
          </div>
          <div class="form-group">
            <label for="customerAddress">Service Address</label>
            <textarea id="customerAddress" rows="3" required></textarea>
          </div>
          <div class="form-group">
            <label for="serviceTime">Preferred Service Time</label>
            <input type="datetime-local" id="serviceTime" required>
          </div>
          <div class="form-group">
            <label for="specialInstructions">Special Instructions</label>
            <textarea id="specialInstructions" rows="2"></textarea>
          </div>
          
          <div class="booking-summary">
            <h4>Service Summary</h4>
            <div id="bookingSummaryItems">
              ${renderBookingSummaryItems()}
            </div>
            <div class="booking-total">
              <span>Total:</span>
              <span id="bookingTotal">₹${bookingState.total.toFixed(2)}</span>
            </div>
          </div>
          
          <div class="coupon-section">
            <div class="form-group">
              <label for="bookingCouponCode">Coupon Code</label>
              <div class="coupon-input">
                <input type="text" id="bookingCouponCode" placeholder="Enter coupon code">
                <button type="button" class="apply-coupon" onclick="applyBookingCoupon()">Apply</button>
              </div>
              <div id="bookingCouponMessage" class="coupon-message"></div>
            </div>
          </div>
          
          <button type="submit" class="submit-booking">
            <i class="fas fa-paper-plane"></i> Confirm Booking
          </button>
        </form>
      </div>
    </div>
    
    <!-- Booking Confirmation Modal -->
    <div class="booking-confirmation-modal" id="bookingConfirmationModal">
      <div class="confirmation-content">
        <div class="confirmation-icon">
          <i class="fas fa-check-circle"></i>
        </div>
        <h3>Booking Confirmed!</h3>
        <p id="bookingConfirmationMessage">Your service booking has been confirmed successfully.</p>
        <div class="confirmation-actions">
          <button class="view-receipt" onclick="viewBookingReceipt()">
            <i class="fas fa-receipt"></i> View Details
          </button>
          <button class="close-confirmation" onclick="closeBookingConfirmationModal()">
            <i class="fas fa-times"></i> Close
          </button>
        </div>
      </div>
    </div>
    
    <!-- Toast Notification -->
    <div class="toast"></div>
  `;

  document.body.appendChild(providerPage);
  addProviderPageStyles();
  initializeTabs();
  initializeProviderCarousel();
  initializeBookingForm();
  initializeSearch();
  scrollToTop();
}

function renderProviderServices(services) {
  if (!services || services.length === 0) {
    return '<p class="no-services">No services available</p>';
  }

  return services.map(service => `
    <div class="service-item" data-name="${service.name}" data-price="${service.price.replace('₹','').split('-')[0]}">
      <div class="service-image">
        <img src="${service.image}" alt="${service.name}">
      </div>
      <div class="service-details">
        <h4>${service.name}</h4>
        <p class="service-description">${service.description}</p>
        <div class="service-pricing">
          <span class="service-price">${service.price}</span>
        </div>
        ${renderServiceVariants(service.variants)}
        <div class="service-actions">
          <button class="add-to-booking-cart" onclick="addToBookingCart(event, '${service.name}', ${service.price.replace('₹','').split('-')[0]}, '${service.image}')">
            <i class="fas fa-cart-plus"></i> Add to Cart
          </button>
          <button class="book-now" onclick="event.stopPropagation(); showBookingForm(event, '${service.name}', ${service.price.replace('₹','').split('-')[0]}, '${service.image}')">
            <i class="fas fa-calendar-check"></i> Book Now
          </button>
        </div>
      </div>
    </div>
  `).join('');
}

function renderServiceVariants(variants) {
  if (!variants || variants.length === 0) return '';
  
  return `
    <div class="variants-container">
      <select class="variant-select" onchange="updateServicePrice(this)">
        ${variants.map(variant => `
          <option value="${variant.price.replace('₹','')}" data-name="${variant.name}">
            ${variant.name} - ${variant.price}
          </option>
        `).join('')}
      </select>
    </div>
  `;
}

// Update the price update function to use the correct naming
function updateServicePrice(selectElement) {
  const price = selectElement.value;
  const itemElement = selectElement.closest('[data-price]');
  if (itemElement) {
    itemElement.dataset.price = price;
    const priceDisplay = itemElement.querySelector('.service-price');
    if (priceDisplay) {
      priceDisplay.textContent = `₹${price}`;
    }
  }
}

// Booking Cart Management
function renderBookingCartItems() {
  if (bookingState.items.length === 0) {
    return `
      <div class="empty-booking-cart">
        <i class="fas fa-shopping-cart"></i>
        <p>Your booking cart is empty</p>
        <button onclick="hideBookingCart()">Continue Browsing</button>
      </div>
    `;
  }

  return bookingState.items.map((item, index) => `
    <div class="booking-cart-item" data-index="${index}">
      <div class="booking-cart-item-image">
        <img src="${item.image}" alt="${item.name}">
      </div>
      <div class="booking-cart-item-details">
        <h4>${item.name}</h4>
        ${item.variant ? `<p class="variant">${item.variant}</p>` : ''}
        <div class="booking-cart-item-price">₹${item.price.toFixed(2)}</div>
        <div class="booking-cart-item-quantity">
          <button class="quantity-btn minus" onclick="updateBookingQuantity(${index}, -1)">−</button>
          <span class="quantity">${item.quantity}</span>
          <button class="quantity-btn plus" onclick="updateBookingQuantity(${index}, 1)">+</button>
        </div>
      </div>
      <button class="remove-item" onclick="removeFromBookingCart(${index})">
        <i class="fas fa-times"></i>
      </button>
    </div>
  `).join('');
}

function renderBookingSummaryItems() {
  return bookingState.items.map(item => `
    <div class="booking-item">
      <span>${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}</span>
      <span>₹${(item.price * item.quantity).toFixed(2)}</span>
    </div>
  `).join('');
}

function addToBookingCart(event, serviceName, basePrice, image) {
  event.stopPropagation();
  
  const itemElement = event.target.closest('[data-name]');
  const variantSelect = itemElement?.querySelector('.variant-select');
  
  let variantName = null;
  let price = parseFloat(basePrice);
  
  // Handle variants if they exist
  if (variantSelect) {
    const selectedOption = variantSelect.options[variantSelect.selectedIndex];
    variantName = selectedOption.dataset.name;
    price = parseFloat(selectedOption.value);
  }
  
  // Check if item already exists in cart
  const existingItemIndex = bookingState.items.findIndex(item => 
    item.name === serviceName && item.variant === variantName
  );
  
  if (existingItemIndex >= 0) {
    // Update quantity if item exists
    bookingState.items[existingItemIndex].quantity += 1;
  } else {
    // Add new item to cart
    bookingState.items.push({
      name: serviceName,
      variant: variantName,
      price: price,
      quantity: 1,
      image: image
    });
  }
  
  // Update booking total
  updateBookingTotal();
  
  // Update booking cart UI
  updateBookingCartUI();
  
  // Show success message
  showToast(`${serviceName} ${variantName ? `(${variantName})` : ''} added to booking cart`);
}

function removeFromBookingCart(index) {
  bookingState.items.splice(index, 1);
  updateBookingTotal();
  updateBookingCartUI();
  
  if (bookingState.items.length === 0) {
    hideBookingCart();
  }
}

function updateBookingQuantity(index, change) {
  const newQuantity = bookingState.items[index].quantity + change;
  
  if (newQuantity < 1) {
    removeFromBookingCart(index);
    return;
  }
  
  bookingState.items[index].quantity = newQuantity;
  updateBookingTotal();
  updateBookingCartUI();
}

function clearBookingCart() {
  bookingState.items = [];
  bookingState.total = 0;
  bookingState.discountedTotal = 0;
  bookingState.appliedCoupon = null;
  updateBookingCartUI();
  hideBookingCart();
}

function updateBookingTotal() {
  bookingState.total = bookingState.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
}

function updateBookingCartUI() {
  const bookingCartOverlay = document.getElementById('bookingCartOverlay');
  if (!bookingCartOverlay) return;
  
  // Update booking cart items list
  const bookingCartItemsContainer = document.getElementById('bookingCartItems');
  if (bookingCartItemsContainer) {
    bookingCartItemsContainer.innerHTML = renderBookingCartItems();
  }
  
  // Update booking cart total
  const bookingCartTotalElement = document.getElementById('bookingCartTotalAmount');
  if (bookingCartTotalElement) {
    bookingCartTotalElement.textContent = `₹${bookingState.total.toFixed(2)}`;
  }
  
  // Update booking cart count in header
  const bookingCartButton = document.querySelector('.cart-button');
  if (bookingCartButton) {
    const bookingCartCount = bookingCartButton.querySelector('.cart-count');
    
    if (bookingState.items.length > 0) {
      if (!bookingCartCount) {
        const countElement = document.createElement('span');
        countElement.className = 'cart-count';
        countElement.textContent = bookingState.items.length;
        bookingCartButton.appendChild(countElement);
      } else {
        bookingCartCount.textContent = bookingState.items.length;
      }
    } else if (bookingCartCount) {
      bookingCartCount.remove();
    }
  }
  
  // Update booking summary in booking form
  const bookingSummaryItems = document.getElementById('bookingSummaryItems');
  const bookingTotal = document.getElementById('bookingTotal');
  
  if (bookingSummaryItems && bookingTotal) {
    bookingSummaryItems.innerHTML = renderBookingSummaryItems();
    bookingTotal.textContent = `₹${bookingState.total.toFixed(2)}`;
  }
}

function initializeBookingForm() {
  const bookingForm = document.getElementById('bookingForm');
  if (!bookingForm) return;

  bookingForm.addEventListener('submit', function(e) {
    e.preventDefault();
    
    // Get form values
    const customerName = document.getElementById('customerName').value;
    const customerPhone = document.getElementById('customerPhone').value;
    const customerAddress = document.getElementById('customerAddress').value;
    const serviceTime = document.getElementById('serviceTime').value;
    const specialInstructions = document.getElementById('specialInstructions').value;
    
    // Format service time
    let formattedServiceTime = '';
    if (serviceTime) {
      const serviceDate = new Date(serviceTime);
      formattedServiceTime = serviceDate.toLocaleString('en-IN', {
        weekday: 'short',
        day: 'numeric',
        month: 'short',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit'
      });
    }
    
    // Determine which items to include in the booking
    const bookingItems = bookingState.items.length > 0 ? 
      bookingState.items.map(item => ({
        name: item.name,
        variant: item.variant,
        price: item.price,
        quantity: item.quantity,
        total: (item.price * item.quantity).toFixed(2)
      })) : 
      (bookingState.directBookingItems || []).map(item => ({
        name: item.name,
        variant: null,
        price: item.price,
        quantity: item.quantity,
        total: (item.price * item.quantity).toFixed(2)
      }));
    
    // Use discounted total if coupon was applied, otherwise use regular total
    const bookingTotal = bookingState.appliedCoupon ? 
      bookingState.discountedTotal : 
      bookingItems.reduce((sum, item) => sum + parseFloat(item.total), 0);
    
    // Create booking details message
    let bookingDetails = `*NEW SERVICE BOOKING*\n\n`;
    bookingDetails += `*Service Provider:* ${bookingState.provider.details.name}\n`;
    bookingDetails += `*Booking Time:* ${new Date().toLocaleString('en-IN')}\n\n`;
    
    bookingDetails += `*Customer Details:*\n`;
    bookingDetails += `👤 Name: ${customerName}\n`;
    bookingDetails += `📞 Phone: ${customerPhone}\n`;
    bookingDetails += `🏠 Address: ${customerAddress}\n`;
    if (formattedServiceTime) {
      bookingDetails += `⏰ Service Time: ${formattedServiceTime}\n`;
    }
    if (specialInstructions) {
      bookingDetails += `📝 Special Instructions: ${specialInstructions}\n`;
    }
    bookingDetails += `\n`;
    
    bookingDetails += `*Services Booked:*\n`;
    bookingDetails += bookingItems.map(item => 
      `- ${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}: ₹${item.total}`
    ).join('\n');
    
    // Add coupon information if applied
    if (bookingState.appliedCoupon) {
      bookingDetails += `\n\n*Discount Applied:*\n`;
      bookingDetails += `Coupon Code: ${bookingState.appliedCoupon.code}\n`;
      bookingDetails += `Discount: ${(bookingState.appliedCoupon.discount * 100)}%\n`;
      bookingDetails += `Discount Amount: ₹${bookingState.appliedCoupon.discountAmount.toFixed(2)}\n`;
    }
    
    bookingDetails += `\n\n*Subtotal:* ₹${bookingItems.reduce((sum, item) => sum + parseFloat(item.total), 0).toFixed(2)}`;
    
    if (bookingState.appliedCoupon) {
      bookingDetails += `\n*Discount:* -₹${bookingState.appliedCoupon.discountAmount.toFixed(2)}`;
    }
    
    bookingDetails += `\n*Booking Total:* ₹${bookingTotal.toFixed(2)}`;
    
    // Send booking based on provider's preferred contact method
    if (bookingState.provider.details.support.primaryContact === "whatsapp" && bookingState.provider.details.support.whatsapp) {
      const whatsappUrl = `https://wa.me/${bookingState.provider.details.support.whatsapp}?text=${encodeURIComponent(bookingDetails)}`;
      window.open(whatsappUrl, '_blank');
    } else if (bookingState.provider.details.support.primaryContact === "email" && bookingState.provider.details.support.email) {
      const subject = `New Booking from ${customerName} - ${bookingState.provider.details.name}`;
      const body = bookingDetails.replace(/\*/g, '');
      const mailtoUrl = `mailto:${bookingState.provider.details.support.email}?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
      window.location.href = mailtoUrl;
    }
    
    // Show confirmation
    showBookingConfirmation(customerName, bookingTotal.toFixed(2));
    
    // Clear booking cart/direct booking and close form
    clearBookingCart();
    delete bookingState.directBookingItems;
    closeBookingForm();
  });
}

function applyBookingCoupon() {
  const couponCode = document.getElementById('bookingCouponCode').value;
  const couponMessage = document.getElementById('bookingCouponMessage');
  
  if (!couponCode) {
    couponMessage.textContent = 'Please enter a coupon code';
    couponMessage.className = 'coupon-message error';
    return;
  }
  
  // Simulate coupon validation
  const validCoupons = {
    'SAVE10': { discount: 0.1, message: '10% discount applied!' },
    'FREESERVICE': { discount: 0, message: 'Free service call applied!' },
    'WELCOME20': { discount: 0.2, message: '20% discount applied!' }
  };
  
  if (validCoupons[couponCode]) {
    const coupon = validCoupons[couponCode];
    const discountAmount = bookingState.total * coupon.discount;
    const newTotal = bookingState.total - discountAmount;
    
    // Update booking state
    bookingState.discountedTotal = newTotal;
    bookingState.appliedCoupon = {
      code: couponCode,
      discount: coupon.discount,
      discountAmount: discountAmount
    };
    
    // Update UI
    couponMessage.textContent = coupon.message;
    couponMessage.className = 'coupon-message success';
    
    document.getElementById('bookingTotal').textContent = `₹${newTotal.toFixed(2)}`;
  } else {
    couponMessage.textContent = 'Invalid coupon code';
    couponMessage.className = 'coupon-message error';
    bookingState.discountedTotal = bookingState.total;
    bookingState.appliedCoupon = null;
  }
}

// Booking Confirmation Functions
function showBookingConfirmation(customerName, bookingTotal) {
  const confirmationModal = document.getElementById('bookingConfirmationModal');
  const confirmationMessage = document.getElementById('bookingConfirmationMessage');
  
  if (confirmationModal && confirmationMessage) {
    confirmationMessage.textContent = `Thank you, ${customerName}! Your booking of ₹${bookingTotal} has been confirmed.`;
    confirmationModal.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function closeBookingConfirmationModal() {
  const confirmationModal = document.getElementById('bookingConfirmationModal');
  if (confirmationModal) {
    confirmationModal.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function viewBookingReceipt() {
  // In a real app, this would generate or show a detailed receipt
  alert('Booking details would be displayed here in a real implementation.');
  closeBookingConfirmationModal();
}

// Booking Visibility Functions
function showBookingCart() {
  const bookingCartOverlay = document.getElementById('bookingCartOverlay');
  if (bookingCartOverlay) {
    bookingCartOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function hideBookingCart() {
  const bookingCartOverlay = document.getElementById('bookingCartOverlay');
  if (bookingCartOverlay) {
    bookingCartOverlay.classList.remove('active');
    document.body.style.overflow = '';
  }
}

function showBookingForm() {
  if (bookingState.items.length === 0 && !bookingState.directBookingItems) {
    showToast('Please add services to book');
    return;
  }
  
  hideBookingCart();
  
  const bookingFormOverlay = document.getElementById('bookingFormOverlay');
  if (bookingFormOverlay) {
    bookingFormOverlay.classList.add('active');
    document.body.style.overflow = 'hidden';
  }
}

function showBookingForm(event, serviceName, servicePrice, serviceImage) {
  if (event) event.stopPropagation();
  
  let items = [];
  
  if (serviceName && servicePrice) {
    // Handle direct booking from "Book Now" button
    const itemElement = event?.target.closest('[data-name]');
    const variantSelect = itemElement?.querySelector('.variant-select');
    
    if (variantSelect) {
      const selectedOption = variantSelect.options[variantSelect.selectedIndex];
      items.push({
        name: `${serviceName} (${selectedOption.dataset.name})`,
        price: parseFloat(selectedOption.value),
        quantity: 1,
        image: serviceImage
      });
    } else {
      items.push({
        name: serviceName,
        price: servicePrice,
        quantity: 1,
        image: serviceImage
      });
    }
    
    // Store the direct booking items temporarily
    bookingState.directBookingItems = items;
    // Update the total for direct booking
    bookingState.total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  } else if (bookingState.items.length === 0) {
    showToast('Please add services to book');
    return;
  }
  
  // Create or show the booking form overlay
  let bookingFormOverlay = document.getElementById('bookingFormOverlay');
  
  if (!bookingFormOverlay) {
    // If overlay doesn't exist (shouldn't happen in normal flow), create it
    bookingFormOverlay = document.createElement('div');
    bookingFormOverlay.id = 'bookingFormOverlay';
    bookingFormOverlay.className = 'booking-form-overlay';
    bookingFormOverlay.innerHTML = `
      <div class="booking-form-container">
        <button class="close-booking-form" onclick="closeBookingForm()">
          <i class="fas fa-times"></i>
        </button>
        <h3>Book Services</h3>
        <form id="bookingForm">
          <!-- Form fields here -->
        </form>
      </div>
    `;
    document.querySelector('.provider-page').appendChild(bookingFormOverlay);
    initializeBookingForm();
  }
  
  // Update the booking summary in the form
  updateBookingSummary();
  
  bookingFormOverlay.classList.add('active');
  document.body.style.overflow = 'hidden';
}

function updateBookingSummary() {
  const bookingSummaryItems = document.getElementById('bookingSummaryItems');
  const bookingTotal = document.getElementById('bookingTotal');
  
  if (!bookingSummaryItems || !bookingTotal) return;
  
  // Determine which items to show (direct booking or cart items)
  const items = bookingState.items.length > 0 ? 
    bookingState.items : 
    (bookingState.directBookingItems || []);
  
  bookingSummaryItems.innerHTML = items.map(item => `
    <div class="booking-item">
      <span>${item.name} ${item.variant ? `(${item.variant})` : ''} × ${item.quantity}</span>
      <span>₹${(item.price * item.quantity).toFixed(2)}</span>
    </div>
  `).join('');
  
  const total = items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  bookingTotal.textContent = `₹${total.toFixed(2)}`;
}

function closeBookingForm() {
  const bookingFormOverlay = document.getElementById('bookingFormOverlay');
  if (bookingFormOverlay) {
    bookingFormOverlay.classList.remove('active');
    document.body.style.overflow = '';
    
    // Reset coupon-related fields and state
    const couponCodeInput = document.getElementById('bookingCouponCode');
    const couponMessage = document.getElementById('bookingCouponMessage');
    
    if (couponCodeInput) couponCodeInput.value = '';
    if (couponMessage) {
      couponMessage.textContent = '';
      couponMessage.className = 'coupon-message';
    }
    
    // Reset the discounted total (but keep the regular total)
    bookingState.discountedTotal = bookingState.total;
    bookingState.appliedCoupon = null;
    
    // Update the displayed total to remove any discount
    const bookingTotalElement = document.getElementById('bookingTotal');
    if (bookingTotalElement) {
      bookingTotalElement.textContent = `₹${bookingState.total.toFixed(2)}`;
    }
  }
}


function closeProviderPage() {
  const providerPage = document.querySelector('.provider-page');
  if (providerPage) {
    providerPage.remove();
  }
}

function initializeProviderCarousel() {
  const carousel = document.querySelector('.provider-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.provider-carousel-item');
  const dots = carousel.querySelectorAll('.provider-pagination-dot');
  let currentIndex = 0;

  function showSlide(index) {
    items.forEach((item, i) => {
      item.classList.remove('active');
      dots[i].classList.remove('active');
      if (i === index) {
        item.classList.add('active');
        dots[i].classList.add('active');
      }
    });
    currentIndex = index;
  }

  function nextSlide() {
    const newIndex = (currentIndex + 1) % items.length;
    showSlide(newIndex);
  }

  function prevSlide() {
    const newIndex = (currentIndex - 1 + items.length) % items.length;
    showSlide(newIndex);
  }

  // Auto-rotate every 5 seconds
  let interval = setInterval(nextSlide, 5000);

  // Pause on hover
  carousel.addEventListener('mouseenter', () => {
    clearInterval(interval);
  });

  carousel.addEventListener('mouseleave', () => {
    interval = setInterval(nextSlide, 5000);
  });
}

function changeProviderCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.provider-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.provider-carousel-item');
  const dots = carousel.querySelectorAll('.provider-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentProviderSlide(index) {
  const carousel = event.target.closest('.provider-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.provider-carousel-item');
  const dots = carousel.querySelectorAll('.provider-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

// Add styles for the provider page
function addProviderPageStyles() {
  const style = document.createElement('style');
  style.textContent = `
.provider-page {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: #1a1a1f;
  z-index: 2000;
  overflow-y: auto;
  color: #fff;
  font-family: 'Poppins', sans-serif;
}

.provider-header {
  position: sticky;
  top: 0;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
  z-index: 10;
  backdrop-filter: blur(12px);
}

.back-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.5em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.back-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.provider-header h2 {
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  text-align: center;
  flex: 1;
  padding: 0 16px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.header-actions {
  display: flex;
  gap: 8px;
}

.cart-button {
  position: relative;
  background: none;
  border: none;
  color: #ffd700;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.cart-button:hover {
  background: rgba(255, 215, 0, 0.1);
}

.cart-count {
  position: absolute;
  top: -5px;
  right: -5px;
  background: #ff0000;
  color: white;
  border-radius: 50%;
  width: 20px;
  height: 20px;
  font-size: 0.7em;
  display: flex;
  align-items: center;
  justify-content: center;
}

.share-button {
  background: none;
  border: none;
  color: #fff;
  font-size: 1.2em;
  cursor: pointer;
  padding: 8px;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease;
}

.share-button:hover {
  background: rgba(255, 255, 255, 0.1);
}

.provider-content {
  padding: 0 0 100px;
  width: 100%;
  margin: 0 auto;
}

.provider-hero {
  position: relative;
  padding: 20px;
}

.provider-image-carousel {
  position: relative;
  width: 100%;
  height: 280px;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  margin-bottom: 16px;
}

.provider-carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease;
}

.provider-carousel-item.active {
  opacity: 1;
}

.provider-carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.provider-carousel-item.active img:hover {
  transform: scale(1.02);
}

.provider-carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 18px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0.7;
  transition: all 0.3s ease;
  z-index: 5;
}

.provider-carousel-control:hover {
  background: rgba(0, 0, 0, 0.9);
  opacity: 1;
  transform: translateY(-50%) scale(1.1);
}

.provider-carousel-control.prev {
  left: 16px;
}

.provider-carousel-control.next {
  right: 16px;
}

.provider-carousel-pagination {
  position: absolute;
  bottom: 16px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.provider-pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
}

.provider-pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
}

.provider-basic-info {
  display: flex;
  gap: 12px;
  margin-top: 16px;
  flex-wrap: wrap;
}

.provider-star,
.provider-type,
.visiting-fees,
.provider-timings {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 8px 12px;
  border-radius: 20px;
  font-size: 0.9em;
  font-weight: 500;
}

.provider-star {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
}

.provider-type {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.visiting-fees {
  background: rgba(0, 191, 255, 0.1);
  color: #00bfff;
}

.provider-timings {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
}

.established-since {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-top: 12px;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.provider-description-section {
  padding: 0 20px 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-description-section h3 {
  margin: 0 0 12px;
  font-size: 1.2em;
  font-weight: 600;
}

.provider-description-section p {
  margin: 0;
  line-height: 1.6;
  color: rgba(255, 255, 255, 0.8);
}

.provider-services-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-services-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.services-container {
  display: grid;
  gap: 20px;
}

.service-item {
  display: flex;
  background: #fff;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
  transition: transform 0.3s ease;
}

.service-item:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 16px rgba(0,0,0,0.15);
}

.service-image {
  width: 150px;
  height: 150px;
  flex-shrink: 0;
}

.service-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.service-details {
  flex: 1;
  padding: 16px;
}

.service-details h4 {
  margin: 0 0 8px;
  color: #000;
  font-size: 1.1em;
}

.service-description {
  color: #000;
  font-size: 0.9em;
  margin: 0 0 12px;
}

.service-pricing {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 8px;
}

.service-price {
  font-weight: bold;
  color: #2e7d32;
  font-size: 1.1em;
}

.variants-container {
  margin-bottom: 12px;
}

.variant-select {
  width: 100%;
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #ddd;
  background: #f9f9f9;
}

.service-actions {
  display: flex;
  gap: 10px;
}

.add-to-booking-cart,
.book-now {
  flex: 1;
  padding: 8px 12px;
  border: none;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.add-to-booking-cart {
  background: #f5f5f5;
  color: #333;
}

.add-to-booking-cart:hover {
  background: #e0e0e0;
}

.book-now {
  background: #ffd700;
  color: #000;
}

.book-now:hover {
  background: #ffc400;
}

.provider-facilities-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-facilities-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.facilities-grid {
  display: grid;
  gap: 16px;
}

.facility-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.facility-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.facility-icon {
  width: 48px;
  height: 48px;
  border-radius: 12px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: #ffd700;
  font-size: 1.2em;
}

.facility-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.facility-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.provider-promos-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-promos-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.promos-grid {
  display: grid;
  gap: 16px;
}

.promo-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  padding: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.promo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.promo-info h4 {
  margin: 0 0 4px;
  font-size: 1em;
  font-weight: 600;
}

.promo-info p {
  margin: 0;
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.9em;
}

.promo-code {
  display: flex;
  align-items: center;
  gap: 8px;
}

.promo-code span {
  background: rgba(255, 215, 0, 0.1);
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  font-weight: 600;
}

.promo-code button {
  background: rgba(255, 215, 0, 0.1);
  border: none;
  color: #ffd700;
  padding: 6px 12px;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.3s ease;
}

.promo-code button:hover {
  background: rgba(255, 215, 0, 0.2);
}

.provider-contact-section {
  padding: 20px;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.provider-contact-section h3 {
  margin: 0 0 16px;
  font-size: 1.2em;
  font-weight: 600;
}

.contact-methods {
  display: grid;
  gap: 12px;
  margin-bottom: 20px;
}

.contact-item {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 8px;
  color: #fff;
  text-decoration: none;
  transition: background 0.3s ease;
}

.contact-item:hover {
  background: rgba(255, 255, 255, 0.1);
}

.social-links {
  display: flex;
  gap: 16px;
  margin-top: 20px;
}

.social-link {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.05);
  color: #fff;
  font-size: 1.2em;
  transition: transform 0.3s ease, background 0.3s ease;
}

.social-link:hover {
  transform: translateY(-2px);
  background: rgba(255, 255, 255, 0.1);
}

.provider-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

/* Booking Cart Overlay Styles */
.booking-cart-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.booking-cart-overlay.active {
  opacity: 1;
  visibility: visible;
}

.booking-cart-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 80vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
}

.booking-cart-header {
  padding: 20px;
  border-bottom: 1px solid #eee;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.booking-cart-header h3 {
  margin: 0;
  font-size: 1.3em;
  color: #333;
}

.close-booking-cart {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.booking-cart-items {
  flex: 1;
  overflow-y: auto;
  padding: 20px;
}

.empty-booking-cart {
  text-align: center;
  padding: 30px 0;
}

.empty-booking-cart i {
  font-size: 3em;
  color: #ccc;
  margin-bottom: 15px;
}

.empty-booking-cart p {
  margin: 10px 0 20px;
  color: #666;
}

.empty-booking-cart button {
  background: var(--primary-color);
  color: #000;
  border: none;
  padding: 10px 20px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
}

.booking-cart-item {
  display: flex;
  padding: 15px 0;
  border-bottom: 1px solid #f5f5f5;
  position: relative;
}

.booking-cart-item-image {
  width: 80px;
  height: 80px;
  flex-shrink: 0;
  margin-right: 15px;
}

.booking-cart-item-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  border-radius: 8px;
}

.booking-cart-item-details {
  flex: 1;
}

.booking-cart-item-details h4 {
  margin: 0 0 5px;
  font-size: 1em;
  color: #333;
}

.booking-cart-item-details .variant {
  margin: 0 0 8px;
  font-size: 0.85em;
  color: #666;
}

.booking-cart-item-price {
  font-weight: bold;
  color: #333;
  margin-bottom: 10px;
}

.booking-cart-item-quantity {
  color: #000;
  display: flex;
  align-items: center;
  gap: 10px;
}

.quantity-btn {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  border: 1px solid #ddd;
  background: #fff;
  font-size: 1em;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.quantity-btn:hover {
  background: #f5f5f5;
}

.quantity {
  min-width: 20px;
  text-align: center;
}

.remove-item {
  position: absolute;
  top: 15px;
  right: 0;
  background: none;
  border: none;
  color: #999;
  cursor: pointer;
  font-size: 1em;
}

.remove-item:hover {
  color: #ff5252;
}

.booking-cart-summary {
  padding: 20px;
  border-top: 1px solid #eee;
}

.booking-cart-total {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
  font-size: 1.1em;
  font-weight: bold;
}

.booking-cart-actions {
  display: flex;
  gap: 10px;
}

.clear-booking-cart, .checkout-booking-btn {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.clear-booking-cart {
  background: #f5f5f5;
  color: #333;
  border: none;
}

.clear-booking-cart:hover {
  background: #e0e0e0;
}

.checkout-booking-btn {
  background: var(--primary-color);
  color: #000;
  border: none;
}

.checkout-booking-btn:hover {
  background: #ffc400;
}

/* Booking Form Styles */
.booking-form-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 3000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.booking-form-overlay.active {
  opacity: 1;
  visibility: visible;
}

.booking-form-container {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 500px;
  max-height: 90vh;
  overflow-y: auto;
  padding: 24px;
  position: relative;
}

.booking-form-container h3 {
  margin: 0 0 20px;
  text-align: center;
  color: #333;
}

.close-booking-form {
  background: none;
  border: none;
  font-size: 1.2em;
  color: #666;
  cursor: pointer;
  padding: 5px;
}

.form-group {
  margin-bottom: 16px;
}

.form-group label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #333;
}

.form-group input,
.form-group select,
.form-group textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 1em;
}

.form-group textarea {
  resize: vertical;
  min-height: 80px;
}

.coupon-input {
  display: flex;
  gap: 10px;
}

.coupon-input input {
  flex: 1;
}

.apply-coupon {
  background: #f5f5f5;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 0 15px;
  cursor: pointer;
}

.apply-coupon:hover {
  background: #e0e0e0;
}

.coupon-message {
  margin-top: 8px;
  font-size: 0.9em;
}

.coupon-message.success {
  color: #2e7d32;
}

.coupon-message.error {
  color: #c62828;
}

.booking-summary {
  margin: 20px 0;
  padding: 16px;
  background: #f9f9f9;
  border-radius: 8px;
}

.booking-summary h4 {
  margin: 0 0 12px;
  color: #333;
}

.booking-item {
  display: flex;
  justify-content: space-between;
  margin-bottom: 8px;
  padding-bottom: 8px;
  border-bottom: 1px solid #eee;
  color: #333;
}

.booking-total {
  display: flex;
  justify-content: space-between;
  margin-top: 12px;
  font-weight: bold;
  font-size: 1.1em;
  color: #333;
}

.submit-booking {
  width: 100%;
  padding: 14px;
  background: var(--primary-color);
  color: #000;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  font-size: 1em;
  cursor: pointer;
  transition: background 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.submit-booking:hover {
  background: #ffc400;
}

/* Booking Confirmation Modal Styles */
.booking-confirmation-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 4000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.booking-confirmation-modal.active {
  opacity: 1;
  visibility: visible;
}

.confirmation-content {
  background: #fff;
  border-radius: 16px;
  width: 90%;
  max-width: 400px;
  padding: 30px;
  text-align: center;
}

.confirmation-icon {
  font-size: 4em;
  color: #4caf50;
  margin-bottom: 20px;
}

.confirmation-content h3 {
  margin: 0 0 15px;
  color: #333;
}

.confirmation-content p {
  margin: 0 0 25px;
  color: #666;
  line-height: 1.5;
}

.confirmation-actions {
  display: flex;
  gap: 10px;
}

.view-receipt, .close-confirmation {
  flex: 1;
  padding: 12px;
  border-radius: 8px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
}

.view-receipt {
  background: #4caf50;
  color: #fff;
  border: none;
}

.view-receipt:hover {
  background: #3d8b40;
}

.close-confirmation {
  background: #f5f5f5;
  color: #fff;
  border: none;
}

.close-confirmation:hover {
  background: #fff;
}

/* Toast notification */
.toast {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: #333;
  color: #fff;
  padding: 12px 24px;
  border-radius: 8px;
  z-index: 4000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.toast.show {
  opacity: 1;
  visibility: visible;
}

/* Responsive Styles */
@media (max-width: 768px) {
  .service-item {
    flex-direction: column;
  }
  
  .service-image {
    width: 100%;
    height: 180px;
  }
  
  .service-actions {
    flex-direction: column;
  }
  
  .add-to-booking-cart,
  .book-now {
    width: 100%;
  }
}

@media (max-width: 480px) {
  .booking-cart-container,
  .booking-form-container {
    width: 95%;
  }
  
  .booking-cart-item {
    flex-direction: column;
  }
  
  .booking-cart-item-image {
    width: 100%;
    height: 150px;
    margin-right: 0;
    margin-bottom: 10px;
  }
  
  .booking-cart-actions,
  .confirmation-actions {
    flex-direction: column;
  }
  
  .booking-form-container {
    padding: 16px;
  }
}
  `;
  document.head.appendChild(style);
}

// Helper functions for provider page
function getFacilityIcon(facilityName) {
  const icons = {
    'Emergency Service': 'ambulance',
    'Guarantee': 'shield-alt',
    'Equipment': 'tools',
    'Certified Technicians': 'certificate',
    'Safety First': 'shield-alt',
    'Warranty': 'file-contract',
    'Custom Work': 'ruler-combined',
    'Quality Materials': 'gem',
    'On-site Service': 'home',
    'Rapid Response': 'bolt',
    'Waterproofing': 'umbrella',
    'Free Estimates': 'file-invoice-dollar',
    'Smart Home': 'home',
    'Energy Audit': 'lightbulb',
    'Safety Checks': 'clipboard-check'
  };
  
  return icons[facilityName] || 'check-circle';
}

// Function to filter providers based on category
function filterProviders(category) {
  const providersGrid = document.getElementById('providersGrid');
  let filteredData = serviceProvidersData;

  if (category !== 'All') {
    filteredData = filteredData.filter(provider => provider.category === category);
  }

  providersGrid.innerHTML = renderProviderCards(filteredData);

  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function sortProviders(criteria) {
  const providersGrid = document.getElementById('providersGrid');
  let sortedData = [...serviceProvidersData];

  switch(criteria) {
    case 'rating':
      sortedData.sort((a, b) => parseFloat(b.details.rating) - parseFloat(a.details.rating));
      break;
    case 'name':
      sortedData.sort((a, b) => a.details.name.localeCompare(b.details.name));
      break;
    case 'distance':
      // This would require location data to be implemented
      break;
  }

  providersGrid.innerHTML = renderProviderCards(sortedData);
  toggleSortOptions();
}

function toggleSortOptions() {
  const sortOptions = document.getElementById('sortOptionsContainer');
  sortOptions.classList.toggle('active');
}

function hideProvidersOverlay() {
  const overlay = document.getElementById('providersOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
    }, 300);
  }
}

// Shared helper functions
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

function performSearch(data, query, gridElement, filterButtons, renderFunction) {
  let filteredData = data;
  let activeCategory = 'All'; // Default to 'All'
  
  if (query) {
    const searchTerm = query.toLowerCase();
    
    // First try to match exact category names
    const categoryMatch = Array.from(filterButtons).find(button => {
      const category = button.getAttribute('data-category').toLowerCase();
      return category === searchTerm;
    });
    
    if (categoryMatch) {
      activeCategory = categoryMatch.getAttribute('data-category');
    } else {
      // If no exact category match, filter the data
      filteredData = filteredData.filter(item => 
        item.details.name.toLowerCase().includes(searchTerm) ||
        item.category.toLowerCase().includes(searchTerm) ||
        item.metropolis.toLowerCase().includes(searchTerm) ||
        (item.details.description && item.details.description.toLowerCase().includes(searchTerm)) ||
        (item.details.tags && item.details.tags.some(tag => tag.toLowerCase().includes(searchTerm)))
      );
      
      // Find if any provider's name matches exactly (for more precise filtering)
      const exactNameMatch = data.find(item => 
        item.details.name.toLowerCase() === searchTerm
      );
      
      if (exactNameMatch) {
        activeCategory = exactNameMatch.category;
      } else if (filteredData.length > 0) {
        // If we have filtered results, use the first result's category
        activeCategory = filteredData[0].category;
      }
    }
  }
  
  // Apply the category filter if not 'All'
  if (activeCategory !== 'All') {
    filteredData = filteredData.filter(item => item.category === activeCategory);
  }
  
  // Update the UI
  gridElement.innerHTML = renderFunction(filteredData);
  
  // Update active filter button
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === activeCategory) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function initializeOverlayVoiceSearch(inputElement, callback) {
  const voiceSearchButton = document.getElementById('voiceSearchButton');
  if (!voiceSearchButton) return;

  voiceSearchButton.addEventListener('click', () => {
    if (!('webkitSpeechRecognition' in window)) {
      alert('Voice search is not supported in your browser');
      return;
    }

    const recognition = new webkitSpeechRecognition();
    recognition.continuous = false;
    recognition.interimResults = false;

    voiceSearchButton.classList.add('active');
    
    recognition.onstart = () => {
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone-slash"></i>';
    };

    recognition.onresult = (event) => {
      const transcript = event.results[0][0].transcript;
      inputElement.value = transcript;
      callback(transcript);
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone"></i>';
      voiceSearchButton.classList.remove('active');
    };

    recognition.onerror = (event) => {
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone"></i>';
      voiceSearchButton.classList.remove('active');
      console.error('Voice recognition error', event.error);
    };

    recognition.onend = () => {
      voiceSearchButton.innerHTML = '<i class="fas fa-microphone"></i>';
      voiceSearchButton.classList.remove('active');
    };

    recognition.start();
  });
}

function openFullScreen(imageUrl) {
  event.stopPropagation();
  const fullScreenDiv = document.createElement('div');
  fullScreenDiv.className = 'full-screen-image';
  fullScreenDiv.innerHTML = `
    <img src="${imageUrl}" alt="Full screen">
    <button class="close-full-screen" onclick="document.querySelector('.full-screen-image').remove()">
      <i class="fas fa-times"></i>
    </button>
  `;
  document.body.appendChild(fullScreenDiv);
  
  setTimeout(() => {
    fullScreenDiv.classList.add('active');
    const img = fullScreenDiv.querySelector('img');
    if (img) {
      img.style.opacity = '1';
      img.style.transform = 'scale(1)';
    }
  }, 10);
}

function changeCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentSlide(index) {
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function copyToClipboard(text) {
  event.stopPropagation();
  navigator.clipboard.writeText(text).then(() => {
    const button = event.target.closest('button');
    if (button) {
      const originalText = button.textContent;
      button.textContent = 'Copied!';
      setTimeout(() => {
        button.textContent = originalText;
      }, 2000);
    }
  }).catch(err => {
    console.error('Failed to copy text: ', err);
  });
}

function shareProvider(providerName) {
  event.stopPropagation();
  const provider = serviceProvidersData.find(prov => prov.details.name === providerName);
  if (!provider) return;

  if (navigator.share) {
    navigator.share({
      title: provider.details.name,
      text: `Check out ${provider.details.name} - ${provider.details.description}`,
      url: provider.details.website || window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    // Fallback for browsers that don't support Web Share API
    const shareUrl = provider.details.website || window.location.href;
    const shareText = `Check out ${provider.details.name}: ${shareUrl}`;
    prompt('Copy this link to share:', shareText);
  }
}

function showChatSupport(providerName) {
  event.stopPropagation();
  alert(`Chat support for ${providerName} would open here in a real implementation.`);
}

const realEstateData = [
  // Offline Properties (Residential)
  {
    type: "Property",
    category: "Residential",
    subcategory: "Apartment",
    metropolis: "Mumbai",
    details: {
      name: "Luxury Bayview Apartments",
      cardImages: [
        "https://i.pinimg.com/originals/23/a3/f6/23a3f6d6d618427e899a29307026a3cf.jpg",
        "https://example.com/apartment-card2.jpg"
      ],
      storeImages: [
        "https://example.com/apartment1.jpg",
        "https://example.com/apartment2.jpg",
        "https://example.com/apartment3.jpg"
      ],
      description: "Premium sea-facing apartments with world-class amenities in the heart of Mumbai. 2-4 BHK configurations available.",
      address: "Marine Drive, Mumbai - 400020",
      price: "₹4.2 Cr - ₹8.5 Cr",
      area: "850 sq.ft - 1800 sq.ft",
      possession: "Ready to Move",
      mapLink: "https://maps.example.com/bayview",
      developer: "Omkar Realtors",
      amenities: [
        { name: "Swimming Pool", icon: "swimming-pool" },
        { name: "Gymnasium", icon: "dumbbell" },
        { name: "24/7 Security", icon: "shield-alt" }
      ],
      specifications: [
        { name: "Bedrooms", value: "2-4 BHK" },
        { name: "Bathrooms", value: "2-4" },
        { name: "Parking", value: "2 Covered" }
      ],
      contact: {
        email: "sales@bayviewmumbai.com",
        phone: "+91-9876543210",
        agent: "Rahul Sharma"
      },
      rating: "4.8",
      tags: ["Sea View", "Premium", "Ready Possession"]
    }
  },
  // Other properties follow the same pattern...
];

// Function to show the real estate overlay
function showRealEstateOverlay() {
  let overlay = document.getElementById('realEstateOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'realEstateOverlay';
    overlay.className = 'real-estate-overlay';
    document.body.appendChild(overlay);
  }

  const categories = ['All', ...new Set(realEstateData.map(item => item.category))];

  overlay.innerHTML = `
    <div class="real-estate-content">
      <div class="real-estate-header">
        <div class="header-title-container">
          <h2>Real Estate</h2>
          <div class="header-subtitle">Find properties across categories</div>
        </div>
        <div class="header-buttons-container">
          <button class="sort-button" onclick="toggleSortOptions()">
            <i class="fas fa-sliders-h"></i>
          </button>
          <button class="close-real-estate" onclick="hideRealEstateOverlay()">
            <i class="fas fa-times"></i>
          </button>
        </div>
      </div>
      
      <div class="sort-options-container" id="sortOptionsContainer">
        <div class="sort-option" onclick="sortRealEstate('rating')">
          <i class="fas fa-star"></i> Highest Rated
        </div>
        <div class="sort-option" onclick="sortRealEstate('price')">
          <i class="fas fa-rupee-sign"></i> Price
        </div>
        <div class="sort-option" onclick="sortRealEstate('name')">
          <i class="fas fa-sort-alpha-down"></i> Alphabetical
        </div>
        <div class="sort-option" onclick="sortRealEstate('distance')">
          <i class="fas fa-map-marker-alt"></i> Nearest
        </div>
      </div>

      <div class="real-estate-filter-container">
        <div class="category-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterRealEstate('${category}')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="real-estate-grid" id="realEstateGrid">
        ${renderRealEstateCards(realEstateData)}
      </div>
    </div>

    <div class="chat-container">
      <div class="input-container">
        <input type="text" id="realEstateExploreInput" placeholder="Search for properties..." />
        <button class="voice-search" id="voiceSearchButton">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  // Initialize functionality
  const exploreInput = document.getElementById('realEstateExploreInput');
  const realEstateGrid = document.getElementById('realEstateGrid');
  const filterButtons = document.querySelectorAll('.filter-button');

  const debouncedSearch = debounce((query) => {
    performSearch(realEstateData, query, realEstateGrid, filterButtons, renderRealEstateCards);
  }, 300);

  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      performSearch(realEstateData, exploreInput.value, realEstateGrid, filterButtons, renderRealEstateCards);
    }
  });

  initializeOverlayVoiceSearch(exploreInput, (query) => {
    performSearch(realEstateData, query, realEstateGrid, filterButtons, renderRealEstateCards);
  });

  setTimeout(() => overlay.classList.add('active'), 10);

  // Add styles
  const style = document.createElement('style');
  style.textContent = `
.real-estate-overlay {
  font-family: 'Poppins', sans-serif;
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.real-estate-overlay::-webkit-scrollbar {
  display: none;
}

.real-estate-overlay.active {
  opacity: 1;
  visibility: visible;
}

.real-estate-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.real-estate-header {
  padding: 12px 20px;
  background: rgba(26, 26, 31, 0.98);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 1px solid rgba(255, 215, 0, 0.15);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  height: 72px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

.header-title-container {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
}

.real-estate-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  font-weight: 600;
  letter-spacing: -0.02em;
  line-height: 1.2;
}

.header-subtitle {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.8em;
  margin-top: 2px;
  font-weight: 400;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.close-real-estate {
  background: rgba(255, 255, 255, 0.1);
  border: none;
  color: white;
  font-size: 1em;
  cursor: pointer;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
}

.close-real-estate:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.05);
}

.close-real-estate:active {
  transform: scale(0.95);
}

.sort-button {
  background: rgba(255, 215, 0, 0.1);
  border: 1px solid rgba(255, 215, 0, 0.3);
  color: var(--primary-color);
  font-size: 0.9em;
  cursor: pointer;
  padding: 8px 12px;
  border-radius: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  transition: all 0.2s ease;
}

.sort-button:hover {
  background: rgba(255, 215, 0, 0.15);
  transform: translateY(-1px);
}

.sort-button:active {
  transform: translateY(0);
}

.sort-options-container {
  position: fixed;
  top: 72px;
  right: 20px;
  background: #2a2a35;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
  z-index: 40;
  padding: 8px 0;
  opacity: 0;
  visibility: hidden;
  transform: translateY(-10px);
  transition: all 0.2s ease;
  border: 1px solid rgba(255, 255, 255, 0.1);
}

.sort-options-container.active {
  opacity: 1;
  visibility: visible;
  transform: translateY(0);
}

.sort-option {
  padding: 12px 20px;
  color: white;
  font-size: 0.9em;
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
  white-space: nowrap;
}

.sort-option:hover {
  background: rgba(255, 215, 0, 0.1);
  color: var(--primary-color);
}

.sort-option i {
  width: 20px;
  text-align: center;
}

.real-estate-filter-container {
  margin-top: 2px;
  position: fixed;
  top: 64px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  margin-left: 10px;
  padding: 10px 15px;
  box-sizing: border-box;
}

.category-filter-buttons {
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 2px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.category-filter-buttons:-webkit-scrollbar {
  height: 0px;
}

.category-filter-buttons:-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  padding-bottom: 10px;
  margin-top: 10px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 12px 20px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-size: 16px;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.real-estate-grid {
  margin-top: 90px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  padding-bottom: 140px;
}

.real-estate-card {
  background: #ffffff;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  cursor: pointer;
  position: relative;
  border: none;
  display: flex;
  flex-direction: column;
  margin-bottom: 16px;
  height: 100%;
}

.real-estate-card:hover {
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.12);
}

.real-estate-card:active {
  transform: translateY(-1px);
  transition: transform 0.1s ease;
}

.image-carousel {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
  border-radius: 12px 12px 0 0;
  margin-bottom: 0;
  box-shadow: none;
}

.carousel-item {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  transition: opacity 0.5s ease, transform 0.5s ease;
  transform: scale(1.03);
  will-change: opacity, transform;
}

.carousel-item.active {
  opacity: 1;
  transform: scale(1);
}

.carousel-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease;
}

.real-estate-card:hover .carousel-item.active img {
  transform: scale(1.05);
}

.carousel-control {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(0, 0, 0, 0.6);
  color: white;
  border: none;
  width: 36px;
  height: 36px;
  border-radius: 50%;
  cursor: pointer;
  font-size: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  opacity: 0;
  transition: all 0.3s ease;
  z-index: 5;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
  -webkit-tap-highlight-color: transparent;
}

.image-carousel:hover .carousel-control {
  opacity: 0.9;
}

.carousel-control:hover {
  background: rgba(0, 0, 0, 0.85);
  transform: translateY(-50%) scale(1.1);
}

.carousel-control:active {
  transform: translateY(-50%) scale(0.95);
}

.carousel-control.prev {
  left: 12px;
}

.carousel-control.next {
  right: 12px;
}

.carousel-pagination {
  position: absolute;
  bottom: 12px;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  gap: 8px;
  z-index: 4;
}

.pagination-dot {
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.5);
  cursor: pointer;
  transition: all 0.25s ease;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.pagination-dot.active {
  background: #fff;
  width: 20px;
  border-radius: 4px;
  box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
}

.real-estate-card-content {
  padding: 16px;
  flex: 1;
  display: flex;
  flex-direction: column;
}

.real-estate-card-header {
  display: flex;
  flex-direction: column;
  padding: 0;
  background: transparent;
  border-bottom: none;
}

.real-estate-name {
  font-family: poppins;
  font-size: 18px;
  font-weight: 600;
  color: #333;
  margin: 0 0 4px 0;
  line-height: 1.3;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
}

.real-estate-meta {
  display: flex;
  align-items: center;
  gap: 6px;
  color: #666;
  font-size: 14px;
  margin-bottom: 8px;
}

.real-estate-rating {
  display: flex;
  align-items: center;
  gap: 4px;
  margin-left: auto;
}

.real-estate-rating .star {
  color: #ffc107;
  font-weight: 600;
}

.real-estate-rating .count {
  color: #666;
  font-size: 14px;
}

.real-estate-info-grid {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 12px;
  margin: 4px 0;
}

.info-item {
  flex: 1 1 calc(50% - 6px);
  display: flex;
  align-items: flex-start;
  gap: 10px;
  padding: 12px;
  border-radius: 10px;
  background: rgba(255, 215, 0, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.1);
  transition: all 0.3s ease;
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

.info-icon {
  width: 32px;
  height: 32px;
  border-radius: 8px;
  background: rgba(255, 215, 0, 0.1);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  color: var(--primary-color);
}

.info-icon i {
  font-size: 14px;
}

.info-content {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.info-label {
  font-size: 11px;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  color: #ffc107;
  margin-bottom: 6px;
}

.info-value {
  font-size: 13px;
  font-weight: 500;
  color: #000;
  line-height: 1.4;
  display: block;
  margin-top: 0;
}

.info-value + .info-value {
  margin-top: 6px;
  position: relative;
  padding-top: 6px;
}

.info-value + .info-value::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 1px;
  background: rgba(255, 215, 0, 0.1);
}

.info-item:hover {
  background: rgba(255, 215, 0, 0.08);
  transform: translateY(-1px);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
}

.real-estate-description {
  color: #666;
  font-size: 14px;
  line-height: 1.5;
  margin: 12px 0;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}

.tag-container {
  display: flex;
  flex-wrap: wrap;
  gap: 6px;
  margin: 10px 0;
}

.tag {
  background: #f5f5f5;
  color: #666;
  padding: 4px 10px;
  border-radius: 12px;
  font-size: 12px;
  font-weight: 500;
  transition: background-color 0.2s ease;
}

.tag:hover {
  background: #eeeeee;
}

.real-estate-card-actions {
  display: flex;
  border-top: 1px solid #eee;
  padding: 0;
  background: transparent;
  margin-top: auto;
}

.action-button {
  flex: 1;
  background: transparent;
  color: #333;
  font-weight: 500;
  padding: 14px 0;
  border: none;
  border-radius: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  font-size: 14px;
  position: relative;
  transition: background-color 0.2s ease;
  -webkit-tap-highlight-color: transparent;
}

.action-button:hover {
  background: #f5f5f5;
}

.action-button:active {
  background: #eeeeee;
}

.action-button i {
  color: #333;
  font-size: 16px;
}

.action-button:first-child {
  border-right: 1px solid #eee;
}

@media (max-width: 991px) {
  .real-estate-grid {
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  }
}

@media (max-width: 768px) {
  .real-estate-grid {
    grid-template-columns: 1fr;
    padding: 0 12px;
    margin-top: 120px;
    gap: 16px;
  }
  
  .real-estate-card {
    margin-bottom: 0;
    border-radius: 12px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  .image-carousel {
    height: 180px;
    border-radius: 12px 12px 0 0;
  }
  
  .real-estate-card-content {
    padding: 14px;
  }
  
  .real-estate-name {
    font-size: 16px;
  }
  
  .real-estate-description {
    font-size: 13px;
    margin: 10px 0;
  }
  
  .tag {
    padding: 3px 8px;
    font-size: 11px;
  }
  
  .action-button {
    padding: 12px 0;
    font-size: 13px;
  }
  
  .carousel-control {
    width: 32px;
    height: 32px;
    opacity: 0.7;
  }
  
  .image-carousel .carousel-control {
    opacity: 0.7;
  }
  
  .pagination-dot {
    width: 6px;
    height: 6px;
  }
  
  .pagination-dot.active {
    width: 16px;
  }
}

@media (max-width: 480px) {
  .real-estate-card {
    margin-bottom: 0px;
  }
  
  .image-carousel {
    height: 160px;
  }
  
  .real-estate-card-content {
    padding: 12px;
  }
  
  .real-estate-description {
    -webkit-line-clamp: 2;
    margin: 8px 0;
  }
    
  .tag-container {
    margin: 8px 0;
  }
  
  .tag {
    padding: 2px 8px;
    font-size: 10px;
  }
  
  .action-button {
    padding: 10px 0;
    font-size: 12px;
  }
  
  .action-button i {
    font-size: 14px;
  }
  
  .carousel-control {
    width: 28px;
    height: 28px;
  }
  
  .real-estate-filter-container {
    padding: 12px;
  }
  
  .filter-button {
    padding: 9px 12px;
    font-size: 14px;
  }
}

@media (max-width: 768px) and (orientation: portrait) {
  .real-estate-grid {
    margin-top: 130px;
  }
  
  .real-estate-card {
    border-radius: 10px;
  }
}

.real-estate-card {
  contain: content;
  will-change: transform;
  -webkit-font-smoothing: antialiased;
  backface-visibility: hidden;
}

@media (prefers-reduced-motion: reduce) {
  .real-estate-card,
  .real-estate-card:hover,
  .carousel-item,
  .carousel-item.active,
  .carousel-item img,
  .carousel-control,
  .carousel-control:hover,
  .pagination-dot {
    transition: none;
    transform: none;
    animation: none;
  }
}

.real-estate-card:focus {
  outline: 2px solid var(--primary-color);
  outline-offset: 3px;
}

@media (prefers-color-scheme: dark) {
  .real-estate-card {
    background: #2a2a35;
  }
  
  .real-estate-name {
    color: #fff;
  }
  
  .real-estate-meta,
  .real-estate-description,
  .real-estate-rating .count {
    color: rgba(255, 255, 255, 0.7);
  }
  
  .tag {
    background: #3a3a45;
    color: rgba(255, 255, 255, 0.8);
  }
  
  .tag:hover {
    background: #4a4a55;
  }
  
  .real-estate-card-actions {
    border-top: 1px solid #3a3a45;
  }
  
  .action-button {
    color: #fff;
  }
  
  .action-button i {
    color: #fff;
  }
  
  .action-button:first-child {
    border-right: 1px solid #3a3a45;
  }
  
  .action-button:hover {
    background: #3a3a45;
  }
  
  .info-item {
    background: rgba(255, 215, 0, 0.05);
    border-color: rgba(255, 215, 0, 0.2);
  }
  
  .info-label {
    color: var(--primary-color);
  }
  
  .info-value {
    color: #fff;
  }
}

.chat-container {
  position: fixed;
  bottom: 0px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

.input-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

#realEstateExploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 12px 50px 12px 20px;
  color: #fff;
  font-size: 0.95em;
  width: 100%;
  transition: border-color 0.3s ease;
}

#realEstateExploreInput:focus {
  border-color: rgba(255, 255, 255, 0.5);
}

.voice-search {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  color: var(--primary-color);
  cursor: pointer;
  padding: 10px;
  transition: all 0.3s ease;
}

.voice-search:hover {
  opacity: 0.8;
}

.voice-search.active {
  color: #ffd700;
  animation: pulse 1.5s infinite;
}

.voice-search i { 
  font-size: 1.5em; 
}

@keyframes pulse {
  0% { transform: scale(1); }
  50% { transform: scale(1.1); }
  100% { transform: scale(1); }
}

@media (min-width: 769px) {
  .chat-container {
    padding: 24px;
    bottom: 0px;
  }

  .input-container {
    max-width: 800px;
    margin: 0 auto;
  }

  #realEstateExploreInput {
    height: 80px;
    font-size: 1.05rem;
    padding: 0 50px 0 24px;
    border-radius: 16px;
  }

  .voice-search {
    width: 50px;
    height: 50px;
    right: 8px;
  }

  .voice-search i {
    font-size: 2.1em;
  }
}

@media (min-width: 769px) and (max-width: 1024px) {
  .input-container {
    max-width: 700px;
  }
}

@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #realEstateExploreInput {
    bottom: 0px;
    width: 60%;
    max-width: 1200px;
  }
  
  .real-estate-grid {
    padding-bottom: 180px;
  }
}

@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .real-estate-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    margin-top: 80px;
    gap: 15px;
  }

  .real-estate-card {
    width: 100%;
    margin: 0;
  }

  .real-estate-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #realEstateExploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}

.full-screen-image {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.95);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 10000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
}

.full-screen-image.active {
  opacity: 1;
  visibility: visible;
}

.full-screen-image img {
  max-width: 90%;
  max-height: 90%;
  border-radius: 10px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.5);
  transform: scale(0.95);
  opacity: 0;
  transition: all 0.4s ease;
}

.full-screen-image.active img {
  transform: scale(1);
  opacity: 1;
}

.close-full-screen {
  position: absolute;
  top: 24px; 
  right: 24px;
  background: rgba(0, 0, 0, 0.5);
  border: none;
  border-radius: 50%;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-size: 24px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.close-full-screen:hover {
  background: rgba(255, 255, 255, 0.2);
  transform: scale(1.1);
}
  `;
  document.head.appendChild(style);
}

function renderRealEstateCards(filteredData) {
  return filteredData.map(item => {
    const carouselHTML = item.details.cardImages && item.details.cardImages.length > 0 ? `
      <div class="image-carousel">
        ${item.details.cardImages.map((image, index) => `
          <div class="carousel-item ${index === 0 ? 'active' : ''}">
            <img src="${image}" alt="${item.details.name}" onclick="openFullScreen('${image}')">
          </div>
        `).join('')}
        <div class="carousel-pagination">
          ${item.details.cardImages.map((_, index) => `
            <div class="pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentSlide(${index + 1}); event.stopPropagation()"></div>
          `).join('')}
        </div>
        <button class="carousel-control prev" onclick="changeCarouselSlide(event, -1); event.stopPropagation()">&#10094;</button>
        <button class="carousel-control next" onclick="changeCarouselSlide(event, 1); event.stopPropagation()">&#10095;</button>
      </div>
    ` : `
      <div class="image-carousel">
        <img src="${item.details.image}" alt="${item.details.name}" class="carousel-item active">
      </div>
    `;
    
    // Generate tags HTML if they exist
    const tagsHTML = item.details.tags && item.details.tags.length > 0 
      ? `<div class="tag-container">
          ${item.details.tags.map(tag => `<span class="tag">${tag}</span>`).join('')}
         </div>`
      : '';
    
    // Generate combined info grid similar to locator card
    const infoGridHTML = `
      <div class="real-estate-info-grid">
        <div class="info-item">
          <div class="info-icon">
            <i class="fas fa-map-marker-alt"></i>
          </div>
          <div class="info-content">
            <span class="info-label">Location & Details</span>
            <span class="info-value">${item.details.address || item.metropolis || 'N/A'}</span>
            <span class="info-value">${item.subcategory || item.category} • ${item.details.area || 'N/A'}</span>
            <span class="info-value">${item.details.price || 'Contact for price'}</span>
          </div>
        </div>
      </div>
    `;
    
    // Generate action buttons
    const actionButtonsHTML = `
      <button class="action-button" onclick="openRealEstatePage('${item.details.name}'); event.stopPropagation()">
        <i class="fas fa-info-circle"></i> Details
      </button>
      <button class="action-button" onclick="event.stopPropagation(); contactAgent('${item.details.contact?.phone || ''}')">
        <i class="fas fa-phone"></i> Contact
      </button>
    `;
    
    return `
      <div class="real-estate-card" onclick="openRealEstatePage('${item.details.name}')">
        ${carouselHTML}
        
        <div class="real-estate-card-content">
          <div class="real-estate-card-header">
            <div style="display: flex; justify-content: space-between; align-items: flex-start;">
              <h3 class="real-estate-name">${item.details.name}</h3>
              ${item.details.rating ? `
                <div class="real-estate-rating">
                  <span class="star">${item.details.rating} ★</span>
                </div>
              ` : ''}
            </div>
            
            <div class="real-estate-meta">
              <span>${item.subcategory || item.category}</span>
              ${item.metropolis ? `<span>• ${item.metropolis}</span>` : ''}
            </div>
            
            ${infoGridHTML}
            
            ${tagsHTML}
          </div>
          
          <p class="real-estate-description">${item.details.description}</p>
        </div>

        <div class="real-estate-card-actions">
          ${actionButtonsHTML}
        </div>
      </div>
    `;
  }).join('');
}

function openRealEstatePage(name) {
  const item = realEstateData.find(i => i.details.name === name);
  if (!item) return;

  const realEstatePage = document.createElement('div');
  realEstatePage.className = 'real-land-page';
  realEstatePage.innerHTML = renderRealLandPage(item);
  document.body.appendChild(realEstatePage);
  addRealLandPageStyles();
  scrollToTop();
}

function renderRealLandPage(property) {
  const headerHTML = `
    <div class="real-land-header">
      <button class="back-button" onclick="closeRealLandPage()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>${property.details.name}</h2>
      <div class="header-actions">
        <button class="share-button" onclick="shareProperty('${property.details.name}')">
          <i class="fas fa-share"></i>
        </button>
      </div>
    </div>
  `;

  const featuresHTML = property.details.features?.map(feature => `
    <div class="feature-item">
      <div class="feature-icon">
        <i class="fas fa-${feature.icon || 'check'}"></i>
      </div>
      <div class="feature-content">
        <div class="feature-name">${feature.name}</div>
        <div class="feature-value">${feature.value}</div>
      </div>
    </div>
  `).join('') || '<p>No features information available</p>';
  
  const specsHTML = property.details.specifications?.map(spec => `
    <div class="spec-item">
      <div class="spec-name">${spec.name}</div>
      <div class="spec-value">${spec.value}</div>
    </div>
  `).join('') || '<p>No specifications available</p>';
  
  const contactHTML = property.details.contact ? `
    <div class="contact-agent">
      <div class="agent-info">
        <div class="agent-name">${property.details.contact.agent || 'Land Specialist'}</div>
        ${property.details.contact.phone ? `
          <a href="tel:${property.details.contact.phone}" class="agent-phone">
            <i class="fas fa-phone"></i> ${property.details.contact.phone}
          </a>
        ` : ''}
        ${property.details.contact.email ? `
          <a href="mailto:${property.details.contact.email}" class="agent-email">
            <i class="fas fa-envelope"></i> ${property.details.contact.email}
          </a>
        ` : ''}
      </div>
      <button class="contact-button" onclick="contactAgent('${property.details.contact.phone || ''}')">
        <i class="fas fa-phone"></i> Contact Agent
      </button>
    </div>
  ` : '';
  
  const imageCarouselHTML = property.details.storeImages && property.details.storeImages.length > 0 ? `
    <div class="real-land-image-carousel">
      ${property.details.storeImages.map((image, index) => `
        <div class="real-land-carousel-item ${index === 0 ? 'active' : ''}">
          <img src="${image}" alt="${property.details.name}" onclick="openFullScreen('${image}')">
        </div>
      `).join('')}
      <div class="real-land-carousel-pagination">
        ${property.details.storeImages.map((_, index) => `
          <div class="real-land-pagination-dot ${index === 0 ? 'active' : ''}" onclick="currentRealLandSlide(${index + 1}); event.stopPropagation()"></div>
        `).join('')}
      </div>
      <button class="real-land-carousel-control prev" onclick="changeRealLandSlide(event, -1); event.stopPropagation()">&#10094;</button>
      <button class="real-land-carousel-control next" onclick="changeRealLandSlide(event, 1); event.stopPropagation()">&#10095;</button>
    </div>
  ` : `
    <div class="real-land-image-container">
      <img src="${property.details.image}" alt="${property.details.name}" class="real-land-image">
    </div>
  `;

  return `
    ${headerHTML}
    
    <div class="real-land-content">
      <!-- Hero Section -->
      <div class="real-land-hero">
        ${imageCarouselHTML}
        <div class="real-land-basic-info">
          <div class="real-land-rating">
            <i class="fas fa-star"></i> ${property.details.rating || '4.0'} (${property.details.reviews || '100+'})
          </div>
          <div class="real-land-type">
            <i class="fas fa-map-marked-alt"></i> ${property.subcategory || property.category}
          </div>
          <div class="real-land-location">
            <i class="fas fa-location-dot"></i> ${property.metropolis || 'N/A'}
          </div>
          <div class="real-land-price">
            <i class="fas fa-tag"></i> ${property.details.price || 'Contact for price'}
          </div>
        </div>
      </div>
      
      <!-- Description Section -->
      <div class="real-land-description-section">
        <h3>Land Details</h3>
        <p>${property.details.description}</p>
      </div>
      
      <!-- Features Section -->
      <div class="real-land-features-section">
        <h3>Key Features</h3>
        <div class="features-grid">
          ${featuresHTML}
        </div>
      </div>
      
      <!-- Specifications Section -->
      <div class="real-land-specs-section">
        <h3>Specifications</h3>
        <div class="specs-grid">
          ${specsHTML}
        </div>
      </div>
      
      <!-- Location Section -->
      <div class="real-land-location-section">
        <h3>Location Details</h3>
        <div class="location-info">
          <div class="address">
            <i class="fas fa-map-marker-alt"></i>
            <span>${property.details.address}</span>
          </div>
          <a href="${property.details.mapLink}" target="_blank" class="map-button">
            <i class="fas fa-map-marked-alt"></i> View on Map
          </a>
        </div>
      </div>
      
      <!-- Contact Section -->
      ${contactHTML}
      
      <!-- Action Buttons -->
      <div class="real-land-actions">
        <button class="primary-action" onclick="contactAgent('${property.details.contact?.phone || ''}')">
          <i class="fas fa-phone"></i> Contact Agent
        </button>
        <a href="${property.details.mapLink}" target="_blank" class="secondary-action">
          <i class="fas fa-directions"></i> Get Directions
        </a>
      </div>
    </div>
  `;
}

function addRealLandPageStyles() {
  const style = document.createElement('style');
  style.textContent = `
    /* Real Land Page Container */
    .real-land-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      color: #fff;
      font-family: 'Poppins', sans-serif;
    }

    /* Real Land Header */
    .real-land-header {
      position: sticky;
      top: 0;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 16px 20px;
      background: rgba(26, 26, 31, 0.95);
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
      z-index: 10;
      backdrop-filter: blur(12px);
    }

    .back-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.5em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s ease;
    }

    .back-button:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    .real-land-header h2 {
      margin: 0;
      font-size: 16px;
      font-weight: 600;
      text-align: center;
      flex: 1;
      padding: 0 16px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .header-actions {
      display: flex;
      gap: 8px;
    }

    .share-button {
      background: none;
      border: none;
      color: #fff;
      font-size: 1.2em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      display: flex;
      align-items: center;
      justify-content: center;
      transition: background 0.2s ease;
    }

    .share-button:hover {
      background: rgba(255, 255, 255, 0.1);
    }

    /* Real Land Content */
    .real-land-content {
      padding: 0 0 100px;
      width: 100%;
      margin: 0 auto;
    }

    /* Hero Section */
    .real-land-hero {
      position: relative;
      padding: 20px;
    }

    .real-land-image-carousel {
      position: relative;
      width: 100%;
      height: 250px;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
    }

    .real-land-carousel-item {
      position: absolute;
      width: 100%;
      height: 100%;
      opacity: 0;
      transition: opacity 0.5s ease, transform 0.5s ease;
      transform: scale(1.03);
      will-change: opacity, transform;
    }

    .real-land-carousel-item.active {
      opacity: 1;
      transform: scale(1);
    }

    .real-land-carousel-item img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.4s ease;
    }

    .real-land-image-carousel:hover .real-land-carousel-item.active img {
      transform: scale(1.05);
    }

    .real-land-carousel-control {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0, 0, 0, 0.6);
      color: white;
      border: none;
      width: 36px;
      height: 36px;
      border-radius: 50%;
      cursor: pointer;
      font-size: 16px;
      display: flex;
      align-items: center;
      justify-content: center;
      opacity: 0;
      transition: all 0.3s ease;
      z-index: 5;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
      -webkit-tap-highlight-color: transparent;
    }

    .real-land-image-carousel:hover .real-land-carousel-control {
      opacity: 0.9;
    }

    .real-land-carousel-control:hover {
      background: rgba(0, 0, 0, 0.85);
      transform: translateY(-50%) scale(1.1);
    }

    .real-land-carousel-control:active {
      transform: translateY(-50%) scale(0.95);
    }

    .real-land-carousel-control.prev {
      left: 12px;
    }

    .real-land-carousel-control.next {
      right: 12px;
    }

    .real-land-carousel-pagination {
      position: absolute;
      bottom: 12px;
      left: 0;
      right: 0;
      display: flex;
      justify-content: center;
      gap: 8px;
      z-index: 4;
    }

    .real-land-pagination-dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: rgba(255, 255, 255, 0.5);
      cursor: pointer;
      transition: all 0.25s ease;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.2);
      border: 1px solid rgba(255, 255, 255, 0.2);
    }

    .real-land-pagination-dot.active {
      background: #fff;
      width: 20px;
      border-radius: 4px;
      box-shadow: 0 0 6px rgba(255, 255, 255, 0.5);
    }

    .real-land-image-container {
      width: 100%;
      height: 250px;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
    }

    .real-land-image {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.3s ease;
    }

    .real-land-image:hover {
      transform: scale(1.02);
    }

    .real-land-basic-info {
      display: flex;
      gap: 12px;
      margin-top: 16px;
      flex-wrap: wrap;
    }

    .real-land-rating,
    .real-land-type,
    .real-land-location,
    .real-land-price {
      display: flex;
      align-items: center;
      gap: 6px;
      padding: 8px 12px;
      border-radius: 20px;
      font-size: 0.9em;
      font-weight: 500;
    }

    .real-land-rating {
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
    }

    .real-land-type {
      background: rgba(0, 191, 255, 0.1);
      color: #00bfff;
    }

    .real-land-location {
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
    }

    .real-land-price {
      background: rgba(76, 175, 80, 0.1);
      color: #4CAF50;
    }

    /* Description Section */
    .real-land-description-section {
      padding: 0 20px 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-description-section h3 {
      margin: 0 0 12px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .real-land-description-section p {
      margin: 0;
      line-height: 1.6;
      color: rgba(255, 255, 255, 0.8);
    }

    /* Features Section */
    .real-land-features-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-features-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .features-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
    }

    .feature-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 8px;
      padding: 12px;
      display: flex;
      gap: 10px;
      align-items: center;
    }

    .feature-icon {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      background: rgba(255, 215, 0, 0.1);
      display: flex;
      align-items: center;
      justify-content: center;
      color: #ffd700;
      flex-shrink: 0;
    }

    .feature-content {
      display: flex;
      flex-direction: column;
    }

    .feature-name {
      font-size: 0.9em;
      color: rgba(255, 255, 255, 0.7);
    }

    .feature-value {
      font-weight: 500;
      font-size: 0.95em;
    }

    /* Specifications Section */
    .real-land-specs-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-specs-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .specs-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
    }

    .spec-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 8px;
      padding: 12px;
    }

    .spec-name {
      color: rgba(255, 255, 255, 0.7);
      font-size: 0.9em;
      margin-bottom: 4px;
    }

    .spec-value {
      font-weight: 500;
    }

    /* Location Section */
    .real-land-location-section {
      padding: 20px;
      border-bottom: 1px solid rgba(255, 255, 255, 0.1);
    }

    .real-land-location-section h3 {
      margin: 0 0 16px;
      font-size: 1.2em;
      font-weight: 600;
    }

    .location-info {
      display: flex;
      flex-direction: column;
      gap: 12px;
    }

    .address {
      display: flex;
      align-items: flex-start;
      gap: 8px;
    }

    .address i {
      margin-top: 2px;
    }

    .map-button {
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      padding: 12px;
      background: rgba(255, 215, 0, 0.1);
      color: #ffd700;
      border-radius: 8px;
      text-decoration: none;
      font-weight: 500;
    }

    /* Contact Agent Section */
    .contact-agent {
      margin: 20px;
      padding: 16px;
      background: rgba(255, 255, 255, 0.05);
      border-radius: 12px;
      display: flex;
      flex-direction: column;
      gap: 16px;
    }

    .agent-info {
      display: flex;
      flex-direction: column;
      gap: 8px;
    }

    .agent-name {
      font-weight: 600;
    }

    .agent-phone,
    .agent-email {
      display: flex;
      align-items: center;
      gap: 8px;
      color: #fff;
      text-decoration: none;
    }

    .contact-button {
      padding: 12px;
      background: var(--primary-color);
      color: #000;
      border: none;
      border-radius: 8px;
      font-weight: 600;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      cursor: pointer;
      transition: transform 0.3s ease;
    }

    .contact-button:hover {
      transform: translateY(-2px);
    }

.real-land-actions {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 16px 20px;
  background: rgba(26, 26, 31, 0.95);
  border-top: 1px solid rgba(255, 255, 255, 0.1);
  display: flex;
  gap: 12px;
  z-index: 10;
  backdrop-filter: blur(12px);
}

.primary-action,
.secondary-action {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  padding: 14px;
  border-radius: 8px;
  font-weight: 500;
  font-size: 0.95em;
  cursor: pointer;
  transition: transform 0.3s ease, background 0.3s ease;
}

.primary-action {
  background: var(--primary-color, #ffd700);
  color: #000;
  border: none;
}

.primary-action:hover {
  background: #ffd700;
  transform: translateY(-2px);
}

.secondary-action {
  background: rgba(255, 255, 255, 0.1);
  color: #fff;
  border: none;
}

.secondary-action:hover {
  background: rgba(255, 255, 255, 0.15);
  transform: translateY(-2px);
}

    /* Tablet Styles */
    @media (min-width: 768px) {
      .real-land-header {
        padding: 20px 30px;
      }
      
      .real-land-header h2 {
        font-size: 1.4em;
      }
      
      .real-land-content {
        max-width: 90%;
        margin: 0 auto;
      }
      
      .real-land-hero {
        padding: 30px;
      }
      
      .real-land-image-carousel,
      .real-land-image-container {
        height: 350px;
        border-radius: 20px;
      }
      
      .real-land-basic-info {
        gap: 16px;
        margin-top: 24px;
      }
      
      .real-land-rating,
      .real-land-type,
      .real-land-location,
      .real-land-price {
        padding: 10px 16px;
        font-size: 1em;
      }
      
      .real-land-description-section,
      .real-land-features-section,
      .real-land-specs-section,
      .real-land-location-section {
        padding: 0 30px 30px;
      }
      
      .real-land-description-section h3,
      .real-land-features-section h3,
      .real-land-specs-section h3,
      .real-land-location-section h3 {
        font-size: 1.3em;
        margin-bottom: 16px;
      }
      
      .real-land-description-section p {
        font-size: 1em;
        line-height: 1.6;
      }
      
      .features-grid,
      .specs-grid {
        grid-template-columns: repeat(3, 1fr);
        gap: 16px;
      }
      
      .real-land-actions {
        padding: 20px 30px;
        max-width: 90%;
        margin: 0 auto;
        left: 50%;
        transform: translateX(-50%);
        border-radius: 12px 12px 0 0;
      }
      
      .primary-action,
      .secondary-action {
        padding: 14px;
        font-size: 1em;
      }
    }

    /* Desktop Styles */
    @media (min-width: 1024px) {
      .real-land-content {
        max-width: 900px;
        margin: 0 auto;
        padding: 0 0 120px;
      }
      
      .real-land-header {
        padding: 22px 32px;
      }
      
      .real-land-header h2 {
        font-size: 16px;
      }
      
      .real-land-hero {
        display: grid;
        grid-template-columns: 1fr 1fr;
        gap: 30px;
        padding: 30px;
        align-items: center;
      }
      
      .real-land-image-carousel,
      .real-land-image-container {
        height: 400px;
        border-radius: 20px;
      }
      
      .real-land-basic-info {
        margin-top: 0;
      }
      
      .real-land-description-section {
        padding: 0 30px 30px;
        max-width: 85%;
        margin: 0 auto;
      }
      
      .real-land-description-section h3 {
        font-size: 1.5em;
      }
      
      .real-land-description-section p {
        font-size: 1.05em;
        line-height: 1.7;
      }
      
      .real-land-features-section,
      .real-land-specs-section,
      .real-land-location-section {
        padding: 30px;
      }
      
      .real-land-features-section h3,
      .real-land-specs-section h3,
      .real-land-location-section h3 {
        font-size: 1.5em;
      }
      
      .features-grid {
        grid-template-columns: repeat(4, 1fr);
      }
      
      .specs-grid {
        grid-template-columns: repeat(4, 1fr);
      }
      
  .real-land-actions {
    max-width: 700px;
    left: 50%;
    transform: translateX(-50%);
  }
  
  .primary-action,
  .secondary-action {
    padding: 16px;
    font-size: 1.05em;
  }
 }

    /* Large Desktop Styles */
    @media (min-width: 1440px) {
      .real-land-content {
        max-width: 1000px;
      }
      
      .real-land-hero {
        grid-template-columns: 3fr 2fr;
      }
      
      .real-land-image-carousel,
      .real-land-image-container {
        height: 450px;
      }
      
      .real-land-description-section {
        max-width: 80%;
      }
      
      .features-grid {
        grid-template-columns: repeat(5, 1fr);
      }
      
      .specs-grid {
        grid-template-columns: repeat(5, 1fr);
      }
      
      .real-land-actions {
        max-width: 800px;
        left: 50%;
      }
    }
  `;
  document.head.appendChild(style);
}

function closeRealLandPage() {
  const page = document.querySelector('.real-land-page');
  if (page) {
    page.style.opacity = '0';
    page.style.transform = 'translateY(20px)';
    setTimeout(() => {
      document.body.removeChild(page);
    }, 300);
  }
}

function shareProperty(propertyName) {
  if (navigator.share) {
    navigator.share({
      title: propertyName,
      text: 'Check out this property I found',
      url: window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    const shareUrl = window.location.href;
    alert(`Share this property: ${propertyName}\n${shareUrl}`);
  }
}

function filterRealEstate(category) {
  const grid = document.getElementById('realEstateGrid');
  let filteredData = realEstateData;

  if (category !== 'All') {
    filteredData = filteredData.filter(item => item.category === category);
  }

  grid.innerHTML = renderRealEstateCards(filteredData);

  const filterButtons = document.querySelectorAll('.category-filter-buttons .filter-button');
  filterButtons.forEach(button => {
    if (button.getAttribute('data-category') === category) {
      button.classList.add('active');
    } else {
      button.classList.remove('active');
    }
  });
}

function sortRealEstate(criteria) {
  const grid = document.getElementById('realEstateGrid');
  let sortedData = [...realEstateData];

  switch(criteria) {
    case 'rating':
      sortedData.sort((a, b) => {
        const ratingA = parseFloat(a.details.rating) || 0;
        const ratingB = parseFloat(b.details.rating) || 0;
        return ratingB - ratingA;
      });
      break;
    case 'price':
      sortedData.sort((a, b) => {
        const priceA = extractPriceValue(a.details.price) || 0;
        const priceB = extractPriceValue(b.details.price) || 0;
        return priceB - priceA;
      });
      break;
    case 'name':
      sortedData.sort((a, b) => a.details.name.localeCompare(b.details.name));
      break;
    case 'distance':
      sortedData.sort(() => Math.random() - 0.5);
      break;
  }

  grid.innerHTML = renderRealEstateCards(sortedData);
  toggleSortOptions();
}

function extractPriceValue(priceStr) {
  if (!priceStr) return 0;
  const match = priceStr.match(/\d[\d,.]*/);
  if (match) {
    return parseFloat(match[0].replace(/,/g, ''));
  }
  return 0;
}

function toggleSortOptions() {
  const container = document.getElementById('sortOptionsContainer');
  container.classList.toggle('active');
}

function hideRealEstateOverlay() {
  const overlay = document.getElementById('realEstateOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
    }, 300);
  }
}

function shareRealEstate(name) {
  const item = realEstateData.find(i => i.details.name === name);
  if (!item) return;

  if (navigator.share) {
    navigator.share({
      title: item.details.name,
      text: `Check out ${item.details.name} - ${item.details.description}`,
      url: item.details.mapLink || window.location.href,
    }).catch(err => {
      console.log('Error sharing:', err);
    });
  } else {
    const shareUrl = item.details.mapLink || window.location.href;
    const shareText = `Check out ${item.details.name}: ${shareUrl}`;
    prompt('Copy this link to share:', shareText);
  }
}

function contactAgent(phoneNumber) {
  event.stopPropagation();
  if (phoneNumber) {
    window.open(`tel:${phoneNumber}`);
  } else {
    alert('Contact information would appear here in a real implementation.');
  }
}

// Carousel functions for real land page
function changeRealLandSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.real-land-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.real-land-carousel-item');
  const dots = carousel.querySelectorAll('.real-land-pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentRealLandSlide(index) {
  const carousel = event.target.closest('.real-land-image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.real-land-carousel-item');
  const dots = carousel.querySelectorAll('.real-land-pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

// Helper functions
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    const context = this;
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(context, args), wait);
  };
}

function performSearch(data, query, container, filterButtons, renderFunction) {
  if (!query.trim()) {
    container.innerHTML = renderFunction(data);
    return;
  }

  const searchTerm = query.toLowerCase();
  const filteredData = data.filter(item => 
    item.details.name.toLowerCase().includes(searchTerm) ||
    (item.category && item.category.toLowerCase().includes(searchTerm)) ||
    (item.metropolis && item.metropolis.toLowerCase().includes(searchTerm)) ||
    (item.details.description && item.details.description.toLowerCase().includes(searchTerm))
  );

  container.innerHTML = renderFunction(filteredData);
}

function initializeOverlayVoiceSearch(inputElement, callback) {
  const voiceSearchButton = document.getElementById('voiceSearchButton');
  if (!voiceSearchButton) return;

  voiceSearchButton.addEventListener('click', () => {
    if (!('webkitSpeechRecognition' in window)) {
      alert('Voice search is not supported in your browser');
      return;
    }

    const recognition = new webkitSpeechRecognition();
    recognition.continuous = false;
    recognition.interimResults = false;

    recognition.onstart = () => {
      voiceSearchButton.classList.add('active');
    };

    recognition.onresult = (event) => {
      const transcript = event.results[0][0].transcript;
      inputElement.value = transcript;
      callback(transcript);
    };

    recognition.onerror = (event) => {
      console.error('Voice recognition error', event.error);
      voiceSearchButton.classList.remove('active');
    };

    recognition.onend = () => {
      voiceSearchButton.classList.remove('active');
    };

    recognition.start();
  });
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

// Carousel functions
function changeCarouselSlide(event, direction) {
  event.stopPropagation();
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');
  let activeIndex = -1;

  items.forEach((item, index) => {
    if (item.classList.contains('active')) {
      activeIndex = index;
      item.classList.remove('active');
      dots[index].classList.remove('active');
    }
  });

  let newIndex = activeIndex + direction;
  if (newIndex < 0) newIndex = items.length - 1;
  if (newIndex >= items.length) newIndex = 0;

  items[newIndex].classList.add('active');
  dots[newIndex].classList.add('active');
}

function currentSlide(index) {
  const carousel = event.target.closest('.image-carousel');
  if (!carousel) return;

  const items = carousel.querySelectorAll('.carousel-item');
  const dots = carousel.querySelectorAll('.pagination-dot');

  items.forEach((item, i) => {
    item.classList.remove('active');
    dots[i].classList.remove('active');
    if (i === index - 1) {
      item.classList.add('active');
      dots[i].classList.add('active');
    }
  });
}

function openFullScreen(imageUrl) {
  event.stopPropagation();
  const fullScreenDiv = document.createElement('div');
  fullScreenDiv.className = 'full-screen-image';
  fullScreenDiv.innerHTML = `
    <img src="${imageUrl}" alt="Full screen">
    <button class="close-full-screen" onclick="document.querySelector('.full-screen-image').remove()">
      <i class="fas fa-times"></i>
    </button>
  `;
  document.body.appendChild(fullScreenDiv);
  
  setTimeout(() => {
    fullScreenDiv.classList.add('active');
    const img = fullScreenDiv.querySelector('img');
    if (img) {
      img.style.opacity = '1';
      img.style.transform = 'scale(1)';
    }
  }, 10);
}

// Modified hide functions for each overlay
function hideLocatorsOverlay() {
  const overlay = document.getElementById('locatorsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}

function hideProvidersOverlay() {
  const overlay = document.getElementById('providersOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}

function hidePlacesOverlay() {
  const overlay = document.getElementById('placesOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => {
      overlay.remove();
      overlayState.currentOverlay = null;
    }, 300);
  }
}


// First, add the voice search functionality
function initializeVoiceSearch(detailName) {
  if ('webkitSpeechRecognition' in window || 'SpeechRecognition' in window) {
    const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
    const recognition = new SpeechRecognition();
    
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.lang = 'en-US';

    recognition.onresult = (event) => {
      const voiceQuery = event.results[0][0].transcript;
      const searchInput = document.getElementById('chatSearchInput');
      searchInput.value = voiceQuery;
      searchFAQs(voiceQuery, detailName);
    };

    recognition.onerror = (event) => {
      console.error('Speech recognition error:', event.error);
      showVoiceSearchError(event.error);
    };

    return recognition;
  }
  return null;
}

function showVoiceSearchError(error) {
  const searchResults = document.getElementById('searchResults');
  searchResults.innerHTML = `
    <div class="voice-search-error">
      <i class="fas fa-microphone-slash"></i>
      <h3>Voice Search Error</h3>
      <p>${getVoiceErrorMessage(error)}</p>
    </div>
  `;
}

function getVoiceErrorMessage(error) {
  switch (error) {
    case 'not-allowed':
      return 'Microphone access was denied. Please enable microphone access to use voice search.';
    case 'no-speech':
      return 'No speech was detected. Please try again.';
    case 'network':
      return 'Network error occurred. Please check your connection and try again.';
    default:
      return 'An error occurred with voice search. Please try again.';
  }
}

function showChatSupport(detailName) {
  const chatPage = document.createElement('div');
  chatPage.className = 'chat-support-page';
  chatPage.innerHTML = `
    <div class="chat-support-header">
      <button class="back-button" onclick="hideChatSupport()">
        <i class="fas fa-arrow-left"></i>
      </button>
      <h2>FAQ Chat Support - ${detailName}</h2>
    </div>
    <div class="chat-support-content">
      <div class="faq-section">
        <h3>Frequently Asked Questions</h3>
        <div class="faq-list" id="faqList">
          ${renderFAQsByDetail(detailName)}
        </div>
      </div>
      <div class="contact-support" id="contactSupport">
        <div class="contact-support-header">
          <h3>Need More Help?</h3>
          <button class="refresh-button" onclick="refreshChatSupport('${detailName}')">
            <i class="fas fa-sync-alt"></i>
          </button>
        </div>
        <a href="mailto:support@example.com" class="contact-button">
          <i class="fas fa-envelope"></i> Email Support
        </a>
        <a href="tel:+1234567890" class="contact-button">
          <i class="fas fa-phone"></i> Call Support
        </a>
      </div>
      <div class="search-results" id="searchResults"></div>
    </div>
    <div class="chat-search-container">
      <div class="input-container">
        <input type="text" id="chatSearchInput" placeholder="Search for answers..." />
        <button class="voice-search" id="voiceSearchBtn">
          <i class="fas fa-microphone"></i>
        </button>
      </div>
    </div>
  `;

  document.body.appendChild(chatPage);
  addChatSupportStyles();
  initializeFAQToggle();
  initializeChatSearch(detailName);
  initializeVoiceSearchButton(detailName);
}

function initializeVoiceSearchButton(detailName) {
  const voiceSearchBtn = document.getElementById('voiceSearchBtn');
  const recognition = initializeVoiceSearch(detailName);
  
  if (recognition) {
    let isListening = false;

    voiceSearchBtn.addEventListener('click', () => {
      if (!isListening) {
        recognition.start();
        voiceSearchBtn.classList.add('listening');
        voiceSearchBtn.querySelector('i').classList.remove('fa-microphone');
        voiceSearchBtn.querySelector('i').classList.add('fa-microphone-slash');
      } else {
        recognition.stop();
        voiceSearchBtn.classList.remove('listening');
        voiceSearchBtn.querySelector('i').classList.remove('fa-microphone-slash');
        voiceSearchBtn.querySelector('i').classList.add('fa-microphone');
      }
      isListening = !isListening;
    });

    recognition.onend = () => {
      voiceSearchBtn.classList.remove('listening');
      voiceSearchBtn.querySelector('i').classList.remove('fa-microphone-slash');
      voiceSearchBtn.querySelector('i').classList.add('fa-microphone');
      isListening = false;
    };
  } else {
    voiceSearchBtn.style.display = 'none';
  }
}

function renderFAQsByDetail(detailName) {
  const faqs = getFAQsByDetail(detailName);
  return faqs.map(faq => `
    <div class="faq-item" onclick="toggleFAQAnswer('${faq.id}')">
      <div class="faq-question">
        <span>${faq.question}</span>
        <i class="fas fa-chevron-down"></i>
      </div>
      <div class="faq-answer" id="faqAnswer${faq.id}">
        <p>${faq.answer}</p>
      </div>
    </div>
  `).join('');
}

function getFAQsByDetail(detailName) {
  // Example FAQs, you can replace this with your own data
  const faqs = {
    "Dmart": [
      { id: 1, question: "What are the store timings for Dmart?", answer: "Dmart stores are open from 9 AM to 10 PM, Monday to Sunday." },
      { id: 2, question: "Does Dmart offer home delivery?", answer: "Yes, Dmart offers home delivery through their app and website." }
    ],
    "Amazon": [
      { id: 3, question: "How do I track my Amazon order?", answer: "You can track your order by logging into your Amazon account and visiting the 'Your Orders' section." },
      { id: 4, question: "What is Amazon Prime?", answer: "Amazon Prime is a subscription service that offers benefits like free delivery, access to streaming services, and more." }
    ],
    "Delhi Plumbing Services": [
      { id: 5, question: "What services do you offer?", answer: "We offer a wide range of plumbing services including pipe repair, leak fixing, and more." },
      { id: 6, question: "Are your plumbers certified?", answer: "Yes, all our plumbers are certified and experienced professionals." }
    ],
    "Lodhi Garden": [
      { id: 7, question: "What are the entry fees for Lodhi Garden?", answer: "Entry to Lodhi Garden is free for all visitors." },
      { id: 8, question: "Are pets allowed in Lodhi Garden?", answer: "Yes, pets are allowed in Lodhi Garden but must be kept on a leash." }
    ]
  };

  return faqs[detailName] || [];
}

function toggleFAQ(index) {
  const answer = document.getElementById(`faq-answer-${index}`);
  const question = answer.previousElementSibling;
  const chevron = question.querySelector('i');

  if (answer.classList.contains('active')) {
    // Collapse the answer
    answer.style.maxHeight = `${answer.scrollHeight}px`;
    setTimeout(() => {
      answer.style.maxHeight = '0';
    }, 10);
  } else {
    // Expand the answer
    answer.style.maxHeight = `${answer.scrollHeight}px`;
  }

  // Toggle the active class after a slight delay
  setTimeout(() => {
    answer.classList.toggle('active');
    question.classList.toggle('active');
    chevron.classList.toggle('fa-chevron-down');
    chevron.classList.toggle('fa-chevron-up');
  }, 10);
}

// Function to initialize FAQ toggle functionality
function initializeFAQToggle() {
  const faqQuestions = document.querySelectorAll('.faq-question');
  faqQuestions.forEach(question => {
    question.addEventListener('click', () => {
      const answer = question.nextElementSibling;
      answer.classList.toggle('active');
      question.classList.toggle('active');
    });
  });
}

// Function to initialize chat search functionality
function initializeChatSearch() {
  const chatSearchInput = document.getElementById('chatSearchInput');
  if (chatSearchInput) {
    chatSearchInput.addEventListener('input', (e) => {
      const query = e.target.value.trim();
      if (query) {
        searchFAQs(query);
      } else {
        clearSearchResults();
      }
    });
  }
}

// Function to search FAQs
function searchFAQs(query) {
  const faqList = document.getElementById('faqList');
  const searchResults = document.getElementById('searchResults');
  if (faqList && searchResults) {
    const faqItems = faqList.querySelectorAll('.faq-item');
    let resultsFound = false;

    faqItems.forEach(item => {
      const question = item.querySelector('.faq-question span').textContent.toLowerCase();
      const answer = item.querySelector('.faq-answer p').textContent.toLowerCase();
      if (question.includes(query.toLowerCase()) || answer.includes(query.toLowerCase())) {
        const resultItem = document.createElement('div');
        resultItem.className = 'search-result-item';
        resultItem.innerHTML = `
          <h4>${item.querySelector('.faq-question span').textContent}</h4>
          <p>${item.querySelector('.faq-answer p').textContent}</p>
        `;
        searchResults.appendChild(resultItem);
        resultsFound = true;
      }
    });

    if (!resultsFound) {
      searchResults.innerHTML = `<p>No results found for "${query}".</p>`;
    }
  }
}

// Function to clear search results
function clearSearchResults() {
  const searchResults = document.getElementById('searchResults');
  if (searchResults) {
    searchResults.innerHTML = '';
  }
}

// Function to hide chat support
function hideChatSupport() {
  const chatPage = document.querySelector('.chat-support-page');
  if (chatPage) {
    chatPage.remove();
  }
}

function refreshChatSupport(detailName) {
  const faqList = document.getElementById('faqList');
  if (faqList) {
    faqList.innerHTML = renderFAQsByDetail(detailName);
  }
}

// Function to add chat support styles
function addChatSupportStyles() {
  const style = document.createElement('style');
  style.textContent = `
    .chat-support-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      animation: slideIn 0.3s ease-out;
    }

    @keyframes slideIn {
      from { transform: translateX(100%); }
      to { transform: translateX(0); }
    }

    .chat-support-header {
      position: sticky;
      top: 0;
      background: rgba(26, 26, 31, 0.95);
      font-size: 11px;
      padding: 20px;
      display: flex;
      align-items: center;
      gap: 20px;
      border-bottom: 2px solid var(--primary-color);
      backdrop-filter: blur(10px);
      z-index: 10;
    }

    .back-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 10px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .back-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .chat-support-content {
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }

    .faq-section {
      margin-top: 0px;
      transition: all 0.3s ease;
    }

    .faq-section h3 {
  color: #fff;
  margin-bottom: 14px;
  font-size: 20px;
  padding-left: 0px;
}

    .faq-item {
      background: rgba(255, 215, 0, 0.05);
      border-radius: 10px;
      margin-top: 5px;
      overflow: hidden;
      transition: all 0.3s ease;
    }

    .faq-question {
      padding: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      color: var(--text-light);
      font-weight: 500;
      transition: all 0.3s ease;
    }

    .faq-question:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .faq-question i {
      transition: transform 0.3s ease;
    }

    .faq-question.active i {
      transform: rotate(180deg);
    }

    .faq-answer {
      padding: 0 20px;
      max-height: 0;
      overflow: hidden;
      transition: all 0.3s ease;
      color: #aaa;
    }

    .faq-answer.active {
      padding: 20px;
      max-height: 500px;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
    }

  
    .contact-support {
      display: none;
      animation: fadeIn 0.3s ease-out;
      padding: 40px 20px;
      background: rgba(255, 215, 0, 0.05);
      border-radius: 15px;
      text-align: center;
      margin: 20px auto;
      max-width: 600px;
    }

    .contact-support-header {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 15px;
      margin-bottom: 20px;
    }

    .refresh-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .refresh-button:hover {
      background: rgba(255, 215, 0, 0.1);
      transform: rotate(180deg);
    }

    .contact-button {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 15px 25px;
      background: rgba(255, 215, 0, 0.1);
      border-radius: 10px;
      color: var(--primary-color);
      text-decoration: none;
      transition: all 0.3s ease;
      margin: 10px 0;
    }

    .contact-button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: translateX(5px);
    }

    .chat-search-container {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      width: 100%;
      max-width: 100%;
      z-index: 1000;
      padding: 20px;
      background: #1a1a1f;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
      display: flex;
      justify-content: center;
      align-items: center;
      box-sizing: border-box;
    }

    .input-container {
      position: relative;
      width: 100%;
      max-width: 800px;
      display: flex;
      align-items: center;
    }

    #chatSearchInput {
      width: 100%;
      background: rgba(255, 255, 255, 0.05);
      border: 1px solid rgba(255, 215, 0, 0.2);
      border-radius: 12px;
      padding: 12px 50px 12px 20px;
      color: #fff;
      font-size: 0.95em;
      height: 60px;
      transition: border-color 0.3s ease;
    }

    #chatSearchInput:focus {
      border-color: rgba(255, 255, 255, 0.5);
      outline: none;
    }

    .voice-search {
      position: absolute;
      right: 10px;
      background: none;
      border: none;
      color: var(--primary-color);
      cursor: pointer;
      padding: 10px;
      transition: all 0.3s ease;
    }

    .voice-search:hover {
      opacity: 0.8;
    }

    .voice-search.active {
      color: #ffd700;
      animation: pulse 1.5s infinite;
    }

    .voice-search i {
      font-size: 1.5em;
    }

    @keyframes pulse {
      0% { transform: scale(1); }
      50% { transform: scale(1.1); }
      100% { transform: scale(1); }
    }

    .voice-search-error {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-light);
  }

  .voice-search-error i {
    font-size: 3em;
    color: #ff4444;
    margin-bottom: 20px;
  }

  /* Responsive styles for the new button */
  @media (max-width: 768px) {
    .voice-search-btn {
      padding: 20px 20px;
    }
  }


    /* Desktop Specific Styles */
    @media (min-width: 769px) {
      .chat-search-container {
        padding: 24px;
        bottom: 0;
      }

      .input-container {
        max-width: 800px;
        margin: 0 auto;
      }

      #chatSearchInput {
        height: 80px;
        font-size: 1.05rem;
        padding: 0 50px 0 24px;
        border-radius: 16px;
      }

      .voice-search {
        width: 50px;
        height: 50px;
        right: 15px;
      }

      .voice-search i {
        font-size: 2.1em;
      }
    }

    /* Mobile styles */
    @media (max-width: 768px) {
    .chat-search-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }
      #chatSearchInput {
    width: 100%;
    padding: 20px 20px;
  }
}

      .voice-search {
        width: 40px;
        height: 40px;
        right: 5px;
      }

      .voice-search i {
        font-size: 1.3em;
      }
    }

  `;
  document.head.appendChild(style);
}

function searchFAQs(query, detailName) {
  const faqList = document.getElementById('faqList');
  const searchResults = document.getElementById('searchResults');
  const contactSupport = document.getElementById('contactSupport');
  
  if (!query.trim()) {
    clearSearchResults();
    faqList.style.display = 'block';
    contactSupport.style.display = 'none';
    return;
  }

  if (faqList && searchResults) {
    const faqItems = faqList.querySelectorAll('.faq-item');
    const results = [];
    
    faqItems.forEach(item => {
      const question = item.querySelector('.faq-question span').textContent;
      const answer = item.querySelector('.faq-answer p').textContent;
      
      // Calculate relevance score using fuzzy matching
      const questionScore = calculateRelevance(query, question);
      const answerScore = calculateRelevance(query, answer);
      const totalScore = Math.max(questionScore, answerScore);

      if (totalScore > 0.3) { // Threshold for relevance
        results.push({
          question,
          answer,
          score: totalScore,
          element: item.cloneNode(true)
        });
      }
    });

    // Sort results by relevance score
    results.sort((a, b) => b.score - a.score);
    
    // Clear previous results
    searchResults.innerHTML = '';
    faqList.style.display = 'none';
    
    if (results.length > 0) {
      // Add search summary
      const summary = document.createElement('div');
      summary.className = 'search-summary';
      summary.innerHTML = `Found ${results.length} result${results.length === 1 ? '' : 's'} for "${query}"`;
      searchResults.appendChild(summary);

      // Display results with highlighted matches
      results.forEach(result => {
        const resultItem = document.createElement('div');
        resultItem.className = 'search-result-item';
        resultItem.innerHTML = `
          <h4>${highlightMatches(result.question, query)}</h4>
          <p>${highlightMatches(result.answer, query)}</p>
          <div class="relevance-indicator" style="width: ${result.score * 100}%"></div>
        `;
        searchResults.appendChild(resultItem);
      });
    } else {
      // Show "no results" message with suggestions
      searchResults.innerHTML = `
        <div class="no-results">
          <i class="fas fa-search"></i>
          <h3>No results found for "${query}"</h3>
          <p>Try:</p>
          <ul>
            <li>Checking your spelling</li>
            <li>Using more general terms</li>
            <li>Using fewer keywords</li>
          </ul>
          <button onclick="showContactSupport()" class="contact-support-btn">
            Contact Support
          </button>
        </div>
      `;
    }

    // Show/hide contact support based on results
    contactSupport.style.display = results.length === 0 ? 'block' : 'none';
  }
}

// Calculate relevance score using Levenshtein distance and token matching
function calculateRelevance(query, text) {
  const queryTokens = query.toLowerCase().split(/\s+/);
  const textTokens = text.toLowerCase().split(/\s+/);
  
  let totalScore = 0;
  queryTokens.forEach(queryToken => {
    let bestTokenScore = 0;
    textTokens.forEach(textToken => {
      const distance = levenshteinDistance(queryToken, textToken);
      const maxLength = Math.max(queryToken.length, textToken.length);
      const similarity = 1 - (distance / maxLength);
      bestTokenScore = Math.max(bestTokenScore, similarity);
    });
    totalScore += bestTokenScore;
  });
  
  return totalScore / queryTokens.length;
}

// Levenshtein distance calculation for fuzzy matching
function levenshteinDistance(str1, str2) {
  const matrix = Array(str2.length + 1).fill().map(() => Array(str1.length + 1).fill(0));
  
  for (let i = 0; i <= str1.length; i++) matrix[0][i] = i;
  for (let j = 0; j <= str2.length; j++) matrix[j][0] = j;
  
  for (let j = 1; j <= str2.length; j++) {
    for (let i = 1; i <= str1.length; i++) {
      const cost = str1[i - 1] === str2[j - 1] ? 0 : 1;
      matrix[j][i] = Math.min(
        matrix[j - 1][i] + 1,
        matrix[j][i - 1] + 1,
        matrix[j - 1][i - 1] + cost
      );
    }
  }
  
  return matrix[str2.length][str1.length];
}

// Highlight matching text in results
function highlightMatches(text, query) {
  const queryTokens = query.toLowerCase().split(/\s+/);
  let highlightedText = text;
  
  queryTokens.forEach(token => {
    const regex = new RegExp(`(${token})`, 'gi');
    highlightedText = highlightedText.replace(regex, '<mark>$1</mark>');
  });
  
  return highlightedText;
}

// Debounce search input to improve performance
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

function initializeChatSearch(detailName) {
  const chatSearchInput = document.getElementById('chatSearchInput');
  if (chatSearchInput) {
    const debouncedSearch = debounce((e) => {
      const query = e.target.value.trim();
      searchFAQs(query, detailName);
    }, 300);
    
    chatSearchInput.addEventListener('input', debouncedSearch);
    
    // Add keyboard navigation
    chatSearchInput.addEventListener('keydown', (e) => {
      if (e.key === 'Escape') {
        clearSearchResults();
        chatSearchInput.value = '';
      }
    });
  }
}

// Additional styles for enhanced features
const additionalStyles = `
  .search-summary {
    color: var(--text-light);
    padding: 10px 0;
    margin-bottom: 15px;
    font-size: 0.9em;
  }

  .search-result-item {
    position: relative;
    overflow: hidden;
  }

  .search-result-item mark {
    background: rgba(255, 215, 0, 0.2);
    color: var(--primary-color);
    padding: 0 2px;
    border-radius: 2px;
  }

  .relevance-indicator {
    position: absolute;
    bottom: 0;
    left: 0;
    height: 2px;
    background: var(--primary-color);
    opacity: 0.5;
    transition: width 0.3s ease;
  }

  .no-results {
    text-align: center;
    padding: 40px 20px;
    color: var(--text-light);
  }

  .no-results i {
    font-size: 3em;
    color: var(--primary-color);
    margin-bottom: 20px;
  }

  .no-results ul {
    list-style: none;
    padding: 0;
    margin: 20px 0;
  }

  .no-results li {
    margin: 10px 0;
    color: #aaa;
  }

  .contact-support-btn {
    background: var(--primary-color);
    color: #1a1a1f;
    border: none;
    padding: 12px 24px;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 500;
    transition: all 0.3s ease;
  }

  .contact-support-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(255, 215, 0, 0.2);
  }

  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }

  .search-result-item {
    animation: fadeIn 0.3s ease-out forwards;
    animation-delay: calc(var(--index) * 0.05s);
  }
`;

// Helper function to hide current overlay
function hideCurrentOverlay() {
  switch (overlayState.currentOverlay) {
    case 'locators':
      hideLocatorsOverlay();
      break;
    case 'providers':
      hideProvidersOverlay();
      break;
    case 'places':
      hidePlacesOverlay();
      break;
    case 'platforms':
      hidePlatformsOverlay();
      break;
  }
}

// Initialize the system when the DOM loads
document.addEventListener('DOMContentLoaded', initializeOverlaySystem);

const deepChatServices = {
  contacts: locatorsData.map(brand => ({
    id: `${brand.details.name}-${brand.metropolis}`,
    name: brand.details.name,
    avatar: brand.details.image,
    subtitle: `${brand.category} • ${brand.metropolis}`,
    lastMessage: brand.details.description,
    timestamp: this.randomTime(),
    unread: Math.floor(Math.random() * 5),
    online: true,
    verification: 'verified',
    metadata: brand.details
  })),
  recent: [],
  getChatHistory: (contactId) => {
    const brand = locatorsData.find(b => `${b.details.name}-${b.metropolis}` === contactId);
    return [
      {
        type: 'system',
        message: `You're now chatting with ${brand.details.name}`,
        timestamp: new Date().toISOString()
      },
      {
        type: 'brand',
        message: brand.details.description,
        timestamp: brand.details.support.phone
      }
    ];
  }
};

// WhatsApp-like UI Components
function createChatInterface() {
  const chatContainer = document.createElement('div');
  chatContainer.className = 'deep-chat-container';
  
  chatContainer.innerHTML = `
    <div class="chat-list">
      ${deepChatServices.contacts.map(contact => `
        <div class="chat-item" onclick="openChatModal('${contact.id}')">
          <div class="avatar-container">
            <img src="${contact.avatar}" class="avatar" />
            ${contact.online ? '<div class="online-indicator"></div>' : ''}
          </div>
          <div class="chat-info">
            <div class="chat-header">
              <h3>${contact.name}</h3>
              <span class="timestamp">${contact.timestamp}</span>
            </div>
            <div class="chat-preview">
              <p>${contact.lastMessage}</p>
              ${contact.unread > 0 ? 
                `<span class="unread-badge">${contact.unread}</span>` : ''}
            </div>
            <span class="category-tag">${contact.subtitle}</span>
          </div>
        </div>
      `).join('')}
    </div>
  `;

  const style = document.createElement('style');
  style.textContent = `
    .deep-chat-container {
      width: 100%;
      height: 100vh;
      background: #111;
      color: white;
      font-family: 'Helvetica Neue', sans-serif;
    }

    .chat-list {
      padding: 20px;
    }

    .chat-item {
      display: flex;
      align-items: center;
      padding: 15px;
      border-bottom: 1px solid #2d2d2d;
      cursor: pointer;
      transition: background 0.3s;
    }

    .chat-item:hover {
      background: rgba(255,255,255,0.05);
    }

    .avatar-container {
      position: relative;
      margin-right: 15px;
    }

    .avatar {
      width: 55px;
      height: 55px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid #ffd700;
    }

    .online-indicator {
      position: absolute;
      bottom: 2px;
      right: 2px;
      width: 12px;
      height: 12px;
      background: #00af4c;
      border-radius: 50%;
      border: 2px solid #111;
    }

    .chat-info {
      flex: 1;
    }

    .chat-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 5px;
    }

    .chat-header h3 {
      margin: 0;
      font-size: 1.1em;
      font-weight: 500;
    }

    .timestamp {
      font-size: 0.8em;
      color: #888;
    }

    .chat-preview {
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .chat-preview p {
      margin: 0;
      font-size: 0.9em;
      color: #888;
      max-width: 70%;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }

    .unread-badge {
      background: #00af4c;
      color: white;
      padding: 3px 8px;
      border-radius: 20px;
      font-size: 0.8em;
    }

    .category-tag {
      display: block;
      font-size: 0.75em;
      color: #ffd700;
      margin-top: 5px;
    }

    /* Chat Modal */
    .chat-modal {
      position: fixed;
      top: 0;
      right: 0;
      bottom: 0;
      width: 400px;
      background: #0c0c0c;
      border-left: 1px solid #2d2d2d;
      transform: translateX(100%);
      transition: transform 0.3s;
    }

    .chat-modal.active {
      transform: translateX(0);
    }

    .chat-header {
      padding: 20px;
      background: #1a1a1a;
      display: flex;
      align-items: center;
    }

    .chat-body {
      padding: 20px;
      height: calc(100vh - 160px);
      overflow-y: auto;
    }

    .message-input {
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 20px;
      background: #1a1a1a;
      display: flex;
      gap: 10px;
    }

    .message-input input {
      flex: 1;
      background: #2d2d2d;
      border: none;
      padding: 12px;
      color: white;
      border-radius: 25px;
    }
  `;

  document.body.appendChild(chatContainer);
  document.head.appendChild(style);
}

function openChatModal(contactId) {
  const brand = deepChatServices.contacts.find(c => c.id === contactId);
  const modal = document.createElement('div');
  modal.className = 'chat-modal active';
  
  modal.innerHTML = `
    <div class="chat-header">
      <img src="${brand.avatar}" class="avatar" />
      <div>
        <h2>${brand.name}</h2>
        <p>${brand.subtitle}</p>
      </div>
    </div>
    
    <div class="chat-body">
      <div class="chat-section">
        <h3>About</h3>
        <p>${brand.metadata.description}</p>
      </div>

      <div class="chat-section">
        <h3>Menu/Services</h3>
        ${brand.metadata.menu?.map(item => `
          <div class="menu-item">
            <img src="${item.image}" />
            <div>
              <h4>${item.name}</h4>
              <p>${item.price}</p>
            </div>
          </div>
        `).join('')}
      </div>

      <div class="chat-section">
        <h3>Promotions</h3>
        ${brand.metadata.promos?.map(promo => `
          <div class="promo-item">
            <p><strong>${promo.code}</strong> - ${promo.description}</p>
          </div>
        `).join('')}
      </div>

      <div class="chat-section">
        <h3>Outlets</h3>
        ${brand.metadata.outlets?.map(outlet => `
          <div class="outlet-item">
            <p>${outlet.name}</p>
            <p>${outlet.address}</p>
            <a href="${outlet.mapLink}" target="_blank">View on Map</a>
          </div>
        `).join('')}
      </div>

      <div class="chat-section">
        <h3>Support</h3>
        <p>Email: ${brand.metadata.support.email}</p>
        <p>Phone: ${brand.metadata.support.phone}</p>
        <button class="support-button">
          <i class="fas fa-comments"></i> Start Chat
        </button>
      </div>
    </div>

    <div class="message-input">
      <input type="text" placeholder="Type your message..." />
      <button class="send-button">
        <i class="fas fa-paper-plane"></i>
      </button>
    </div>
  `;

  document.body.appendChild(modal);
}

// Initialize the chat interface
createChatInterface();

function submitBookingViaWhatsApp() { const name = document.getElementById('nameInput').value.trim(); const contact = document.getElementById('contactInput').value.trim(); const address = document.getElementById('addressInput').value.trim(); const pincode = document.getElementById('pincodeInput').value.trim(); const date = document.getElementById('dateInput').value.trim(); const time = document.getElementById('timeInput').value.trim(); const bookingForm = document.getElementById('bookingForm'); const categoryId = bookingForm.dataset.categoryId || 'Not Specified'; const serviceName = bookingForm.dataset.serviceName || 'Not Specified'; const packageType = bookingForm.dataset.packageType || 'Not Specified'; if (!name || !contact) { showAlert('Please fill in Name and Contact Number', 'error'); return; } const message = `New Booking Request:\n📋 Category: ${categoryId}\n🛠 Service: ${serviceName}\n💎 Package: ${packageType}\n👤 Name: ${name}\n📞 Contact: ${contact}\n🏠 Address: ${address || 'Not Provided'}\n📍 Pincode: ${pincode || 'Not Provided'}\n📅 Date: ${date}\n⏰ Time: ${time}\n\nPlease confirm and process this booking.`; const encodedMessage = encodeURIComponent(message); const whatsappNumber = '78698 09022'; const whatsappUrl = `https://wa.me/${whatsappNumber.replace(/\s/g, '')}?text=${encodedMessage}`; window.open(whatsappUrl, '_blank'); showBookingConfirmation(); } function showBookingConfirmation() { const confirmationModal = document.createElement('div'); confirmationModal.id = 'bookingConfirmation'; confirmationModal.className = 'modal-overlay'; confirmationModal.innerHTML = `<div class="modal-content"><div class="confirmation-icon"><i class="fas fa-check-circle"></i></div><h2>Booking Submitted!</h2><p>Your booking request has been sent to WhatsApp. Our team will contact you shortly.</p><button onclick="closeConfirmation()" class="action-button">Close</button></div>`; document.body.appendChild(confirmationModal); document.getElementById('bookingForm').reset(); } function closeConfirmation() { const confirmationModal = document.getElementById('bookingConfirmation'); if (confirmationModal) { confirmationModal.remove(); } } document.addEventListener('DOMContentLoaded', function() { const submitBookingButton = document.getElementById('submitBooking'); if (submitBookingButton) { submitBookingButton.addEventListener('click', submitBookingViaWhatsApp); } }); document.addEventListener('DOMContentLoaded', function() { const locationNav = document.querySelector('.nav-item[data-page="location"]'); if (locationNav) { locationNav.addEventListener('click', function(e) { e.preventDefault(); showCitiesOverlay(); }); } }); function showAlert(message, type = 'success') { const alert = document.createElement('div'); alert.className = `alert alert-${type}`; alert.textContent = message; document.body.appendChild(alert); setTimeout(() => { alert.remove(); }, 3000); } function setupEventListeners() { window.addEventListener('click', function(event) { if (event.target.classList.contains('modal-overlay')) { closePackageInfo(); closeBookingForm(); closeConfirmation(); } }); }
                            </script>
                            
