# Jinro.mbti16typetest.bata
  const q=Qs[cur],n=cur+1,total=Qs.length;
  document.getElementById('q-counter').textContent=`Q${String(n).padStart(2,'0')} / ${total}`;
  document.getElementById('q-pct').textContent=Math.round(cur/total*100)+'%';
  document.getElementById('prog-fill').style.width=(cur/total*100)+'%';
  document.getElementById('q-num').textContent='Q'+String(n).padStart(2,'0');
  document.getElementById('q-text').textContent=q.t;
  const wrap=document.getElementById('scale-wrap');
  wrap.innerHTML='';
  const opts=[
    {val:2,icon:'◎',label:'そう思う'},
    {val:1,icon:'○',label:'ややそう思う'},
    {val:0,icon:'△',label:'どちらとも言えない'},
    {val:-1,icon:'×',label:'あまりそう思わない'},
    {val:-2,icon:'✕',label:'そう思わない'},
  ];
  opts.forEach(o=>{
    const b=document.createElement('button');
    b.className='scale-btn';
    b.innerHTML=`<span class="s-icon">${o.icon}</span>${o.label}`;
    b.onclick=()=>pick(o.val);
    wrap.appendChild(b);
  });
}

function pick(val){
  const q=Qs[cur];
  scores[q.ax]+=val*q.dir;
  cur++;
  if(cur>=Qs.length) showResult(); else renderQ();
}

function showResult(){
  const A=scores.a>=0?'A':'P';
  const S=scores.s>=0?'S':'O';
  const I=scores.i>=0?'I':'L';
  const R=scores.r>=0?'R':'I';
  const code=A+S+I+R;
  const j=JOBS[code]||JOBS['ASIR'];

  document.getElementById('res-code').textContent=code;
  document.getElementById('res-job').textContent=j.name;
  document.getElementById('res-job').style.color=j.color;
  document.getElementById('res-tagline').textContent=j.tagline;

  const da=document.getElementById('dot-art');
  buildSprite(code,da,'full');

  // axis bars
  const barsEl=document.getElementById('axis-bars');
  barsEl.innerHTML='<div class="bars-title">▶ AXIS SCORE</div>';
  const axes=[
    {key:'a',left:'能動',right:'受動',color:'#e07040',max:26},
    {key:'s',left:'主観',right:'客観',color:'#4aaa6a',max:26},
    {key:'i',left:'直感',right:'論理',color:'#4a9fb8',max:26},
    {key:'r',left:'現実',right:'理想',color:'#9b6dd6',max:26},
  ];
  axes.forEach(ax=>{
    const sc=scores[ax.key];
    const pct=((sc+ax.max)/(ax.max*2))*100;
    const row=document.createElement('div');
    row.className='axis-bar-row';
    row.innerHTML=`
      <div class="axis-bar-labels">
        <span style="color:${ax.color}">${ax.left}</span>
        <span style="color:var(--text-dim);font-family:var(--dot);font-size:10px;">${sc>0?'+':''}${sc}</span>
        <span style="color:var(--text-dim)">${ax.right}</span>
      </div>
      <div class="axis-bar-track">
        <div class="axis-bar-fill" style="width:${pct}%;background:${ax.color};"></div>
        <div class="axis-bar-mid"></div>
      </div>`;
    barsEl.appendChild(row);
  });

  ['res-str','res-weak'].forEach((id,i)=>{
    const el=document.getElementById(id); el.innerHTML='';
    (i===0?j.str:j.weak).forEach(t=>{
      const li=document.createElement('li'); li.textContent=t; el.appendChild(li);
    });
  });

  document.getElementById('res-play').textContent=j.play;

  const compat=document.getElementById('res-compat');
  compat.innerHTML='';
  j.compat.forEach((c,i)=>{
    const sp=document.createElement('span');
    sp.className='compat-chip'+(i===0?' first':'');
    sp.textContent=(i+1)+'. '+c+' '+JOBS[c].name;
    compat.appendChild(sp);
  });

  document.querySelectorAll('.r-card').forEach(el=>el.style.borderColor=j.color+'55');
  showScreen('sc-result');
}

function retry(){ showScreen('sc-top'); }
function showScreen(id){
  document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  window.scrollTo(0,0);
}
</script>
</body>
</html>
