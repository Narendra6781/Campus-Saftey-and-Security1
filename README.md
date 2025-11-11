# Campus-Saftey-and-Security1
{
  "name": "my-v0-project",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint ."
  },
  "dependencies": {
    "@hookform/resolvers": "^3.10.0",
    "@radix-ui/react-accordion": "1.2.2",
    "@radix-ui/react-alert-dialog": "1.1.4",
    "@radix-ui/react-aspect-ratio": "1.1.1",
    "@radix-ui/react-avatar": "1.1.2",
    "@radix-ui/react-checkbox": "1.1.3",
    "@radix-ui/react-collapsible": "1.1.2",
    "@radix-ui/react-context-menu": "2.2.4",
    "@radix-ui/react-dialog": "1.1.4",
    "@radix-ui/react-dropdown-menu": "2.1.4",
    "@radix-ui/react-hover-card": "1.1.4",
    "@radix-ui/react-label": "2.1.1",
    "@radix-ui/react-menubar": "1.1.4",
    "@radix-ui/react-navigation-menu": "1.2.3",
    "@radix-ui/react-popover": "1.1.4",
    "@radix-ui/react-progress": "1.1.1",
    "@radix-ui/react-radio-group": "1.2.2",
    "@radix-ui/react-scroll-area": "1.2.2",
    "@radix-ui/react-select": "2.1.4",
    "@radix-ui/react-separator": "1.1.1",
    "@radix-ui/react-slider": "1.2.2",
    "@radix-ui/react-slot": "1.1.1",
    "@radix-ui/react-switch": "1.1.2",
    "@radix-ui/react-tabs": "1.1.2",
    "@radix-ui/react-toast": "1.2.4",
    "@radix-ui/react-toggle": "1.1.1",
    "@radix-ui/react-toggle-group": "1.1.1",
    "@radix-ui/react-tooltip": "1.1.6",
    "@vercel/analytics": "1.3.1",
    "autoprefixer": "^10.4.20",
    "class-variance-authority": "^0.7.1",
    "clsx": "^2.1.1",
    "cmdk": "1.0.4",
    "date-fns": "4.1.0",
    "embla-carousel-react": "8.5.1",
    "input-otp": "1.4.1",
    "lucide-react": "^0.454.0",
    "next": "16.0.0",
    "next-themes": "^0.4.6",
    "react": "19.2.0",
    "react-day-picker": "9.8.0",
    "react-dom": "19.2.0",
    "react-hook-form": "^7.60.0",
    "react-resizable-panels": "^2.1.7",
    "recharts": "2.15.4",
    "sonner": "^1.7.4",
    "tailwind-merge": "^3.3.1",
    "tailwindcss-animate": "^1.0.7",
    "vaul": "^0.9.9",
    "zod": "3.25.76"
  },
  "devDependencies": {
    "@tailwindcss/postcss": "^4.1.9",
    "@types/node": "^22",
    "@types/react": "^19",
    "@types/react-dom": "^19",
    "postcss": "^8.5",
    "tailwindcss": "^4.1.9",
    "tw-animate-css": "1.3.3",
    "typescript": "^5"
  }
}

"use client"

import type React from "react"

import { useState, useRef } from "react"
import { Card } from "@/components/ui/card"
import { Button } from "@/components/ui/button"
import { Upload } from "lucide-react"

