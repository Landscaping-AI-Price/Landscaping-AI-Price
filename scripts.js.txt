Scripts.js

import { AR } from 'ar.js';
import { QRCodeReader } from 'qr-code-reader';

const app = document.getElementById('app');
const qrReader = new QRCodeReader();
const arView = document.getElementById('ar-view');
const measurementForm = document.getElementById('measurement-form');
const measurementInput = document.getElementById('measurement-input');
const measurementButton = document.getElementById('measurement-button');

let anchorPoints = [];
let measurements = [];

qrReader.on('decode', (decodedText) => {
  if (decodedText === 'anchor-point-1') {
    // First anchor point
    anchorPoints.push({
      x: 0,
      y: 0,
    });
    arView.innerHTML = 'Place the flier at the base of the tree trunk';
  } else if (decodedText === 'anchor-point-2') {
    // Second anchor point
    anchorPoints.push({
      x: 0,
      y: 0,
    });
    arView.innerHTML = 'Place the flier at the top of the tree';
  } else if (decodedText === 'anchor-point-3') {
    // Third anchor point
    anchorPoints.push({
      x: 0,
      y: 0,
    });
    arView.innerHTML = 'Place the flier at the end of the branch';
  } else if (decodedText === 'anchor-point-4') {
    // Fourth anchor point
    anchorPoints.push({
      x: 0,
      y: 0,
    });
    arView.innerHTML = 'Place the flier at the top of the tree';
  }
});

measurementButton.addEventListener('click', () => {
  const height = parseFloat(measurementInput.value);
  measurements.push({
    type: 'height',
    value: height,
  });
  measurementInput.value = '';
  measurementForm.reset();
});

// Calculate distances between anchor points
function calculateDistances() {
  const distances = [];
  for (let i = 0; i < anchorPoints.length - 1; i++) {
    const distance = calculateDistance(anchorPoints[i], anchorPoints[i + 1]);
    distances.push(distance);
  }
  return distances;
}

// Calculate distance between two points
function calculateDistance(point1, point2) {
  const x1 = point1.x;
  const y1 = point1.y;
  const x2 = point2.x;
  const y2 = point2.y;
  const dx = x2 - x1;
  const dy = y2 - y1;
  const distance = Math.sqrt(dx * dx + dy * dy);
  return distance;
}

// Convert distance to feet
function convertDistance(distance) {
  return distance * 3.2808; // 1 meter is approximately 3.2808 feet
}

// Display measurements
function displayMeasurements() {
  const measurementsList = document.getElementById('measurements-list');
  measurementsList.innerHTML = '';
  measurements.forEach((measurement) => {
    const measurementListItem = document.createElement('li');
    measurementListItem.textContent = `${measurement.type}: ${measurement.value} feet`;
    measurementsList.appendChild(measurementListItem);
  });
}

// Main app logic
function main() {
  qrReader.start();
  arView.innerHTML = 'Scan the QR code to start';
}

main();
