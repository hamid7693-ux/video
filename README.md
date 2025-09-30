<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text to Video Generator</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #1e293b, #0f172a);
            color: #e2e8f0;
        }
        .gradient-text {
            background: linear-gradient(90deg, #60a5fa, #8b5cf6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        .glass-effect {
            backdrop-filter: blur(12px);
            background: rgba(30, 41, 59, 0.4);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 1rem;
        }
        .progress-bar {
            background: linear-gradient(90deg, #60a5fa, #8b5cf6);
            height: 4px;
            border-radius: 2px;
        }
        .btn-primary {
            background: linear-gradient(90deg, #60a5fa, #8b5cf6);
            color: white;
            border: none;
            border-radius: 0.5rem;
            padding: 0.75rem 1.5rem;
            font-weight: 600;
            transition: all 0.3s ease;
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 25px rgba(96, 165, 250, 0.3);
        }
        .card-hover {
            transition: all 0.3s ease;
        }
        .card-hover:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
        }
        .feature-icon {
            width: 48px;
            height: 48px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 12px;
            margin-bottom: 1rem;
        }
    </style>
</head>
<body class="min-h-screen">
    <div id="root"></div>
    <script type="text/babel">
        const App = () => {
            const [text, setText] = React.useState('');
            const [duration, setDuration] = React.useState(15);
            const [aspectRatio, setAspectRatio] = React.useState('16:9');
            const [voice, setVoice] = React.useState('male');
            const [music, setMusic] = React.useState('ambient');
            const [isGenerating, setIsGenerating] = React.useState(false);
            const [progress, setProgress] = React.useState(0);
            const [generatedVideo, setGeneratedVideo] = React.useState(null);
            const [showTutorial, setShowTutorial] = React.useState(true);

            const aspectRatios = [
                { value: '16:9', label: 'Landscape (16:9)', width: 1920, height: 1080 },
                { value: '9:16', label: 'Portrait (9:16)', width: 1080, height: 1920 },
                { value: '1:1', label: 'Square (1:1)', width: 1080, height: 1080 },
                { value: '4:3', label: 'Classic (4:3)', width: 1440, height: 1080 },
                { value: '21:9', label: 'Cinematic (21:9)', width: 2560, height: 1080 }
            ];

            const voices = [
                { value: 'male', label: 'Male Voice', description: 'Deep and clear male voice' },
                { value: 'female', label: 'Female Voice', description: 'Warm and engaging female voice' },
                { value: 'robot', label: 'AI Voice', description: 'Futuristic robotic voice' },
                { value: 'narrator', label: 'Narrator', description: 'Professional documentary voice' }
            ];

            const musicTracks = [
                { value: 'ambient', label: 'Ambient', description: 'Calm background music' },
                { value: 'upbeat', label: 'Upbeat', description: 'Energetic and positive' },
                { value: 'corporate', label: 'Corporate', description: 'Professional business music' },
                { value: 'cinematic', label: 'Cinematic', description: 'Dramatic and epic' },
                { value: 'minimal', label: 'Minimal', description: 'Simple and clean' }
            ];

            const features = [
                { 
                    icon: '🎥', 
                    title: '4K Quality', 
                    description: 'Generate stunning 4K resolution videos with crisp details and vibrant colors' 
                },
                { 
                    icon: '🎙️', 
                    title: 'Voice Over', 
                    description: 'Professional voice synthesis with multiple voice options and natural speech patterns' 
                },
                { 
                    icon: '🎵', 
                    title: 'Background Music', 
                    description: 'Royalty-free music tracks that perfectly match your video\'s mood and pacing' 
                },
                { 
                    icon: '⏱️', 
                    title: 'Custom Duration', 
                    description: 'Create videos from 5 to 60 seconds with precise timing control' 
                },
                { 
                    icon: '📱', 
                    title: 'Multiple Ratios', 
                    description: 'Support for all popular aspect ratios including social media formats' 
                },
                { 
                    icon: '⚡', 
                    title: 'Fast Processing', 
                    description: 'Quick generation with optimized rendering pipelines' 
                }
            ];

            const steps = [
                { number: 1, title: 'Enter Your Text', description: 'Write the script you want to convert to video' },
                { number: 2, title: 'Configure Settings', description: 'Choose duration, aspect ratio, voice, and music' },
                { number: 3, title: 'Generate Video', description: 'Click generate and watch your video come to life' },
                { number: 4, title: 'Download & Share', description: 'Save your 4K video and share it anywhere' }
            ];

            const handleGenerate = () => {
                if (!text.trim()) return;
                
                setIsGenerating(true);
                setProgress(0);
                setGeneratedVideo(null);
                
                // Simulate generation process
                const interval = setInterval(() => {
                    setProgress(prev => {
                        if (prev >= 100) {
                            clearInterval(interval);
                            setIsGenerating(false);
                            setGeneratedVideo({
                                url: 'https://placehold.co/1920x1080/60a5fa/ffffff?text=Generated+Video+Preview',
                                duration: duration,
                                resolution: '3840×2160',
                                size: '15.2 MB'
                            });
                            return 100;
                        }
                        return prev + 2;
                    });
                }, 100);
            };

            const selectedRatio = aspectRatios.find(r => r.value === aspectRatio);

            return (
                <div className="min-h-screen text-gray-200">
                    {/* Header */}
                    <header className="py-6 px-4 sm:px-6 lg:px-8">
                        <div className="max-w-7xl mx-auto flex justify-between items-center">
                            <div className="flex items-center space-x-2">
                                <div className="w-10 h-10 bg-gradient-to-r from-blue-500 to-purple-600 rounded-xl flex items-center justify-center">
                                    <span className="text-white font-bold text-lg">T</span>
                                </div>
                                <h1 className="text-2xl font-bold gradient-text">Text2Video Pro</h1>
                            </div>
                            <nav className="hidden md:flex space-x-8">
                                <a href="#" className="text-gray-300 hover:text-white transition">Home</a>
                                <a href="#" className="text-gray-300 hover:text-white transition">Features</a>
                                <a href="#" className="text-gray-300 hover:text-white transition">Pricing</a>
                                <a href="#" className="text-gray-300 hover:text-white transition">Support</a>
                            </nav>
                        </div>
                    </header>

                    {/* Hero Section */}
                    <section className="py-12 px-4 sm:px-6 lg:px-8">
                        <div className="max-w-7xl mx-auto">
                            <div className="text-center mb-12">
                                <h2 className="text-4xl md:text-6xl font-bold mb-6">
                                    <span className="gradient-text">Transform Text</span> to Stunning Videos
                                </h2>
                                <p className="text-xl text-gray-300 mb-8 max-w-3xl mx-auto">
                                    Create professional 4K videos from text in seconds. Add voice overs and background music 
                                    with our AI-powered video generation technology.
                                </p>
                                <button 
                                    onClick={() => setShowTutorial(!showTutorial)}
                                    className="btn-primary text-lg"
                                >
                                    {showTutorial ? 'Hide Tutorial' : 'Show Tutorial'}
                                </button>
                            </div>

                            {/* Tutorial Steps */}
                            {showTutorial && (
                                <div className="mb-12 glass-effect p-8 rounded-2xl">
                                    <h3 className="text-2xl font-bold mb-6 text-center">How It Works</h3>
                                    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                                        {steps.map((step, index) => (
                                            <div key={index} className="text-center p-6 rounded-xl bg-gray-800/50">
                                                <div className="w-12 h-12 bg-gradient-to-r from-blue-500 to-purple-600 rounded-full flex items-center justify-center text-white font-bold text-lg mx-auto mb-4">
                                                    {step.number}
                                                </div>
                                                <h4 className="font-bold mb-2">{step.title}</h4>
                                                <p className="text-gray-400 text-sm">{step.description}</p>
                                            </div>
                                        ))}
                                    </div>
                                </div>
                            )}

                            <div className="grid grid-cols-1 lg:grid-cols-2 gap-8">
                                {/* Input Panel */}
                                <div className="glass-effect p-8 rounded-2xl">
                                    <h3 className="text-2xl font-bold mb-6">Create Your Video</h3>
                                    
                                    <div className="space-y-6">
                                        <div>
                                            <label className="block text-sm font-medium mb-2">Your Text</label>
                                            <textarea
                                                value={text}
                                                onChange={(e) => setText(e.target.value)}
                                                placeholder="Enter the text you want to convert to video..."
                                                className="w-full h-32 p-4 bg-gray-800/50 border border-gray-600 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent resize-none text-white placeholder-gray-400"
                                            />
                                            <p className="text-sm text-gray-400 mt-2">
                                                {text.length}/500 characters
                                            </p>
                                        </div>

                                        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                                            <div>
                                                <label className="block text-sm font-medium mb-2">Duration ({duration}s)</label>
                                                <input
                                                    type="range"
                                                    min="5"
                                                    max="60"
                                                    value={duration}
                                                    onChange={(e) => setDuration(parseInt(e.target.value))}
                                                    className="w-full h-2 bg-gray-700 rounded-lg appearance-none cursor-pointer slider"
                                                />
                                                <div className="flex justify-between text-xs text-gray-400 mt-1">
                                                    <span>5s</span>
                                                    <span>60s</span>
                                                </div>
                                            </div>

                                            <div>
                                                <label className="block text-sm font-medium mb-2">Aspect Ratio</label>
                                                <select
                                                    value={aspectRatio}
                                                    onChange={(e) => setAspectRatio(e.target.value)}
                                                    className="w-full p-3 bg-gray-800/50 border border-gray-600 rounded-xl focus:ring-2 focus:ring-blue-500 focus:border-transparent text-white"
                                                >
                                                    {aspectRatios.map(ratio => (
                                                        <option key={ratio.value} value={ratio.value}>
                                                            {ratio.label}
                                                        </option>
                                                    ))}
                                                </select>
                                            </div>
                                        </div>

                                        <div>
                                            <label className="block text-sm font-medium mb-2">Voice Over</label>
                                            <div className="grid grid-cols-2 gap-3">
                                                {voices.map(v => (
                                                    <label key={v.value} className="flex items-center p-3 bg-gray-800/50 rounded-xl cursor-pointer hover:bg-gray-700/50 transition">
                                                        <input
                                                            type="radio"
                                                            name="voice"
                                                            value={v.value}
                                                            checked={voice === v.value}
                                                            onChange={(e) => setVoice(e.target.value)}
                                                            className="sr-only"
                                                        />
                                                        <div className="flex-1">
                                                            <div className="font-medium">{v.label}</div>
                                                            <div className="text-xs text-gray-400">{v.description}</div>
                                                        </div>
                                                        {voice === v.value && (
                                                            <div className="w-5 h-5 bg-blue-500 rounded-full flex items-center justify-center">
                                                                <div className="w-2 h-2 bg-white rounded-full"></div>
                                                            </div>
                                                        )}
                                                    </label>
                                                ))}
                                            </div>
                                        </div>

                                        <div>
                                            <label className="block text-sm font-medium mb-2">Background Music</label>
                                            <div className="space-y-2">
                                                {musicTracks.map(track => (
                                                    <label key={track.value} className="flex items-center p-3 bg-gray-800/50 rounded-xl cursor-pointer hover:bg-gray-700/50 transition">
                                                        <input
                                                            type="radio"
                                                            name="music"
                                                            value={track.value}
                                                            checked={music === track.value}
                                                            onChange={(e) => setMusic(e.target.value)}
                                                            className="sr-only"
                                                        />
                                                        <div className="flex-1">
                                                            <div className="font-medium">{track.label}</div>
                                                            <div className="text-xs text-gray-400">{track.description}</div>
                                                        </div>
                                                        {music === track.value && (
                                                            <div className="w-5 h-5 bg-blue-500 rounded-full flex items-center justify-center">
                                                                <div className="w-2 h-2 bg-white rounded-full"></div>
                                                            </div>
                                                        )}
                                                    </label>
                                                ))}
                                            </div>
                                        </div>

                                        <button
                                            onClick={handleGenerate}
                                            disabled={!text.trim() || isGenerating}
                                            className="btn-primary w-full text-lg py-4 font-bold"
                                        >
                                            {isGenerating ? 'Generating...' : 'Generate Video'}
                                        </button>
                                    </div>
                                </div>

                                {/* Preview Panel */}
                                <div className="glass-effect p-8 rounded-2xl">
                                    <h3 className="text-2xl font-bold mb-6">Video Preview</h3>
                                    
                                    {isGenerating ? (
                                        <div className="space-y-6">
                                            <div className="bg-gray-800/50 rounded-xl p-6 text-center">
                                                <div className="w-16 h-16 border-4 border-blue-500 border-t-transparent rounded-full animate-spin mx-auto mb-4"></div>
                                                <h4 className="font-bold text-lg mb-2">Creating Your Video</h4>
                                                <p className="text-gray-400 mb-4">Processing your text and generating voice over...</p>
                                                <div className="w-full bg-gray-700 rounded-full h-2 mb-2">
                                                    <div 
                                                        className="progress-bar rounded-full h-2" 
                                                        style={{ width: `${progress}%` }}
                                                    ></div>
                                                </div>
                                                <p className="text-sm text-gray-400">{Math.round(progress)}% Complete</p>
                                            </div>
                                            
                                            <div className="grid grid-cols-3 gap-4 text-center">
                                                <div className="p-4 bg-gray-800/50 rounded-xl">
                                                    <div className="text-2xl font-bold text-blue-400">{duration}s</div>
                                                    <div className="text-xs text-gray-400">Duration</div>
                                                </div>
                                                <div className="p-4 bg-gray-800/50 rounded-xl">
                                                    <div className="text-2xl font-bold text-purple-400">{aspectRatio}</div>
                                                    <div className="text-xs text-gray-400">Ratio</div>
                                                </div>
                                                <div className="p-4 bg-gray-800/50 rounded-xl">
                                                    <div className="text-2xl font-bold text-green-400">4K</div>
                                                    <div className="text-xs text-gray-400">Quality</div>
                                                </div>
                                            </div>
                                        </div>
                                    ) : generatedVideo ? (
                                        <div className="space-y-6">
                                            <div className="bg-gray-800/50 rounded-xl overflow-hidden">
                                                <img 
                                                    src={generatedVideo.url} 
                                                    alt="Generated video preview"
                                                    className="w-full h-64 object-cover"
                                                />
                                                <div className="p-4">
                                                    <div className="flex justify-between items-center mb-2">
                                                        <span className="font-medium">Generated Video</span>
                                                        <span className="text-sm text-gray-400">{generatedVideo.duration}s</span>
                                                    </div>
                                                    <div className="text-sm text-gray-400 space-y-1">
                                                        <div>Resolution: {generatedVideo.resolution}</div>
                                                        <div>Size: {generatedVideo.size}</div>
                                                    </div>
                                                </div>
                                            </div>
                                            
                                            <div className="flex space-x-4">
                                                <button className="flex-1 btn-primary py-3 font-medium">
                                                    Download MP4
                                                </button>
                                                <button className="flex-1 bg-gray-700 hover:bg-gray-600 text-white py-3 px-6 rounded-xl font-medium transition">
                                                    Share
                                                </button>
                                            </div>
                                        </div>
                                    ) : (
                                        <div className="bg-gray-800/50 rounded-xl p-12 text-center">
                                            <div className="text-6xl mb-4">🎥</div>
                                            <h4 className="font-bold text-lg mb-2">No Video Generated Yet</h4>
                                            <p className="text-gray-400">Enter your text and click "Generate Video" to create your first video.</p>
                                        </div>
                                    )}
                                </div>
                            </div>
                        </div>
                    </section>

                    {/* Features Section */}
                    <section className="py-16 px-4 sm:px-6 lg:px-8">
                        <div className="max-w-7xl mx-auto">
                            <div className="text-center mb-12">
                                <h2 className="text-3xl md:text-4xl font-bold mb-4">Powerful Features</h2>
                                <p className="text-xl text-gray-300">Everything you need to create professional videos from text</p>
                            </div>
                            
                            <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                                {features.map((feature, index) => (
                                    <div key={index} className="card-hover glass-effect p-8 rounded-2xl text-center">
                                        <div className="feature-icon bg-gradient-to-r from-blue-500 to-purple-600 text-white mx-auto">
                                            <span className="text-2xl">{feature.icon}</span>
                                        </div>
                                        <h3 className="text-xl font-bold mb-3">{feature.title}</h3>
                                        <p className="text-gray-400">{feature.description}</p>
                                    </div>
                                ))}
                            </div>
                        </div>
                    </section>

                    {/* Requirements Section */}
                    <section className="py-16 px-4 sm:px-6 lg:px-8">
                        <div className="max-w-7xl mx-auto">
                            <div className="glass-effect rounded-2xl p-8 md:p-12">
                                <h2 className="text-3xl md:text-4xl font-bold mb-8 text-center">Technical Requirements</h2>
                                
                                <div className="grid grid-cols-1 lg:grid-cols-2 gap-12">
                                    <div>
                                        <h3 className="text-2xl font-bold mb-6 text-blue-400">Software Requirements</h3>
                                        <ul className="space-y-4">
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-blue-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>Text-to-Speech Engine:</strong> High-quality voice synthesis with multiple voice options
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-blue-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>Video Rendering Engine:</strong> 4K video processing with multiple aspect ratios
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-blue-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>Audio Processing:</strong> Background music integration with volume control
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-blue-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>Animation System:</strong> Text animations and visual effects
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-blue-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>Export Options:</strong> Multiple formats including MP4, MOV, and GIF
                                                </div>
                                            </li>
                                        </ul>
                                    </div>
                                    
                                    <div>
                                        <h3 className="text-2xl font-bold mb-6 text-purple-400">System Requirements</h3>
                                        <ul className="space-y-4">
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-purple-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>Processor:</strong> Multi-core CPU (Intel i7 or equivalent)
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-purple-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>RAM:</strong> 16GB minimum, 32GB recommended for 4K rendering
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-purple-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>GPU:</strong> NVIDIA RTX series or AMD equivalent for hardware acceleration
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-purple-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>Storage:</strong> SSD with at least 100GB free space
                                                </div>
                                            </li>
                                            <li className="flex items-start">
                                                <span className="w-2 h-2 bg-purple-500 rounded-full mt-3 mr-3 flex-shrink-0"></span>
                                                <div>
                                                    <strong>OS:</strong> Windows 10/11, macOS 10.15+, or Linux (Ubuntu 20.04+)
                                                </div>
                                            </li>
                                        </ul>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </section>

                    {/* Footer */}
                    <footer className="py-12 px-4 sm:px-6 lg:px-8 border-t border-gray-800">
                        <div className="max-w-7xl mx-auto">
                            <div className="text-center">
                                <div className="flex items-center justify-center space-x-2 mb-4">
                                    <div className="w-8 h-8 bg-gradient-to-r from-blue-500 to-purple-600 rounded-lg flex items-center justify-center">
                                        <span className="text-white font-bold text-sm">T</span>
                                    </div>
                                    <span className="text-xl font-bold gradient-text">Text2Video Pro</span>
                                </div>
                                <p className="text-gray-400 mb-6">
                                    Transform your ideas into stunning videos with AI-powered technology.
                                </p>
                                <div className="flex justify-center space-x-6 text-sm text-gray-500">
                                    <a href="#" className="hover:text-gray-300 transition">Privacy Policy</a>
                                    <a href="#" className="hover:text-gray-300 transition">Terms of Service</a>
                                    <a href="#" className="hover:text-gray-300 transition">Contact Us</a>
                                </div>
                            </div>
                        </div>
                    </footer>
                </div>
            );
        };

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(<App />);
    </script>
</body>
</html>