export default function VideoDetectionPanel() {
  const [videoLoaded, setVideoLoaded] = useState(false)
  const [detections, setDetections] = useState<
    Array<{ id: number; x: number; y: number; label: string; confidence: number }>
  >([])
  const [isAnalyzing, setIsAnalyzing] = useState(false)
  const fileInputRef = useRef<HTMLInputElement>(null)

  const handleVideoUpload = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0]
    if (file) {
      setVideoLoaded(true)
      setIsAnalyzing(true)

      // Simulate video analysis
      setTimeout(() => {
        setDetections([
          { id: 1, x: 30, y: 25, label: "Person", confidence: 0.98 },
          { id: 2, x: 55, y: 40, label: "Person", confidence: 0.96 },
          { id: 3, x: 70, y: 35, label: "Person", confidence: 0.97 },
          { id: 4, x: 45, y: 60, label: "Person", confidence: 0.95 },
        ])
        setIsAnalyzing(false)
      }, 2000)
    }
  }

  return (
    <div className="space-y-8">
      <div>
        <h2 className="text-2xl font-bold mb-6">Video Upload & Real-Time Detection</h2>

        <div className="grid md:grid-cols-3 gap-6">
          {/* Video Player Area */}
          <div className="md:col-span-2">
            <Card className="glow-accent-sm border-accent/20 bg-card/50 p-0 overflow-hidden">
              <div className="relative w-full aspect-video bg-black flex items-center justify-center">
                {!videoLoaded ? (
                  <div className="text-center">
                    <Upload className="w-12 h-12 mx-auto text-muted-foreground mb-4" />
                    <p className="text-muted-foreground mb-4">Upload CCTV footage to analyze</p>
                    <Button onClick={() => fileInputRef.current?.click()}>Choose Video File</Button>
                    <input
                      ref={fileInputRef}
                      type="file"
                      accept="video/*"
                      onChange={handleVideoUpload}
                      className="hidden"
                    />
                  </div>
                ) : (
                  <div className="relative w-full h-full">
                    {/* Video placeholder with detection boxes */}
                    <div className="w-full h-full bg-gradient-to-br from-slate-900 via-black to-slate-900 flex items-center justify-center relative">
                      <div className="text-center z-10">
                        <p className="text-sm text-muted-foreground">Sample CCTV Stream</p>
                      </div>

                      {/* Detection boxes */}
                      {detections.map((det) => (
                        <div
                          key={det.id}
                          className="absolute glow-accent-sm"
                          style={{
                            left: `${det.x}%`,
                            top: `${det.y}%`,
                            width: "120px",
                            height: "150px",
                            border: "2px solid rgba(139, 92, 246, 0.6)",
                            transform: "translate(-50%, -50%)",
                          }}
                        >
                          <div className="absolute -top-6 left-0 bg-accent text-accent-foreground px-2 py-1 rounded text-xs whitespace-nowrap font-mono">
                            {det.label} {(det.confidence * 100).toFixed(0)}%
                          </div>
                        </div>
                      ))}

                      {/* Scan line effect */}
                      {isAnalyzing && <div className="absolute inset-0 scan-line" />}
                    </div>
                  </div>
                )}
              </div>
            </Card>
          </div>

          {/* Detection Stats */}
          <Card className="glow-accent-sm border-accent/20 bg-card/50 p-6">
            <h3 className="font-semibold mb-4">Detection Results</h3>
            <div className="space-y-4">
              <div className="p-4 bg-accent/10 border border-accent/30 rounded">
                <div className="text-2xl font-bold text-accent">{detections.length}</div>
                <div className="text-sm text-muted-foreground">Persons Detected</div>
              </div>

              {detections.length > 0 && (
                <div className="bg-destructive/10 border border-destructive/30 rounded p-3">
                  <p className="text-sm font-semibold text-destructive mb-1">⚠️ Alert Triggered</p>
                  <p className="text-xs text-muted-foreground">
                    {detections.length > 2
                      ? "Group of 3+ individuals detected in restricted zone"
                      : "Individual detected"}
                  </p>
                </div>
              )}

              <div className="pt-4 border-t border-border">
                <div className="text-xs text-muted-foreground mb-3">Average Confidence</div>
                <div className="flex gap-2">
                  {detections.map((det) => (
                    <div
                      key={det.id}
                      className="flex-1 h-8 bg-accent/20 rounded flex items-center justify-center text-xs font-mono text-accent"
                      title={`Person ${det.id}: ${(det.confidence * 100).toFixed(0)}%`}
                    >
                      {(det.confidence * 100).toFixed(0)}%
                    </div>
                  ))}
                </div>
              </div>
            </div>
          </Card>
        </div>
      </div>
    </div>
  )
}
"use client"

import { useState, useEffect } from "react"
import { Card } from "@/components/ui/card"
import { ResponsiveContainer, Radar, RadarChart, PolarGrid, PolarAngleAxis, PolarRadiusAxis } from "recharts"

