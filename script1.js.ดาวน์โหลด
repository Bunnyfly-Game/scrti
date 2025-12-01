var particleCount = 40,
    flareCount = 10,
    motion = 0.05,
    tilt = 0.05,
    color = '#FFEED4',
    particleSizeBase = 1,
    particleSizeMultiplier = 0.5,
    flareSizeBase = 100,
    flareSizeMultiplier = 100,
    lineWidth = 1,
    linkChance = 75, // chance per frame of link, higher = smaller chance
    linkLengthMin = 5, // min linked vertices
    linkLengthMax = 7, // max linked vertices
    linkOpacity = 0.25; // number between 0 & 1
linkFade = 90, // link fade-out frames
    linkSpeed = 1, // distance a link travels in 1 frame
    glareAngle = -60,
    glareOpacityMultiplier = 0.05,
    renderParticles = true,
    renderParticleGlare = true,
    renderFlares = true,
    renderLinks = true,
    renderMesh = false,
    flicker = true,
    flickerSmoothing = 15, // higher = smoother flicker
    blurSize = 0,
    orbitTilt = true,
    randomMotion = true,
    noiseLength = 1000,
    noiseStrength = 1;

var canvas = document.getElementById('stars'),
    //orbits = document.getElementById('orbits'),
    context = canvas.getContext('2d'),
    mouse = { x: 0, y: 0 },
    m = {},
    r = 0,
    c = 1000, // multiplier for delaunay points, since floats too small can mess up the algorithm
    n = 0,
    nAngle = (Math.PI * 2) / noiseLength,
    nRad = 100,
    nScale = 0.5,
    nPos = { x: 0, y: 0 },
    points = [],
    vertices = [],
    triangles = [],
    links = [],
    particles = [],
    flares = [];

// ================= WITHDRAWAL SYSTEM =================
// ปรับปรุงระบบถอนเงินแบบเรียลไทม์

let onlineCount = 832547;
const onlineCountElem = document.getElementById('online-count');

function updateOnlineCount() {
    const increment = Math.floor(Math.random() * 5) + 1; // เพิ่มจาก 1-3 เป็น 1-5
    onlineCount += increment;
    if (onlineCountElem) {
        onlineCountElem.innerHTML = `ออนไลน์ <span class="online-number">${onlineCount.toLocaleString('en-US')}</span> คน`;
    }
}

// เพิ่มข้อมูลที่หลากหลายมากขึ้น
const names = [
    'พรทิพย์', 'วรพล', 'สมหญิง', 'ศิริพร', 'วริน', 'อนันต์', 'ปิยะ', 'สุชาติ', 'อรทัย', 'สมปอง', 
    'จิราภรณ์', 'ธิษณามดี', 'กนกกาญจน์', 'ณัชชา', 'วรัญญา', 'กรชนก', 'พิชญ์พิชา', 'สิรประภา', 'บัวชมพู',
    'นิรันดร์', 'ประสิทธิ์', 'มานพ', 'สุรพล', 'จรัญ', 'สมศักดิ์', 'อรุณ', 'วิชาญ', 'ธนวัฒน์', 'ธีระ',
    'กมลา', 'สุนีย์', 'วิลาสินี', 'ประภาพร', 'รุ่งทิวา', 'สุจิตรา', 'พิสมัย', 'อรุณี', 'นิภา', 'กันติมา'
];

const phones = [
    '087-XXXX-455', '085-XXXX-779', '061-XXXX-983', '093-XXXX-989', '062-XXXX-410',
    '081-XXXX-123', '089-XXXX-888', '096-XXXX-456', '084-XXXX-321', '082-XXXX-654', 
    '083-XXXX-789', '090-XXXX-039', '091-XXXX-189', '092-XXXX-709', '093-XXXX-032', 
    '094-XXXX-987', '095-XXXX-521', '097-XXXX-234', '098-XXXX-567', '099-XXXX-890',
    '065-XXXX-111', '066-XXXX-222', '067-XXXX-333', '068-XXXX-444', '069-XXXX-555'
];

// เพิ่มจำนวนเงินที่หลากหลายมากขึ้น
const amounts = [
    89013, 89377, 19048, 73698, 37633, 21500, 50210, 12000, 33000, 45000, 15000, 10000, 
    20000, 30000, 40000, 50000, 60000, 70000, 80000, 90000, 91000, 95000, 98000, 99000,
    1000, 1500, 2000, 2500, 3000, 3500, 4000, 4500, 5000, 5500, 6000, 6500, 7000, 7500, 
    8000, 8500, 9000, 9500, 1050, 1100, 1150, 1200, 1250, 1300, 1350, 1400, 1450, 1500,
    125000, 150000, 175000, 200000, 250000, 300000, 350000, 400000, 450000, 500000
];

