<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>COREX//SYNC Clothing Hud</title>
    <style>
        :root { --purple-trans: rgba(0, 0, 0, 0.6); --glow: 0 0 10px rgba(255, 238, 219, 0.6); --text-glow: 0 0 10px rgba(255, 238, 219, 0.8); }
        body { margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; color: #cbaf9b; min-height: 100vh; display: flex; justify-content: center; align-items: center; position: relative; background: #000; }
        body::before { content: ""; position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-image: url('closet1.jpg'); background-size: cover; background-position: center; background-attachment: fixed; opacity: 0.5; z-index: -1; }
        .container { width: 63%; max-width: 600px; height: 350px; overflow-y: auto; background: rgba(0, 0, 0, 0.5); padding: 10px; border-radius: 12px; border: 7px solid var(--purple-trans); text-align: center; }
        .container::-webkit-scrollbar { width: 8px; }
        .container::-webkit-scrollbar-thumb { background: rgba(255, 238, 219, 0.3); border-radius: 3px; }
        h1 { font-size: 32px; text-align: center; color: #e0d0c9; letter-spacing: 4px; text-transform: uppercase; margin: 0 0 20px 0; cursor: pointer; font-family: 'Courier New', Courier, monospace; font-weight: 350; text-shadow: var(--text-glow); }
        
        .menu-btn, .sub-btn, .nude-btn, .remove-hud-btn { display: block; width: 60%; margin: 0 auto 10px auto; background: rgba(0, 0, 0, 0.6); border: 2px solid rgba(255, 238, 219, 0.4); color: #FFF6ED; padding: 15px 10px; text-transform: uppercase; font-family: 'Courier New', Courier, monospace; font-size: 22px; font-weight: 700; cursor: pointer; border-radius: 6px; opacity: 0.5; transition: all 0.2s ease; }
        
        /* Slight Glow on Hover - only for inactive buttons */
        .menu-btn:hover:not(.active-glow), 
        .sub-btn:hover:not(.active-glow), 
        .nude-btn:hover:not(.active-glow), 
        .remove-hud-btn:hover:not(.active-glow) { 
            border-color: #e0d0c9; 
            box-shadow: 0 0 8px rgba(255, 238, 219, 0.4); 
            opacity: 0.7;
        }

        .slot-btn { width: 40px; height: 35px; font-size: 20px; font-family: 'Courier New', Courier, monospace; background: rgba(0,0,0,0.4); color: #FFF6ED; border: 2px solid rgba(255, 238, 219, 0.4); cursor: pointer; margin: 3px; transition: all 0.2s ease; }
        /* Slight Glow on Hover for slots */
        .slot-btn:hover { border-color: #e0d0c9; box-shadow: 0 0 8px rgba(255, 238, 219, 0.4); }

        .active-glow { border-color: #e0d0c9 !important; box-shadow: var(--glow) !important; opacity: 1 !important; }
        .slot-container, .submenu-items { display: none; padding: 10px; }
        .content { display: none; }
        .controls { display: none; grid-template-columns: 1fr 1fr; gap: 15px; margin-top: 30px; }
        .ctrl-btn { background: rgba(0,0,0,0.4); border: 3px solid var(--purple-trans); color: #e0d0c9; padding: 15px; font-size: 17px; cursor: pointer; }
    </style>
</head>
<body>
    <div class="container">
        <h1 onclick="showHome()">𝙲𝙾𝚁𝙴𝚇//𝚂𝚈𝙽𝙲</h1>
        
        <div id="home">
            <button class="menu-btn" onclick="showCategory(this, 'dresser')">Dresser</button>
            <button class="menu-btn" onclick="showCategory(this, 'closet')">Closet</button>
            <button class="menu-btn" onclick="showCategory(this, 'wardrobe')">Wardrobe</button>
            <button class="menu-btn" onclick="showCategory(this, 'nude_cat')">Nude</button>
            <button class="remove-hud-btn" onclick="send(this,'HUD','remove')">Remove HUD</button>
        </div>

        <div id="dresser" class="content"></div>
        <div id="closet" class="content"></div>
        <div id="wardrobe" class="content"></div>
        
        <div id="nude_cat" class="content">
            <div class="slot-container" style="display:block;">
                <button class="slot-btn" onclick="send(this,'Nude','style1')">1</button>
                <button class="slot-btn" onclick="send(this,'Nude','style2')">2</button>
                <button class="slot-btn" onclick="send(this,'Nude','style3')">3</button>
                <button class="slot-btn" onclick="send(this,'Nude','style4')">4</button>
                <button class="slot-btn" onclick="send(this,'Nude','style5')">5</button>
                <button class="slot-btn" onclick="send(this,'Nude','style6')">6</button>
            </div>
            <button class="ctrl-btn" onclick="showHome()">Back</button>
        </div>

        <div id="controls" class="controls">
            <button class="ctrl-btn" onclick="showHome()">Back</button>
            <button class="ctrl-btn" onclick="send(this,'RemoveLast','detach')">Remove Last Item</button>
        </div>
    </div>

    <script>
        window.onload = function() {
            addCategory('dresser', [["Lounge", "Dresser/Lounge"], ["Pjs", "Dresser/Pjs"], ["Swimwear", "Dresser/Swimwear"], ["Undies", null, [["Bras", "Dresser/Undies/Bras"], ["Panties", "Dresser/Undies/Panties"], ["Boxer/Briefs", "Dresser/Undies/BoxerBriefs"], ["Lingerie", "Dresser/Undies/Lingerie"], ["Stockings", "Dresser/Undies/Stockings"], ["Socks", "Dresser/Undies/Socks"]]]]);
            addCategory('closet', [["Skirts", "Closet/Skirts"], ["Shorts", "Closet/Shorts"], ["Sweaters", "Closet/Sweaters"], ["Dresses", null, [["Long", "Closet/Dresses/Long"], ["Short", "Closet/Dresses/Short"]]], ["Shirts", null, [["Dress Shirts", "Closet/Shirts/DressShirts"], ["Long Sleeve", "Closet/Shirts/LongSleeve"], ["Short Sleeve", "Closet/Shirts/ShortSleeve"], ["Tanks/T's", "Closet/Shirts/TanksTs"], ["Tube Tops", "Closet/Shirts/TubeTops"]]], ["Pants", null, [["Slacks", "Closet/Pants/Slacks"], ["Casual", "Closet/Pants/Casual"], ["Jeans", "Closet/Pants/Jeans"], ["Leggings", "Closet/Pants/Leggings"]]]]);
            addCategory('wardrobe', [["Casual", "Wardrobe/Casual"], ["Club", "Wardrobe/Club"], ["Shopping", "Wardrobe/Shopping"], ["Sleep", "Wardrobe/Sleep"], ["Roleplay", "Wardrobe/Roleplay"], ["Formal", null, [["Gowns", "Wardrobe/Formal/Gowns"], ["Suits", "Wardrobe/Formal/Suits"]]], ["Seasons", null, [["Spring", "Wardrobe/Seasons/Spring"], ["Summer", "Wardrobe/Seasons/Summer"], ["Fall", "Wardrobe/Seasons/Fall"], ["Winter", "Wardrobe/Seasons/Winter"]]]]);
        };

        function send(btn, part, style) { 
            let simUrl = window.location.hash.substring(1);
            if (!simUrl) { return; }
            btn.style.pointerEvents = 'none';
            setTimeout(() => { btn.style.pointerEvents = 'auto'; }, 800);
            simUrl = decodeURIComponent(simUrl);
            const form = document.getElementById('sl_form');
            document.getElementById('p_data').value = part + "," + style; 
            form.action = simUrl;
            form.submit(); 
        }

        function toggleSubmenu(btn, subDiv) { 
            if (subDiv.style.display === 'block') { subDiv.style.display = 'none'; btn.classList.remove('active-glow'); } 
            else { document.querySelectorAll('.sub-btn').forEach(b => b.classList.remove('active-glow')); subDiv.style.display = 'block'; btn.classList.add('active-glow'); }
        }

        function toggleOnlySlots(btn) { 
            let slots = btn.nextElementSibling;
            if (slots.style.display === 'block') { slots.style.display = 'none'; btn.classList.remove('active-glow'); } 
            else { document.querySelectorAll('.sub-btn, .nude-btn, .menu-btn').forEach(b => b.classList.remove('active-glow')); slots.style.display = 'block'; btn.classList.add('active-glow'); }
        }

        function showCategory(btn, id) { 
            document.querySelectorAll('.menu-btn').forEach(b => b.classList.remove('active-glow'));
            btn.classList.add('active-glow');
            document.getElementById('home').style.display = 'none'; 
            document.getElementById(id).style.display = 'block'; 
            document.getElementById('controls').style.display = (id === 'nude_cat') ? 'none' : 'grid'; 
        }

        function showHome() { 
            document.querySelectorAll('.menu-btn, .sub-btn, .nude-btn').forEach(b => b.classList.remove('active-glow'));
            document.getElementById('home').style.display = 'block'; 
            document.querySelectorAll('.content').forEach(c => c.style.display = 'none'); 
            document.getElementById('controls').style.display = 'none'; 
        }
        
        function addCategory(id, items) { 
            var container = document.getElementById(id);
            items.forEach(item => {
                var btn = document.createElement('button'); btn.className = 'sub-btn'; btn.innerText = item[0];
                if(item[2]) {
                    var subDiv = document.createElement('div'); subDiv.className = 'submenu-items';
                    item[2].forEach(sub => {
                        var subBtn = document.createElement('button'); subBtn.className = 'sub-btn'; subBtn.innerText = sub[0];
                        var slots = document.createElement('div'); slots.className = 'slot-container';
                        for(var i=1; i<=6; i++) slots.innerHTML += `<button class="slot-btn" onclick="send(this,'${sub[1]}','style${i}')">${i}</button>`;
                        subBtn.onclick = () => toggleOnlySlots(subBtn);
                        subDiv.appendChild(subBtn); subDiv.appendChild(slots);
                    });
                    btn.onclick = () => toggleSubmenu(btn, subDiv);
                    container.appendChild(btn); container.appendChild(subDiv);
                } else {
                    btn.onclick = () => toggleOnlySlots(btn);
                    var div = document.createElement('div'); div.className = 'slot-container';
                    for(var i=1; i<=6; i++) div.innerHTML += `<button class="slot-btn" onclick="send(this,'${item[1]}','style${i}')">${i}</button>`;
                    container.appendChild(btn); container.appendChild(div);
                }
            });
        }
    </script>
    <iframe name="secret_frame" style="display:none;"></iframe>
    <form id="sl_form" method="POST" target="secret_frame" style="display:none;"><input type="hidden" id="p_data" name="data"></form>
</body>
</html>