export default function SensorFusionDashboard() {
  const [sensorData] = useState([
    { name: "CCTV", value: 98, status: "active", color: "#8b5cf6" },
    { name: "Wi-Fi Density", value: 85, status: "active", color: "#6366f1" },
    { name: "Motion Sensors", value: 92, status: "active", color: "#a855f7" },
    { name: "Access Control", value: 78, status: "active", color: "#d946ef" },
  ])

  const [radarData] = useState([
    { sensor: "CCTV", confidence: 98 },
    { sensor: "Wi-Fi", confidence: 85 },
    { sensor: "Motion", confidence: 92 },
    { sensor: "Access", confidence: 78 },
    { sensor: "Acoustic", confidence: 65 },
  ])

  const [fusionScore, setFusionScore] = useState(87)

  useEffect(() => {
    const interval = setInterval(() => {
      setFusionScore((prev) => {
        const newScore = prev + (Math.random() - 0.5) * 4
        return Math.max(60, Math.min(99, newScore))
      })
    }, 2000)

    return () => clearInterval(interval)
  }, [])

  return (
    <div className="space-y-8">
      <div>
        <h2 className="text-2xl font-bold mb-6">Real-Time Sensor Fusion</h2>

        {/* Fusion Score */}
        <Card className="glow-accent p-8 border-accent/20 bg-gradient-to-br from-accent/10 to-card/50 mb-6">
          <div className="flex items-center justify-between">
            <div>
              <h3 className="text-lg text-muted-foreground mb-2">Overall Fusion Score</h3>
              <p className="text-5xl font-bold text-accent">{fusionScore.toFixed(0)}/100</p>
            </div>
            <div className="text-center">
              <div className="w-32 h-32 relative">
                <div className="absolute inset-0 rounded-full border-4 border-accent/20" />
                <div
                  className="absolute inset-0 rounded-full border-4 border-accent"
                  style={{
                    clipPath: `polygon(0 0, ${fusionScore}% 0, ${fusionScore}% 100%, 0 100%)`,
                  }}
                />
                <div className="absolute inset-2 rounded-full bg-background flex items-center justify-center">
                  <span className="text-lg font-bold text-accent">{Math.round(fusionScore)}%</span>
                </div>
              </div>
            </div>
          </div>
        </Card>

        <div className="grid md:grid-cols-2 gap-6">
          {/* Sensor Status */}
          <Card className="glow-accent-sm border-accent/20 bg-card/50 p-6">
            <h3 className="font-semibold text-lg mb-4">Active Sensors</h3>
            <div className="space-y-3">
              {sensorData.map((sensor, idx) => (
                <div key={idx} className="space-y-1">
                  <div className="flex items-center justify-between text-sm">
                    <span className="font-medium">{sensor.name}</span>
                    <span className="font-mono text-accent">{sensor.value}%</span>
                  </div>
                  <div className="w-full bg-card/50 rounded-full h-2 overflow-hidden border border-border">
                    <div className="h-full bg-accent/70 transition-all" style={{ width: `${sensor.value}%` }} />
                  </div>
                </div>
              ))}
            </div>
          </Card>

          {/* Confidence Radar */}
          <Card className="glow-accent-sm border-accent/20 bg-card/50 p-6">
            <h3 className="font-semibold text-lg mb-4">Multi-Sensor Confidence</h3>
            <ResponsiveContainer width="100%" height={250}>
              <RadarChart data={radarData}>
                <PolarGrid stroke="rgba(139, 92, 246, 0.2)" />
                <PolarAngleAxis dataKey="sensor" stroke="rgba(139, 92, 246, 0.5)" />
                <PolarRadiusAxis stroke="rgba(139, 92, 246, 0.3)" />
                <Radar name="Confidence" dataKey="confidence" stroke="#8b5cf6" fill="#8b5cf6" fillOpacity={0.3} />
              </RadarChart>
            </ResponsiveContainer>
          </Card>
        </div>

        {/* Fusion Details */}
        <Card className="glow-accent-sm border-accent/20 bg-card/50 p-6">
          <h3 className="font-semibold text-lg mb-4">Sensor Integration Details</h3>
          <div className="grid md:grid-cols-2 gap-6 text-sm">
            <div>
              <h4 className="font-semibold text-accent mb-2">CCTV Analysis</h4>
              <ul className="space-y-1 text-muted-foreground">
                <li>• 4 persons detected in frame</li>
                <li>• Group formation detected</li>
                <li>• Restricted zone alert: YES</li>
                <li>• Confidence: 98%</li>
              </ul>
            </div>
            <div>
              <h4 className="font-semibold text-accent mb-2">Wi-Fi & Motion Correlation</h4>
              <ul className="space-y-1 text-muted-foreground">
                <li>• Wi-Fi hotspots: 6 devices</li>
                <li>• Motion intensity: HIGH</li>
                <li>• Spike correlation: 0.92</li>
                <li>• Badge access: None recorded</li>
              </ul>
            </div>
          </div>
        </Card>
      </div>
    </div>
  )
}
"use client"

import { useState, useEffect } from "react"
import { Card } from "@/components/ui/card"
import { ResponsiveContainer, Radar, RadarChart, PolarGrid, PolarAngleAxis, PolarRadiusAxis } from "recharts"