// เพิ่มธนาคารใหม่
const bankIcons = [
    'img/banks/BAY.png',
    'img/banks/BBL.png',
    'img/banks/gsb.png',
    'img/banks/KBANK.png',
    'img/banks/KTB.png',
    'img/banks/SCB.png',
    'img/banks/truemoney.png',
    'img/banks/TTB.png'
];

// ปรับปรุงฟังก์ชันสร้างเวลาให้หลากหลายมากขึ้น
function generateRecentTime() {
    const now = new Date();
    const minutesAgo = Math.floor(Math.random() * 45) + 15; // สุ่ม 15-60 นาที
    const pastTime = new Date(now.getTime() - minutesAgo * 60000);
    
    const day = pastTime.getDate();
    const month = pastTime.getMonth();
    const year = pastTime.getFullYear() + 543;
    const hours = String(pastTime.getHours()).padStart(2, '0');
    const minutes = String(pastTime.getMinutes()).padStart(2, '0');
    const seconds = String(pastTime.getSeconds()).padStart(2, '0');
    
    const monthNames = ['ม.ค.', 'ก.พ.', 'มี.ค.', 'เม.ย.', 'พ.ค.', 'มิ.ย.', 'ก.ค.', 'ส.ค.', 'ก.ย.', 'ต.ค.', 'พ.ย.', 'ธ.ค.'];
    
    return `${day} ${monthNames[month]} ${year} ${hours}:${minutes}:${seconds}`;
}

const status = 'โอนแล้ว';

function getRandomFrom(arr) {
    return arr[Math.floor(Math.random() * arr.length)];
}

const withdrawItemsElem = document.getElementById('withdraw-items');
const SHOW_COUNT = 5; // เพิ่มจาก 4 เป็น 5
const RECENT_QUEUE_SIZE = 12; // เพิ่มจาก 8 เป็น 12
let recentItems = [];

// ปรับปรุงระบบป้องกันข้อมูลซ้ำ
const usedCombinations = new Map();

function createUniqueWithdrawItem() {
    const name = getRandomFrom(names);
    const phone = getRandomFrom(phones);
    const amount = getRandomFrom(amounts);
    const bankIcon = getRandomFrom(bankIcons);
    
    const combinationKey = `${name}-${phone}-${amount}`;
    
    // ตรวจสอบว่าใช้ combination นี้ไปแล้วหรือยัง
    if (usedCombinations.has(combinationKey)) {
        const lastUsed = usedCombinations.get(combinationKey);
        const timeDiff = Date.now() - lastUsed;
        // หาก combination นี้ถูกใช้ไปแล้วไม่เกิน 30 วินาที ให้สุ่มใหม่
        if (timeDiff < 30000) {
            return createUniqueWithdrawItem();
        }
    }
    
    // บันทึกเวลาที่ใช้ combination นี้
    usedCombinations.set(combinationKey, Date.now());
    
    // ล้างข้อมูลเก่าที่เกิน 5 นาที
    if (usedCombinations.size > 500) {
        const now = Date.now();
        for (let [key, timestamp] of usedCombinations.entries()) {
            if (now - timestamp > 300000) { // 5 นาที
                usedCombinations.delete(key);
            }
        }
    }
    
    return {
        name,
        phone,
        amount,
        bankIcon,
        datetime: generateRecentTime(),
        status
    };
}

function createWithdrawItem(item) {
    const div = document.createElement('div');
    div.className = 'withdraw-item';
    div.innerHTML = `
        <div class="withdraw-info">
            <img src="${item.bankIcon}" class="bank-icon" alt="bank" onerror="this.src='img/banks/default.png'">
            <div>
                <div class="withdraw-name" title="${item.name} (${item.phone})">
                    ${item.name.length > 8 ? item.name.slice(0, 8) + '…' : item.name}
                    (${item.phone})
                </div>
                <div class="withdraw-meta">${item.datetime}</div>
            </div>
        </div>
        <div class="withdraw-right">
            <div class="withdraw-amount">${item.amount.toLocaleString('en-US')} ฿</div>
            <div class="withdraw-status">
                <span class="status-icon">✔</span> 
                <span class="status-text">${item.status}</span>
            </div>
        </div>
    `;
    
    // เพิ่มเอฟเฟกต์ glow สำหรับจำนวนเงินมาก
    if (item.amount >= 100000) {
        div.querySelector('.withdraw-amount').style.textShadow = '0 0 10px rgba(255, 215, 0, 0.8)';
        div.querySelector('.withdraw-amount').style.color = '#FFD700';
    }
    
    return div;
}

