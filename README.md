<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>A&J CREATES - Anime Avatar Maker</title>
  <style>
    body { font-family: 'Segoe UI',sans-serif; background: #faf0fb; color: #333; margin: 0; padding: 20px; }
    h1 { color: #d63384; text-align: center; }
    #avatarContainer { display: grid; grid-template-columns: repeat(5, 1fr); gap: 10px; max-width: 600px; margin: 20px auto; }
    .avatar-slot { background: white; border: 2px dashed #ccc; aspect-ratio: 1; display: flex; align-items: center; justify-content: center; cursor: pointer; transition: border-color .2s; }
    .avatar-slot:hover { border-color: #d63384; }
    #customizer { max-width: 400px; margin: 20px auto; padding: 20px; background: white; box-shadow: 0 2px 6px rgba(0,0,0,.1); display: none; }
    #customizer h2 { margin-top: 0; color: #d63384; }
    .option-group { margin: 10px 0; text-align: left; }
    .option-group label { display: block; font-size: .9rem; margin-bottom: 4px; }
    select, button { width: 100%; padding: 8px; margin-top: 4px; border: 1px solid #ccc; border-radius: 4px; }
    button.save { background: #d63384; color: #fff; font-weight: bold; }
  </style>
</head>
<body>

  <h1>✨ A&J CREATES ✨</h1>
  <div id="avatarContainer"></div>
  <div id="customizer">
    <h2>Customize Avatar <span id="avatarIndex"></span></h2>
    <div class="option-group"><label>Gender</label><select id="gender"><option>boy</option><option>girl</option></select></div>
    <div class="option-group"><label>Hair</label><select id="hair"><option>short</option><option>long</option><option>spiky</option></select></div>
    <div class="option-group"><label>Eyes</label><select id="eyes"><option>blue</option><option>green</option><option>brown</option></select></div>
    <div class="option-group"><label>Skin</label><select id="skin"><option>light</option><option>medium</option><option>dark</option></select></div>
    <div class="option-group"><label>Clothes</label><select id="clothes"><option>casual</option><option>formal</option><option>ninja</option></select></div>
    <div class="option-group"><label>Accessory</label><select id="accessory"><option>none</option><option>glasses</option><option>hat</option><option>earrings</option></select></div>
    <button class="save" onclick="saveAvatar()">Save</button>
    <button onclick="cancelEdit()">Cancel</button>
  </div>

  <script>
    const MAX = 20;
    const avatars = JSON.parse(localStorage.getItem('avatars') || '{}');
    const container = document.getElementById('avatarContainer');
    const customizer = document.getElementById('customizer');
    let currentIndex = null;

    function init() {
      for (let i = 0; i < MAX; i++) {
        const slot = document.createElement('div');
        slot.className = 'avatar-slot';
        slot.innerText = avatars[i] ? format(avatars[i]) : `Slot ${i+1}`;
        slot.addEventListener('click', () => openCustomizer(i));
        container.append(slot);
      }
    }

    function format(a) {
      return `${a.gender}, ${a.hair} hair, ${a.eyes} eyes`;
    }

    function openCustomizer(i) {
      currentIndex = i;
      const a = avatars[i] || {gender:'girl',hair:'short',eyes:'blue',skin:'light',clothes:'casual',accessory:'none'};
      document.getElementById('avatarIndex').textContent = `#${i+1}`;
      ['gender','hair','eyes','skin','clothes','accessory'].forEach(id => {
        document.getElementById(id).value = a[id];
      });
      customizer.style.display = 'block';
    }

    function saveAvatar() {
      avatars[currentIndex] = {};
      ['gender','hair','eyes','skin','clothes','accessory'].forEach(id => {
        avatars[currentIndex][id] = document.getElementById(id).value;
      });
      localStorage.setItem('avatars', JSON.stringify(avatars));
      refresh();
      customizer.style.display = 'none';
    }

    function cancelEdit() {
      customizer.style.display = 'none';
    }

    function refresh() {
      container.innerHTML = '';
      init();
    }

    window.onload = init;
  </script>
</body>
</html>
