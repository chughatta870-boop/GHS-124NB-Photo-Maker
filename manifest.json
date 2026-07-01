const upload = document.getElementById('upload');
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');
const previewArea = document.getElementById('previewArea');
const kbSize = document.getElementById('kbSize');
const downloadBtn = document.getElementById('download');
const resetBtn = document.getElementById('reset');

const TARGET_W = 600;
const TARGET_H = 800;
const MIN_KB = 10;
const MAX_KB = 25;

upload.addEventListener('change', e => {
  const file = e.target.files[0];
  if(!file) return;

  const img = new Image();
  img.onload = () => {
    // 1. White Background Fill
    ctx.fillStyle = '#FFFFFF';
    ctx.fillRect(0, 0, TARGET_W, TARGET_H);

    // 2. Image ko Center me Fit karo, Crop nahi
    const scale = Math.min(TARGET_W / img.width, TARGET_H / img.height);
    const w = img.width * scale;
    const h = img.height * scale;
    const x = (TARGET_W - w) / 2;
    const y = (TARGET_H - h) / 2;
    ctx.drawImage(img, x, y, w, h);

    previewArea.style.display = 'block';
    compressToTargetKB(0.9); // Start with 90% quality
  };
  img.src = URL.createObjectURL(file);
});

function compressToTargetKB(quality){
  canvas.toBlob(blob => {
    let sizeKB = blob.size / 1024;
    kbSize.textContent = sizeKB.toFixed(1) + ' KB';

    // 3. Auto Compress Logic: 10KB to 25KB ke beech laana hai
    if(sizeKB > MAX_KB && quality > 0.3){
      compressToTargetKB(quality - 0.1); // Quality kam karo
    } else {
      window.finalBlob = blob; // Save kar lo download ke liye
    }
  }, 'image/jpeg', quality);
}

downloadBtn.onclick = () => {
  if(!window.finalBlob) return alert('Pehle picture upload karein');
  const a = document.createElement('a');
  a.href = URL.createObjectURL(window.finalBlob);
  a.download = 'passport_600x800.jpg';
  a.click();
};

resetBtn.onclick = () => {
  ctx.clearRect(0,0,TARGET_W,TARGET_H);
  ctx.fillStyle = '#FFFFFF';
  ctx.fillRect(0,0,TARGET_W,TARGET_H);
  previewArea.style.display = 'none';
  upload.value = '';
  kbSize.textContent = '0 KB';
};