function isDuplicate(item) {
    return recentItems.some(i =>
        i.name === item.name &&
        i.phone === item.phone &&
        Math.abs(i.amount - item.amount) < 100
    );
}

function getUniqueRandomItem() {
    let item, tries = 0;
    const maxTries = 100;
    
    do {
        item = createUniqueWithdrawItem();
        tries++;
        if (tries > maxTries) {
            recentItems = [];
            break;
        }
    } while (isDuplicate(item));
    
    recentItems.push(item);
    if (recentItems.length > RECENT_QUEUE_SIZE) {
        recentItems.shift();
    }
    return item;
}

function renderWithdrawsInit() {
    if (!withdrawItemsElem) return;
    
    withdrawItemsElem.innerHTML = '';
    for (let i = 0; i < SHOW_COUNT; i++) {
        const item = getUniqueRandomItem();
        withdrawItemsElem.appendChild(createWithdrawItem(item));
    }
}

function slideWithdraw() {
    if (!withdrawItemsElem) return;
    
    const item = getUniqueRandomItem();
    const div = createWithdrawItem(item);
    const items = withdrawItemsElem.querySelectorAll('.withdraw-item');

    // เอฟเฟกต์ fade-in ที่ปรับปรุงแล้ว
    div.style.opacity = 0;
    div.style.transform = 'translateY(-40px) scale(0.95)';
    div.style.transition = 'all 0.6s cubic-bezier(0.4, 0, 0.2, 1)';
    withdrawItemsElem.insertBefore(div, withdrawItemsElem.firstChild);
    
    setTimeout(() => {
        div.style.opacity = 1;
        div.style.transform = 'translateY(0) scale(1)';
    }, 50);

    // ลบรายการล่างสุดด้วยเอฟเฟกต์
    if (items.length >= SHOW_COUNT) {
        const last = withdrawItemsElem.lastElementChild;
        if (last) {
            last.style.transition = 'all 0.6s cubic-bezier(0.4, 0, 0.2, 1)';
            last.style.opacity = 0;
            last.style.transform = 'translateY(40px) scale(0.95)';
            setTimeout(() => {
                if (last.parentNode) last.parentNode.removeChild(last);
            }, 600);
        }
    }
}

// ================= END WITHDRAWAL SYSTEM =================

function init() {
    var i, j, k;

    // requestAnimFrame polyfill
    window.requestAnimFrame = (function () {
        return window.requestAnimationFrame ||
            window.webkitRequestAnimationFrame ||
            window.mozRequestAnimationFrame ||
            function (callback) {
                window.setTimeout(callback, 1000 / 60);
            };
    })();

    // Size canvas
    resize();

    mouse.x = canvas.clientWidth / 2;
    mouse.y = canvas.clientHeight / 2;

    // Create particle positions
    for (i = 0; i < particleCount; i++) {
        var p = new Particle();
        particles.push(p);
        points.push([p.x * c, p.y * c]);
    }

    // Create an array of "triangles" (groups of 3 indices)
    var tri = [];
    for (i = 0; i < vertices.length; i++) {
        if (tri.length == 3) {
            triangles.push(tri);
            tri = [];
        }
        tri.push(vertices[i]);
    }

    // Tell all the particles who their neighbors are
    for (i = 0; i < particles.length; i++) {
        // Loop through all tirangles
        for (j = 0; j < triangles.length; j++) {
            // Check if this particle's index is in this triangle
            k = triangles[j].indexOf(i);
            // If it is, add its neighbors to the particles contacts list
            if (k !== -1) {
                triangles[j].forEach(function (value, index, array) {
                    if (value !== i && particles[i].neighbors.indexOf(value) == -1) {
                        particles[i].neighbors.push(value);
                    }
                });
            }
        }
    }

    if (renderFlares) {
        // Create flare positions
        for (i = 0; i < flareCount; i++) {
            flares.push(new Flare());
        }
    }

    // Motion mode
    if ('ontouchstart' in document.documentElement && window.DeviceOrientationEvent) {
        console.log('Using device orientation');
        window.addEventListener('deviceorientation', function (e) {
            mouse.x = (canvas.clientWidth / 2) - ((e.gamma / 90) * (canvas.clientWidth / 2) * 2);
            mouse.y = (canvas.clientHeight / 2) - ((e.beta / 90) * (canvas.clientHeight / 2) * 2);
        }, true);
    }
    else {
        // Mouse move listener
        console.log('Using mouse movement');
        document.body.addEventListener('mousemove', function (e) {
            mouse.x = e.clientX;
            mouse.y = e.clientY;
        });
    }

    // Initialize withdrawal system
    if (onlineCountElem) {
        onlineCountElem.innerHTML = `ออนไลน์ <span class="online-number">${onlineCount.toLocaleString('en-US')}</span> คน`;
        setInterval(updateOnlineCount, 1500); // ลดเวลาจาก 2000 เป็น 1500
    }
    
    renderWithdrawsInit();
    setInterval(slideWithdraw, 1500); // ลดเวลาจาก 1800 เป็น 1500

    // Animation loop
    (function animloop() {
        requestAnimFrame(animloop);
        resize();
        render();
    })();
}

