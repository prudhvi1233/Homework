# Homework
Homework portal

import React, { useState } from 'react';
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';
import { motion, AnimatePresence } from 'framer-motion';
import { AuthProvider, useAuth } from './context/AuthContext';
import { ToastProvider } from './context/ToastContext';
import { ThemeProvider } from './context/ThemeContext';
import Login from './pages/Login';
import Register from './pages/Register';
import Dashboard from './pages/Dashboard';
import Landing from './pages/Landing';
import ChatBot from './components/ChatBot';

import './index.css';

const PrivateRoute = ({ children }) => {
  const { user } = useAuth();
  return user ? children : <Navigate to="/login" />;
};

const pageVariants = {
  initial: {
    opacity: 0,
    y: 20,
  },
  in: {
    opacity: 1,
    y: 0,
    transition: {
      duration: 0.3,
      ease: "easeOut",
    },
  },
  out: {
    opacity: 0,
    y: -20,
    transition: {
      duration: 0.2,
      ease: "easeIn",
    },
  },
};

function App() {
  const [isChatBotOpen, setIsChatBotOpen] = useState(false);

  return (
    <ThemeProvider>
      <ToastProvider>
        <AuthProvider>
          <Router>
            <div className="app-container">
              <AnimatePresence mode="wait">
                <Routes>
                  <Route path="/" element={
                    <motion.div
                      variants={pageVariants}
                      initial="initial"
                      animate="in"
                      exit="out"
                      className="page-wrapper"
                    >
                      <Landing />
                    </motion.div>
                  } />
                  <Route path="/login" element={
                    <motion.div
                      variants={pageVariants}
                      initial="initial"
                      animate="in"
                      exit="out"
                      className="page-wrapper"
                    >
                      <Login />
                    </motion.div>
                  } />
                  <Route path="/register" element={
                    <motion.div
                      variants={pageVariants}
                      initial="initial"
                      animate="in"
                      exit="out"
                      className="page-wrapper"
                    >
                      <Register />
                    </motion.div>
                  } />
                  <Route
                    path="/dashboard/*"
                    element={
                      <PrivateRoute>
                        <motion.div
                          variants={pageVariants}
                          initial="initial"
                          animate="in"
                          exit="out"
                          className="page-wrapper"
                        >
                          <Dashboard />
                        </motion.div>
                      </PrivateRoute>
                    }
                  />
                </Routes>
              </AnimatePresence>
              <ChatBot 
                isOpen={isChatBotOpen} 
                onClose={() => setIsChatBotOpen(false)} 
              />
              <button 
                className="chatbot-toggle-btn"
                onClick={() => setIsChatBotOpen(!isChatBotOpen)}
              >
                💬
              </button>
            </div>
          </Router>
        </AuthProvider>
      </ToastProvider>
    </ThemeProvider>
  );
}

export default App;
