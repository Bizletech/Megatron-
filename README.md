import React, { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Camera, Video, Upload, User } from "lucide-react";
import { motion, AnimatePresence } from "framer-motion";

export default function MegatronApp() {
  const [uploadedMedia, setUploadedMedia] = useState([]);

  const handleUpload = (type) => {
    const newMedia = { type, id: Date.now() };
    setUploadedMedia((prev) => [...prev, newMedia]);
  };

  useEffect(() => {
    const interval = setInterval(() => {
      setUploadedMedia((prev) => prev.slice(-5)); // Keep latest 5 uploads
    }, 5000);
    return () => clearInterval(interval);
  }, []);

  return (
    <div className="bg-black min-h-screen text-white p-4 space-y-6">
      {/* Header */}
      <motion.header className="text-center text-neon-blue text-2xl font-bold" initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ duration: 1 }}>
        Megatron
      </motion.header>
      
      {/* Home Screen */}
      <Card className="bg-gray-900 text-center p-4 rounded-2xl shadow-lg">
        <CardContent>
          <h2 className="text-xl font-semibold text-neon-purple">Welcome to Megatron</h2>
          <p className="text-gray-400 mt-2">Upload your best moments and get featured live!</p>
          <Button className="mt-4 bg-neon-blue hover:bg-blue-600">Explore Events</Button>
        </CardContent>
      </Card>

      {/* Upload Screen */}
      <Card className="bg-gray-900 p-4 rounded-2xl shadow-lg text-center">
        <CardContent>
          <h2 className="text-xl font-semibold text-neon-purple">Upload Your Moment</h2>
          <div className="flex justify-center space-x-4 mt-4">
            <Button variant="outline" className="border-neon-blue text-neon-blue" onClick={() => handleUpload("selfie")}>
              <Camera className="w-5 h-5 mr-2" /> Selfie
            </Button>
            <Button variant="outline" className="border-neon-purple text-neon-purple" onClick={() => handleUpload("video")}>
              <Video className="w-5 h-5 mr-2" /> Video
            </Button>
          </div>
          <Button className="mt-4 bg-neon-blue hover:bg-blue-600 w-full">
            <Upload className="w-5 h-5 mr-2" /> Upload Now
          </Button>
        </CardContent>
      </Card>

      {/* Live Feed */}
      <Card className="bg-gray-900 p-4 rounded-2xl shadow-lg text-center">
        <CardContent>
          <h2 className="text-xl font-semibold text-neon-purple">Live Jumbotron Feed</h2>
          <p className="text-gray-400 mt-2">See what's trending now!</p>
          <div className="mt-4 h-40 bg-gray-800 rounded-xl flex items-center justify-center overflow-hidden">
            <AnimatePresence>
              {uploadedMedia.length > 0 ? (
                <motion.div className="flex flex-wrap gap-2 p-2" initial={{ opacity: 0, y: 10 }} animate={{ opacity: 1, y: 0 }} exit={{ opacity: 0, y: -10 }}>
                  {uploadedMedia.map((media) => (
                    <motion.div key={media.id} className="bg-gray-700 p-2 rounded-md text-sm" whileHover={{ scale: 1.1 }}>
                      {media.type === "selfie" ? "📸 Selfie" : "🎥 Video"}
                    </motion.div>
                  ))}
                </motion.div>
              ) : (
                <p className="text-gray-500">No uploads yet...</p>
              )}
            </AnimatePresence>
          </div>
        </CardContent>
      </Card>

      {/* Profile & Subscription */}
      <Card className="bg-gray-900 p-4 rounded-2xl shadow-lg text-center">
        <CardContent>
          <h2 className="text-xl font-semibold text-neon-purple">Your Profile</h2>
          <User className="w-12 h-12 text-neon-blue mx-auto mt-4" />
          <p className="text-gray-400 mt-2">Manage your uploads and subscriptions</p>
          <Button className="mt-4 bg-neon-purple hover:bg-purple-600">Upgrade to Premium</Button>
        </CardContent>
      </Card>
    </div>
  );
}