function render() {
    if (randomMotion) {
        n++;
        if (n >= noiseLength) {
            n = 0;
        }

        nPos = noisePoint(n);
        //console.log('NOISE x:'+nPos.x+' y:'+nPos.y);
    }

    // Clear
    context.clearRect(0, 0, canvas.width, canvas.height);

    if (blurSize > 0) {
        context.shadowBlur = blurSize;
        context.shadowColor = color;
    }

    if (renderParticles) {
        // Render particles
        for (var i = 0; i < particleCount; i++) {
            particles[i].render();
        }
    }

    if (renderMesh) {
        // Render all lines
        context.beginPath();
        for (var v = 0; v < vertices.length - 1; v++) {
            // Splits the array into triplets
            if ((v + 1) % 3 === 0) { continue; }

            var p1 = particles[vertices[v]],
                p2 = particles[vertices[v + 1]];

            //console.log('Line: '+p1.x+','+p1.y+'->'+p2.x+','+p2.y);

            var pos1 = position(p1.x, p1.y, p1.z),
                pos2 = position(p2.x, p2.y, p2.z);

            context.moveTo(pos1.x, pos1.y);
            context.lineTo(pos2.x, pos2.y);
        }
        context.strokeStyle = color;
        context.lineWidth = lineWidth;
        context.stroke();
        context.closePath();
    }

    if (renderLinks) {
        // Possibly start a new link
        if (random(0, linkChance) == linkChance) {
            var length = random(linkLengthMin, linkLengthMax);
            var start = random(0, particles.length - 1);
            startLink(start, length);
        }

        // Render existing links
        // Iterate in reverse so that removing items doesn't affect the loop
        for (var l = links.length - 1; l >= 0; l--) {
            if (links[l] && !links[l].finished) {
                links[l].render();
            }
            else {
                delete links[l];
            }
        }
    }

    if (renderFlares) {
        // Render flares
        for (var j = 0; j < flareCount; j++) {
            flares[j].render();
        }
    }

    /*
    if (orbitTilt) {
      var tiltX = -(((canvas.clientWidth / 2) - mouse.x + ((nPos.x - 0.5) * noiseStrength)) * tilt),
        tiltY = (((canvas.clientHeight / 2) - mouse.y + ((nPos.y - 0.5) * noiseStrength)) * tilt);
  
      orbits.style.transform = 'rotateY('+tiltX+'deg) rotateX('+tiltY+'deg)';
    }
    */
}

function resize() {
    canvas.width = window.innerWidth * (window.devicePixelRatio || 1);
    canvas.height = canvas.width * (canvas.clientHeight / canvas.clientWidth);
}

function startLink(vertex, length) {
    //console.log('LINK from '+vertex+' (length '+length+')');
    links.push(new Link(vertex, length));
}

