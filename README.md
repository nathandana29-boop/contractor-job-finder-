# contractor-job-finder-
https://claude.ai/chat/4dd69c05-9b80-484d-b02e-8eba2642f675
// === package.json ===
{
  "name": "contractor-job-finder",
  "version": "0.1.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1",
    "lucide-react": "^0.263.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "eslintConfig": {
    "extends": [
      "react-app",
      "react-app/jest"
    ]
  },
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
}

// === public/index.html ===
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta name="description" content="Contractor Job Finder - Find work opportunities near you" />
    <script src="https://cdn.tailwindcss.com"></script>
    <title>Contractor Job Finder</title>
  </head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
  </body>
</html>

// === src/index.js ===
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// === src/index.css ===
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}

// === src/App.js ===
import React, { useState, useEffect } from 'react';
import { MapPin, Clock, DollarSign, Wrench, Navigation, Filter, Search, Phone } from 'lucide-react';

const ContractorJobFinder = () => {
  const [jobs, setJobs] = useState([]);
  const [contractors, setContractors] = useState([]);
  const [filters, setFilters] = useState({
    jobType: 'all',
    priority: 'all',
    maxDistance: 50
  });
  const [searchTerm, setSearchTerm] = useState('');
  const [userLocation, setUserLocation] = useState({ lat: 40.7128, lng: -74.0060 }); // NYC default

  // Mock data
  useEffect(() => {
    const mockJobs = [
      {
        id: 'job1',
        title: 'Kitchen Sink Repair',
        jobType: 'plumbing',
        priority: 'HIGH',
        address: '123 Main St, Brooklyn, NY',
        location: { lat: 40.6782, lng: -73.9442 },
        estimatedHours: 2,
        hourlyRate: 85,
        clientName: 'Sarah Johnson',
        clientPhone: '(555) 123-4567',
        description: 'Leaky faucet and clogged drain need fixing',
        status: 'OPEN',
        createdAt: '2024-09-17T09:00:00Z',
        urgency: 'Today'
      },
      {
        id: 'job2',
        title: 'Electrical Outlet Installation',
        jobType: 'electrical',
        priority: 'NORMAL',
        address: '456 Oak Ave, Queens, NY',
        location: { lat: 40.7282, lng: -73.7949 },
        estimatedHours: 1.5,
        hourlyRate: 95,
        clientName: 'Mike Chen',
        clientPhone: '(555) 987-6543',
        description: 'Install 3 new outlets in home office',
        status: 'OPEN',
        createdAt: '2024-09-17T11:30:00Z',
        urgency: 'This Week'
      },
      {
        id: 'job3',
        title: 'HVAC System Check',
        jobType: 'hvac',
        priority: 'LOW',
        address: '789 Pine St, Manhattan, NY',
        location: { lat: 40.7505, lng: -73.9934 },
        estimatedHours: 3,
        hourlyRate: 110,
        clientName: 'Lisa Rodriguez',
        clientPhone: '(555) 456-7890',
        description: 'Annual maintenance and filter replacement',
        status: 'OPEN',
        createdAt: '2024-09-16T14:20:00Z',
        urgency: 'Next Week'
      },
      {
        id: 'job4',
        title: 'Emergency Pipe Burst',
        jobType: 'plumbing',
        priority: 'EMERGENCY',
        address: '321 Cedar Rd, Bronx, NY',
        location: { lat: 40.8448, lng: -73.8648 },
        estimatedHours: 4,
        hourlyRate: 120,
        clientName: 'David Wilson',
        clientPhone: '(555) 111-2222',
        description: 'Basement flooding from burst pipe - URGENT',
        status: 'OPEN',
        createdAt: '2024-09-17T13:45:00Z',
        urgency: 'ASAP'
      }
    ];

    const mockContractors = [
      {
        id: 'c1',
        name: 'Current User',
        location: { lat: 40.7128, lng: -74.0060 },
        skills: ['plumbing', 'electrical']
      }
    ];

    setJobs(mockJobs);
    setContractors(mockContractors);
  }, []);

  // Calculate distance between two points (Haversine formula)
  const calculateDistance = (lat1, lng1, lat2, lng2) => {
    const R = 3959; // Earth's radius in miles
    const dLat = (lat2 - lat1) * Math.PI / 180;
    const dLng = (lng2 - lng1) * Math.PI / 180;
    const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
              Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) *
              Math.sin(dLng/2) * Math.sin(dLng/2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
    return R * c;
  };

  // Filter and sort jobs
  const filteredJobs = jobs
    .filter(job => {
      const matchesType = filters.jobType === 'all' || job.jobType === filters.jobType;
      const matchesPriority = filters.priority === 'all' || job.priority === filters.priority;
      const distance = calculateDistance(userLocation.lat, userLocation.lng, job.location.lat, job.location.lng);
      const matchesDistance = distance <= filters.maxDistance;
      const matchesSearch = job.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
                           job.description.toLowerCase().includes(searchTerm.toLowerCase()) ||
                           job.address.toLowerCase().includes(searchTerm.toLowerCase());
      
      return matchesType && matchesPriority && matchesDistance && matchesSearch;
    })
    .map(job => ({
      ...job,
      distance: calculateDistance(userLocation.lat, userLocation.lng, job.location.lat, job.location.lng),
      estimatedEarnings: job.estimatedHours * job.hourlyRate
    }))
    .sort((a, b) => {
      // Sort by priority first (EMERGENCY > HIGH > NORMAL > LOW), then by distance
      const priorityOrder = { 'EMERGENCY': 4, 'HIGH': 3, 'NORMAL': 2, 'LOW': 1 };
      if (priorityOrder[a.priority] !== priorityOrder[b.priority]) {
        return priorityOrder[b.priority] - priorityOrder[a.priority];
      }
      return a.distance - b.distance;
    });

  const getPriorityColor = (priority) => {
    switch(priority) {
      case 'EMERGENCY': return 'bg-red-500 text-white';
      case 'HIGH': return 'bg-orange-500 text-white';
      case 'NORMAL': return 'bg-blue-500 text-white';
      case 'LOW': return 'bg-gray-500 text-white';
      default: return 'bg-gray-500 text-white';
    }
  };

  const getJobTypeIcon = (type) => {
    return <Wrench className="w-4 h-4" />;
  };

  const handleClaimJob = (jobId) => {
    alert(`Job ${jobId} claimed! In the full app, this would assign the job and send notifications.`);
  };

  return (
    <div className="max-w-6xl mx-auto p-6 bg-gray-50 min-h-screen">
      <div className="mb-8">
        <h1 className="text-3xl font-bold text-gray-800 mb-2">Available Jobs Near You</h1>
        <p className="text-gray-600">Find work opportunities sorted by priority and distance</p>
      </div>

      {/* Search and Filters */}
      <div className="bg-white rounded-lg shadow-sm border border-gray-200 p-6 mb-6">
        <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
          <div className="relative">
            <Search className="absolute left-3 top-3 w-4 h-4 text-gray-400" />
            <input
              type="text"
              placeholder="Search jobs..."
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
              className="w-full pl-10 pr-4 py-2 border border-gray-300 rounded-md"
            />
          </div>

          <select
            value={filters.jobType}
            onChange={(e) => setFilters({...filters, jobType: e.target.value})}
            className="px-3 py-2 border border-gray-300 rounded-md"
          >
            <option value="all">All Job Types</option>
            <option value="plumbing">Plumbing</option>
            <option value="electrical">Electrical</option>
            <option value="hvac">HVAC</option>
            <option value="general">General</option>
          </select>

          <select
            value={filters.priority}
            onChange={(e) => setFilters({...filters, priority: e.target.value})}
            className="px-3 py-2 border border-gray-300 rounded-md"
          >
            <option value="all">All Priorities</option>
            <option value="EMERGENCY">Emergency</option>
            <option value="HIGH">High</option>
            <option value="NORMAL">Normal</option>
            <option value="LOW">Low</option>
          </select>

          <div className="flex items-center space-x-2">
            <span className="text-sm text-gray-600">Max:</span>
            <input
              type="range"
              min="5"
              max="100"
              value={filters.maxDistance}
              onChange={(e) => setFilters({...filters, maxDistance: parseInt(e.target.value)})}
              className="flex-1"
            />
            <span className="text-sm font-medium text-gray-700">{filters.maxDistance}mi</span>
          </div>
        </div>
      </div>

      {/* Job Results */}
      <div className="space-y-4">
        <div className="flex justify-between items-center">
          <h2 className="text-xl font-semibold text-gray-800">
            {filteredJobs.length} Available Jobs
          </h2>
          <div className="flex items-center space-x-2 text-sm text-gray-600">
            <Navigation className="w-4 h-4" />
            <span>Sorted by priority & distance</span>
          </div>
        </div>

        {filteredJobs.length === 0 ? (
          <div className="bg-white rounded-lg shadow-sm border border-gray-200 p-8 text-center">
            <p className="text-gray-500">No jobs found matching your criteria</p>
          </div>
        ) : (
          filteredJobs.map((job) => (
            <div key={job.id} className="bg-white rounded-lg shadow-sm border border-gray-200 p-6 hover:shadow-md transition-shadow">
              <div className="flex justify-between items-start mb-4">
                <div className="flex-1">
                  <div className="flex items-center space-x-3 mb-2">
                    <h3 className="text-lg font-semibold text-gray-800">{job.title}</h3>
                    <span className={`px-2 py-1 rounded text-xs font-medium ${getPriorityColor(job.priority)}`}>
                      {job.priority}
                    </span>
                    <span className="bg-green-100 text-green-800 px-2 py-1 rounded text-xs font-medium">
                      {job.urgency}
                    </span>
                  </div>
                  
                  <div className="flex items-center space-x-4 text-sm text-gray-600 mb-3">
                    <div className="flex items-center space-x-1">
                      {getJobTypeIcon(job.jobType)}
                      <span className="capitalize">{job.jobType}</span>
                    </div>
                    <div className="flex items-center space-x-1">
                      <MapPin className="w-4 h-4" />
                      <span>{job.address}</span>
                    </div>
                    <div className="flex items-center space-x-1">
                      <Navigation className="w-4 h-4" />
                      <span>{job.distance.toFixed(1)} miles away</span>
                    </div>
                  </div>
                  
                  <p className="text-gray-700 mb-3">{job.description}</p>
                  
                  <div className="flex items-center space-x-6 text-sm">
                    <div className="flex items-center space-x-1">
                      <Clock className="w-4 h-4 text-blue-500" />
                      <span>{job.estimatedHours}h estimated</span>
                    </div>
                    <div className="flex items-center space-x-1">
                      <DollarSign className="w-4 h-4 text-green-500" />
                      <span>${job.hourlyRate}/hr (${job.estimatedEarnings} total)</span>
                    </div>
                    <div className="flex items-center space-x-1">
                      <Phone className="w-4 h-4 text-gray-500" />
                      <span>{job.clientName} - {job.clientPhone}</span>
                    </div>
                  </div>
                </div>
                
                <div className="flex flex-col space-y-2 ml-4">
                  <button
                    onClick={() => handleClaimJob(job.id)}
                    className="px-6 py-2 bg-blue-500 text-white rounded-md hover:bg-blue-600 font-medium"
                  >
                    Claim Job
                  </button>
                  <button className="px-6 py-2 border border-gray-300 text-gray-700 rounded-md hover:bg-gray-50">
                    Get Directions
                  </button>
                </div>
              </div>
              
              {/* Fuel Cost Estimate */}
              <div className="border-t border-gray-200 pt-3 mt-3">
                <div className="flex justify-between items-center text-sm">
                  <div className="text-gray-600">
                    Est. fuel cost: <span className="font-medium">${(job.distance * 0.15).toFixed(2)}</span> 
                    <span className="text-xs ml-1">(round trip @ $3.50/gal, 25mpg)</span>
                  </div>
                  <div className="text-gray-600">
                    Net earnings: <span className="font-medium text-green-600">${(job.estimatedEarnings - (job.distance * 0.15)).toFixed(2)}</span>
                  </div>
                </div>
              </div>
            </div>
          ))
        )}
      </div>

      {/* Quick Stats */}
      <div className="mt-8 grid grid-cols-1 md:grid-cols-3 gap-4">
        <div className="bg-white rounded-lg shadow-sm border border-gray-200 p-4">
          <h3 className="font-medium text-gray-800 mb-2">Total Available</h3>
          <p className="text-2xl font-bold text-blue-600">{filteredJobs.length}</p>
        </div>
        <div className="bg-white rounded-lg shadow-sm border border-gray-200 p-4">
          <h3 className="font-medium text-gray-800 mb-2">Average Distance</h3>
          <p className="text-2xl font-bold text-green-600">
            {filteredJobs.length > 0 ? (filteredJobs.reduce((sum, job) => sum + job.distance, 0) / filteredJobs.length).toFixed(1) : 0}mi
          </p>
        </div>
        <div className="bg-white rounded-lg shadow-sm border border-gray-200 p-4">
          <h3 className="font-medium text-gray-800 mb-2">Total Potential Earnings</h3>
          <p className="text-2xl font-bold text-purple-600">
            ${filteredJobs.reduce((sum, job) => sum + job.estimatedEarnings, 0).toFixed(0)}
          </p>
        </div>
      </div>
    </div>
  );
};

export default ContractorJobFinder;