export default function SensorFusionDashboard() {
  const [sensorData] = useState([
    { name: "CCTV", value: 98, status: "active", color: "#8b5cf6" },
    { name: "Wi-Fi Density", value: 85, status: "active", color: "#6366f1" },
    { name: "Motion Sensors", value: 92, status: "active", color: "#a855f7" },
    { name: "Access Control", value: 78, status: "active", color: "#d946ef" },
  ])

  const [radarData] = useState([
    { sensor: "CCTV", confidence: 98 },
    { sensor: "Wi-Fi", confidence: 85 },
    { sensor: "Motion", confidence: 92 },
    { sensor: "Access", confidence: 78 },
    { sensor: "Acoustic", confidence: 65 },
  ])

  const [fusionScore, setFusionScore] = useState(87)

  useEffect(() => {
    const interval = setInterval(() => {
      setFusionScore((prev) => {
        const newScore = prev + (Math.random() - 0.5) * 4
        return Math.max(60, Math.min(99, newScore))
      })
    }, 2000)

    return () => clearInterval(interval)
  }, [])

  return (
    <div className="space-y-8">
      <div>
        <h2 className="text-2xl font-bold mb-6">Real-Time Sensor Fusion</h2>

        {/* Fusion Score */}
        <Card className="glow-accent p-8 border-accent/20 bg-gradient-to-br from-accent/10 to-card/50 mb-6">
          <div className="flex items-center justify-between">
            <div>
              <h3 className="text-lg text-muted-foreground mb-2">Overall Fusion Score</h3>
              <p className="text-5xl font-bold text-accent">{fusionScore.toFixed(0)}/100</p>
            </div>
            <div className="text-center">
              <div className="w-32 h-32 relative">
                <div className="absolute inset-0 rounded-full border-4 border-accent/20" />
                <div
                  className="absolute inset-0 rounded-full border-4 border-accent"
                  style={{
                    clipPath: `polygon(0 0, ${fusionScore}% 0, ${fusionScore}% 100%, 0 100%)`,
                  }}
                />
                <div className="absolute inset-2 rounded-full bg-background flex items-center justify-center">
                  <span className="text-lg font-bold text-accent">{Math.round(fusionScore)}%</span>
                </div>
              </div>
            </div>
          </div>
        </Card>

        <div className="grid md:grid-cols-2 gap-6">
          {/* Sensor Status */}
          <Card className="glow-accent-sm border-accent/20 bg-card/50 p-6">
            <h3 className="font-semibold text-lg mb-4">Active Sensors</h3>
            <div className="space-y-3">
              {sensorData.map((sensor, idx) => (
                <div key={idx} className="space-y-1">
                  <div className="flex items-center justify-between text-sm">
                    <span className="font-medium">{sensor.name}</span>
                    <span className="font-mono text-accent">{sensor.value}%</span>
                  </div>
                  <div className="w-full bg-card/50 rounded-full h-2 overflow-hidden border border-border">
                    <div className="h-full bg-accent/70 transition-all" style={{ width: `${sensor.value}%` }} />
                  </div>
                </div>
              ))}
            </div>
          </Card>

          {/* Confidence Radar */}
          <Card className="glow-accent-sm border-accent/20 bg-card/50 p-6">
            <h3 className="font-semibold text-lg mb-4">Multi-Sensor Confidence</h3>
            <ResponsiveContainer width="100%" height={250}>
              <RadarChart data={radarData}>
                <PolarGrid stroke="rgba(139, 92, 246, 0.2)" />
                <PolarAngleAxis dataKey="sensor" stroke="rgba(139, 92, 246, 0.5)" />
                <PolarRadiusAxis stroke="rgba(139, 92, 246, 0.3)" />
                <Radar name="Confidence" dataKey="confidence" stroke="#8b5cf6" fill="#8b5cf6" fillOpacity={0.3} />
              </RadarChart>
            </ResponsiveContainer>
          </Card>
        </div>

        {/* Fusion Details */}
        <Card className="glow-accent-sm border-accent/20 bg-card/50 p-6">
          <h3 className="font-semibold text-lg mb-4">Sensor Integration Details</h3>
          <div className="grid md:grid-cols-2 gap-6 text-sm">
            <div>
              <h4 className="font-semibold text-accent mb-2">CCTV Analysis</h4>
              <ul className="space-y-1 text-muted-foreground">
                <li>• 4 persons detected in frame</li>
                <li>• Group formation detected</li>
                <li>• Restricted zone alert: YES</li>
                <li>• Confidence: 98%</li>
              </ul>
            </div>
            <div>
              <h4 className="font-semibold text-accent mb-2">Wi-Fi & Motion Correlation</h4>
              <ul className="space-y-1 text-muted-foreground">
                <li>• Wi-Fi hotspots: 6 devices</li>
                <li>• Motion intensity: HIGH</li>
                <li>• Spike correlation: 0.92</li>
                <li>• Badge access: None recorded</li>
              </ul>
            </div>
          </div>
        </Card>
      </div>
    </div>
  )
}