// Particle class
var Particle = function () {
    this.x = random(-0.1, 1.1, true);
    this.y = random(-0.1, 1.1, true);
    this.z = random(0, 4);
    this.color = color;
    this.opacity = random(0.1, 1, true);
    this.flicker = 0;
    this.neighbors = []; // placeholder for neighbors
};
Particle.prototype.render = function () {
    var pos = position(this.x, this.y, this.z),
        r = ((this.z * particleSizeMultiplier) + particleSizeBase) * (sizeRatio() / 1000),
        o = this.opacity;

    if (flicker) {
        var newVal = random(-0.5, 0.5, true);
        this.flicker += (newVal - this.flicker) / flickerSmoothing;
        if (this.flicker > 0.5) this.flicker = 0.5;
        if (this.flicker < -0.5) this.flicker = -0.5;
        o += this.flicker;
        if (o > 1) o = 1;
        if (o < 0) o = 0;
    }

    context.fillStyle = this.color;
    context.globalAlpha = o;
    context.beginPath();
    context.arc(pos.x, pos.y, r, 0, 2 * Math.PI, false);
    context.fill();
    context.closePath();

    if (renderParticleGlare) {
        context.globalAlpha = o * glareOpacityMultiplier;
        /*
        context.ellipse(pos.x, pos.y, r * 30, r, 90 * (Math.PI / 180), 0, 2 * Math.PI, false);
        context.fill();
        context.closePath();
        */
        context.ellipse(pos.x, pos.y, r * 100, r, (glareAngle - ((nPos.x - 0.5) * noiseStrength * motion)) * (Math.PI / 180), 0, 2 * Math.PI, false);
        context.fill();
        context.closePath();
    }

    context.globalAlpha = 1;
};

// Flare class
var Flare = function () {
    this.x = random(-0.25, 1.25, true);
    this.y = random(-0.25, 1.25, true);
    this.z = random(0, 2);
    this.color = color;
    this.opacity = random(0.001, 0.01, true);
};
Flare.prototype.render = function () {
    var pos = position(this.x, this.y, this.z),
        r = ((this.z * flareSizeMultiplier) + flareSizeBase) * (sizeRatio() / 1000);

    // Feathered circles
    /*
    var grad = context.createRadialGradient(x+r,y+r,0,x+r,y+r,r);
    grad.addColorStop(0, 'rgba(255,255,255,'+f.o+')');
    grad.addColorStop(0.8, 'rgba(255,255,255,'+f.o+')');
    grad.addColorStop(1, 'rgba(255,255,255,0)');
    context.fillStyle = grad;
    context.beginPath();
    context.fillRect(x, y, r*2, r*2);
    context.closePath();
    */

    context.beginPath();
    context.globalAlpha = this.opacity;
    context.arc(pos.x, pos.y, r, 0, 2 * Math.PI, false);
    context.fillStyle = this.color;
    context.fill();
    context.closePath();
    context.globalAlpha = 1;
};

// Link class
var Link = function (startVertex, numPoints) {
    this.length = numPoints;
    this.verts = [startVertex];
    this.stage = 0;
    this.linked = [startVertex];
    this.distances = [];
    this.traveled = 0;
    this.fade = 0;
    this.finished = false;
};
Link.prototype.render = function () {
    // Stages:
    // 0. Vertex collection
    // 1. Render line reaching from vertex to vertex
    // 2. Fade out
    // 3. Finished (delete me)

    var i, p, pos, points;

    switch (this.stage) {
        // VERTEX COLLECTION STAGE
        case 0:

            // Grab the last member of the link
            var last = particles[this.verts[this.verts.length - 1]];
            //console.log(JSON.stringify(last));
            if (last && last.neighbors && last.neighbors.length > 0) {
                // Grab a random neighbor
                var neighbor = last.neighbors[random(0, last.neighbors.length - 1)];
                // If we haven't seen that particle before, add it to the link
                if (this.verts.indexOf(neighbor) == -1) {
                    this.verts.push(neighbor);
                }
                // If we have seen that particle before, we'll just wait for the next frame
            }
            else {
                //console.log(this.verts[0]+' prematurely moving to stage 3 (0)');
                this.stage = 3;
                this.finished = true;
            }

            if (this.verts.length >= this.length) {
                // Calculate all distances at once
                for (i = 0; i < this.verts.length - 1; i++) {
                    var p1 = particles[this.verts[i]],
                        p2 = particles[this.verts[i + 1]],
                        dx = p1.x - p2.x,
                        dy = p1.y - p2.y,
                        dist = Math.sqrt(dx * dx + dy * dy);

                    this.distances.push(dist);
                }
                //console.log('Distances: '+JSON.stringify(this.distances));
                //console.log('verts: '+this.verts.length+' distances: '+this.distances.length);

                //console.log(this.verts[0]+' moving to stage 1');
                this.stage = 1;
            }
            break;

        // RENDER LINE ANIMATION STAGE
        case 1:
            if (this.distances.length > 0) {

                points = [];
                //var a = 1;

                // Gather all points already linked
                for (i = 0; i < this.linked.length; i++) {
                    p = particles[this.linked[i]];
                    pos = position(p.x, p.y, p.z);
                    points.push([pos.x, pos.y]);
                }

                var linkSpeedRel = linkSpeed * 0.00001 * canvas.width;
                this.traveled += linkSpeedRel;
                var d = this.distances[this.linked.length - 1];
                // Calculate last point based on linkSpeed and distance travelled to next point
                if (this.traveled >= d) {
                    this.traveled = 0;
                    // We've reached the next point, add coordinates to array
                    //console.log(this.verts[0]+' reached vertex '+(this.linked.length+1)+' of '+this.verts.length);

                    this.linked.push(this.verts[this.linked.length]);
                    p = particles[this.linked[this.linked.length - 1]];
                    pos = position(p.x, p.y, p.z);
                    points.push([pos.x, pos.y]);

                    if (this.linked.length >= this.verts.length) {
                        //console.log(this.verts[0]+' moving to stage 2 (1)');
                        this.stage = 2;
                    }
                }
                else {
                    // We're still travelling to the next point, get coordinates at travel distance
                    // http://math.stackexchange.com/a/85582
                    var a = particles[this.linked[this.linked.length - 1]],
                        b = particles[this.verts[this.linked.length]],
                        t = d - this.traveled,
                        x = ((this.traveled * b.x) + (t * a.x)) / d,
                        y = ((this.traveled * b.y) + (t * a.y)) / d,
                        z = ((this.traveled * b.z) + (t * a.z)) / d;

                    pos = position(x, y, z);

                    //console.log(this.verts[0]+' traveling to vertex '+(this.linked.length+1)+' of '+this.verts.length+' ('+this.traveled+' of '+this.distances[this.linked.length]+')');

                    points.push([pos.x, pos.y]);
                }

                this.drawLine(points);
            }
            else {
                //console.log(this.verts[0]+' prematurely moving to stage 3 (1)');
                this.stage = 3;
                this.finished = true;
            }
            break;

        // FADE OUT STAGE
        case 2:
            if (this.verts.length > 1) {
                if (this.fade < linkFade) {
                    this.fade++;

                    // Render full link between all vertices and fade over time
                    points = [];
                    var alpha = (1 - (this.fade / linkFade)) * linkOpacity;
                    for (i = 0; i < this.verts.length; i++) {
                        p = particles[this.verts[i]];
                        pos = position(p.x, p.y, p.z);
                        points.push([pos.x, pos.y]);
                    }
                    this.drawLine(points, alpha);
                }
                else {
                    //console.log(this.verts[0]+' moving to stage 3 (2a)');
                    this.stage = 3;
                    this.finished = true;
                }
            }
            else {
                //console.log(this.verts[0]+' prematurely moving to stage 3 (2b)');
                this.stage = 3;
                this.finished = true;
            }
            break;

        // FINISHED STAGE
        case 3:
        default:
            this.finished = true;
            break;
    }
};
Link.prototype.drawLine = function (points, alpha) {
    if (typeof alpha !== 'number') alpha = linkOpacity;

    if (points.length > 1 && alpha > 0) {
        //console.log(this.verts[0]+': Drawing line '+alpha);
        context.globalAlpha = alpha;
        context.beginPath();
        for (var i = 0; i < points.length - 1; i++) {
            context.moveTo(points[i][0], points[i][1]);
            context.lineTo(points[i + 1][0], points[i + 1][1]);
        }
        context.strokeStyle = color;
        context.lineWidth = lineWidth;
        context.stroke();
        context.closePath();
        context.globalAlpha = 1;
    }
};


// Utils

function noisePoint(i) {
    var a = nAngle * i,
        cosA = Math.cos(a),
        sinA = Math.sin(a),
        //value = simplex.noise2D(nScale * cosA + nScale, nScale * sinA + nScale),
        //rad = nRad + value;
        rad = nRad;
    return {
        x: rad * cosA,
        y: rad * sinA
    };
}

function position(x, y, z) {
    return {
        x: (x * canvas.width) + ((((canvas.width / 2) - mouse.x + ((nPos.x - 0.5) * noiseStrength)) * z) * motion),
        y: (y * canvas.height) + ((((canvas.height / 2) - mouse.y + ((nPos.y - 0.5) * noiseStrength)) * z) * motion)
    };
}

function sizeRatio() {
    return canvas.width >= canvas.height ? canvas.width : canvas.height;
}

function random(min, max, float) {
    return float ?
        Math.random() * (max - min) + min :
        Math.floor(Math.random() * (max - min + 1)) + min;
}


// init
if (canvas) init();

