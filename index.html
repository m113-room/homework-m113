'use strict';

const SCRIPT_URL = 'https://script.google.com/macros/s/' + 
                   'AKfycbw1lacaaSbd6ZVh5jrD_s7yU1lmK6bmG_31yzUywyknR7xPZ7BDd_RLAygQHtgfvPRu/exec';

let currentRole='member', currentUsername='', currentTab='pending';
let currentDate=new Date(), selectedDateStr=getFormattedDate(new Date());
let tasks=[], dailyEvents={}, allowedUsersList=[];
let subjectOptions={codes:[],subjects:[]};

let lastActivityTime = Date.now();
const IDLE_TIMEOUT_MS = 7 * 60 * 1000;

function resetIdleTimer() { lastActivityTime = Date.now(); }
['click', 'touchstart', 'keypress', 'scroll', 'mousemove'].forEach(evt => {
  window.addEventListener(evt, resetIdleTimer, { passive: true });
});

setInterval(() => {
  if (localStorage.getItem('isLoggedIn') === 'true') {
    if (Date.now() - lastActivityTime > IDLE_TIMEOUT_MS) logoutDueToTimeout();
  }
}, 30000);

function logoutDueToTimeout() {
  alert('เซสชันหมดอายุเนื่องจากไม่มีการใช้งานเกิน 7 นาที');
  logout();
}

document.addEventListener('visibilitychange', () => {
  if (document.visibilityState === 'visible') {
    if (localStorage.getItem('isLoggedIn') === 'true') {
      if (Date.now() - lastActivityTime > IDLE_TIMEOUT_MS) {
        logoutDueToTimeout();
      } else {
        fetchAllData(false);
      }
    }
  }
});

const MONTH_NAMES_TH=['มกราคม','กุมภาพันธ์','มีนาคม','เมษายน','พฤษภาคม','มิถุนายน','กรกฎาคม','สิงหาคม','กันยายน','ตุลาคม','พฤศจิกายน','ธันวาคม'];
const MONTH_SHORT_TH=['ม.ค.','ก.พ.','มี.ค.','เม.ย.','พ.ค.','มิ.ย.','ก.ค.','ส.ค.','ก.ย.','ต.ค.','พ.ย.','ธ.ค.'];
const DAY_SHORT_TH=['อา.','จ.','อ.','พ.','พฤ.','ศ.','ส.'];

function showLoading(text='กำลังโหลดข้อมูล...'){
  document.getElementById('loading-text').textContent=text;
  document.getElementById('loading-overlay').classList.remove('hidden');
}
function hideLoading(){
  document.getElementById('loading-overlay').classList.add('hidden');
}

function getFormattedDate(d){
  return `${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}-${String(d.getDate()).padStart(2,'0')}`;
}
function normalizeDateStr(v){
  if(!v)return '';
  if(typeof v==='string'){
    const s=v.trim();
    const m=s.match(/^(\d{4})-(\d{1,2})-(\d{1,2})/);
    if(m)return `${m[1]}-${String(m[2]).padStart(2,'0')}-${String(m[3]).padStart(2,'0')}`;
  }
  const d=new Date(v);
  return isNaN(d.getTime())?'':getFormattedDate(d);
}
function formatThaiShortDate(v){
  const s=normalizeDateStr(v); if(!s)return '-';
  const [y,m,d]=s.split('-').map(Number);
  const dt=new Date(y,m-1,d);
  return `${DAY_SHORT_TH[dt.getDay()]} ${d} ${MONTH_SHORT_TH[m-1]} ${(y+543).toString().slice(-2)}`;
}
function escapeHtml(v){
  return String(v??'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;').replace(/'/g,'&#039;');
}
function showSuccess(title,message){
  document.getElementById('success-title').textContent=title;
  document.getElementById('success-message').textContent=message;
  document.getElementById('success-popup').classList.remove('hidden');
}
function closeSuccessPopup(){document.getElementById('success-popup').classList.add('hidden');}
function closeModal(id){document.getElementById(id).classList.add('hidden');}
function cacheTasks(){localStorage.setItem('homework_cached_tasks_v2',JSON.stringify(tasks));}
function cacheEvents(){localStorage.setItem('homework_cached_events_v2',JSON.stringify(dailyEvents));}

window.addEventListener('DOMContentLoaded',async()=>{
  try{tasks=JSON.parse(localStorage.getItem('homework_cached_tasks_v2')||'[]');}catch(e){tasks=[];}
  try{const ev=JSON.parse(localStorage.getItem('homework_cached_events_v2')||'{}');dailyEvents=ev?.events??ev;}catch(e){dailyEvents={};}
  
  if(localStorage.getItem('isLoggedIn')==='true'){
    currentRole=localStorage.getItem('userRole')||'member';
    currentUsername=localStorage.getItem('usernameKey')||'';
    openDashboard(localStorage.getItem('userName')||currentUsername.toUpperCase());
  }
  renderTasks(); renderCalendar(); selectDate(selectedDateStr);
  fetchAllData(false);
});

async function fetchJSON(url){
  const r=await fetch(url,{cache:'no-store'});
  if(!r.ok)throw new Error(`HTTP ${r.status}`);
  return await r.json();
}

async function fetchAllData(showAlert=false){
  if(showAlert) showLoading('กำลังซิงค์ข้อมูล...');
  try{
    const [tasksRes, usersRes, eventsRes, optionsRes] = await Promise.allSettled([
      fetchJSON(`${SCRIPT_URL}?t=${Date.now()}`),
      fetchJSON(`${SCRIPT_URL}?action=getUsers&t=${Date.now()}`),
      fetchJSON(`${SCRIPT_URL}?type=events&t=${Date.now()}`),
      fetchJSON(`${SCRIPT_URL}?action=getOptions&t=${Date.now()}`)
    ]);

    if(tasksRes.status==='fulfilled' && Array.isArray(tasksRes.value)){
      tasks=tasksRes.value.map(t=>({...t,id:String(t.id),isPinned:t.isPinned===true||String(t.isPinned).toUpperCase()==='TRUE'}));
      cacheTasks();
    }
    if(usersRes.status==='fulfilled' && Array.isArray(usersRes.value)){
      const sheetUsers = usersRes.value.map(u=>({
        username:String(u.username||u.Username||'').trim().toLowerCase(),
        role:String(u.role||u.Role||'member').trim().toLowerCase(),
        name:String(u.name||u.Name||u.username||u.Username||'').trim()
      })).filter(u=>u.username);
      if(sheetUsers.length>0) allowedUsersList=sheetUsers;
    }
    if(eventsRes.status==='fulfilled'){
      const data=eventsRes.value;
      const source=data.events??data, normalized={};
      if(source && typeof source==='object' && !Array.isArray(source)){
        Object.entries(source).forEach(([date,items])=>{
          const key=normalizeDateStr(date);
          if(key&&Array.isArray(items))normalized[key]=items.filter(x=>x!=null).map(String);
        });
        dailyEvents=normalized; cacheEvents();
      }
    }
    if(optionsRes.status==='fulfilled'){
      const data=optionsRes.value;
      if(data.status!=='error'){
        subjectOptions={
          codes:Array.isArray(data.codes)?data.codes.map(String):[],
          subjects:Array.isArray(data.subjects)?data.subjects.map(String):[]
        };
      }
    }

    renderTasks();
    renderCalendar();
    selectDate(selectedDateStr);
    
    if(showAlert) showSuccess('ซิงค์ข้อมูลสำเร็จ', 'อัปเดตข้อมูลล่าสุดเรียบร้อยแล้ว');
    return true;
  }catch(e){
    console.error('Turbo Fetch Error:', e);
    if(showAlert) alert('ซิงค์ข้อมูลไม่สำเร็จ กรุณาตรวจสอบการเชื่อมต่อ');
    return false;
  }finally{
    if(showAlert) hideLoading();
  }
}

async function login(){
  const raw=document.getElementById('username').value.trim().toLowerCase();
  if(!raw){alert('กรุณากรอกชื่อผู้ใช้งาน');return;}

  showLoading('กำลังตรวจสอบข้อมูลผู้ใช้...');
  const btn=document.getElementById('login-btn');
  btn.disabled=true;

  await fetchAllData(false);

  let user = allowedUsersList.find(u => u.username === raw);
  hideLoading();
  btn.disabled=false;

  if(!user){
    alert(`ไม่พบชื่อผู้ใช้งาน "${raw}" ในระบบ (โปรดตรวจสอบรายชื่อในแท็บ Users บน Google Sheets)`);
    return;
  }

  currentRole=user.role;
  currentUsername=user.username;
  localStorage.setItem('isLoggedIn','true');
  localStorage.setItem('userRole',currentRole);
  localStorage.setItem('userName',user.name);
  localStorage.setItem('usernameKey',currentUsername);
  
  resetIdleTimer();
  openDashboard(user.name);
  logActivity('login','เข้าสู่ระบบสำเร็จ');
}

function openDashboard(name){
  document.getElementById('login-page').classList.add('hidden');
  document.getElementById('main-page').classList.remove('hidden');
  document.getElementById('guest-badge').textContent=name;
  document.getElementById('btn-manage-history').classList.toggle('hidden',currentRole!=='admin');
  document.getElementById('btn-view-logs').classList.toggle('hidden',currentRole!=='admin');
  switchTab('pending'); renderTasks(); renderCalendar(); selectDate(selectedDateStr);
}

function logout(){
  logActivity('logout','ออกจากระบบ');
  ['isLoggedIn','userRole','userName','usernameKey'].forEach(k=>localStorage.removeItem(k));
  currentUsername=''; currentRole='member';
  document.getElementById('main-page').classList.add('hidden');
  document.getElementById('login-page').classList.remove('hidden');
  document.getElementById('username').value='';
}

function switchTab(tab){
  currentTab=tab;
  document.getElementById('tab-pending').className=tab==='pending'?'flex-1 py-2.5 rounded-xl bg-[#FF75BC] text-white':'flex-1 py-2.5 rounded-xl text-slate-500';
  document.getElementById('tab-calendar').className=tab==='calendar'?'flex-1 py-2.5 rounded-xl bg-[#50D8FF] text-slate-900':'flex-1 py-2.5 rounded-xl text-slate-500';
  document.getElementById('view-calendar').classList.toggle('hidden',tab!=='calendar');
  document.getElementById('view-tasks').classList.toggle('hidden',tab!=='pending');
  document.getElementById('admin-form').classList.add('hidden');
  document.getElementById('notice-text').textContent=tab==='calendar'?'คลิกเลือกวันที่ในปฏิทินเพื่อดูการบ้าน':'แสดงรายการการบ้านที่ค้างอยู่ทั้งหมด';
  if(tab==='calendar'){renderCalendar();selectDate(selectedDateStr);}else renderTasks();
}

function toggleAddForm(){
  switchTab('pending');
  const form=document.getElementById('admin-form');form.classList.toggle('hidden');
  if(!form.classList.contains('hidden'))form.scrollIntoView({behavior:'smooth'});
}

function renderCalendar(){
  const year=currentDate.getFullYear(),month=currentDate.getMonth();
  document.getElementById('calendar-month-year').textContent=`ปฏิทิน: ${MONTH_NAMES_TH[month]} (${year+543})`;
  const box=document.getElementById('calendar-days');box.innerHTML='';
  const offset=new Date(year,month,1).getDay(),total=new Date(year,month+1,0).getDate();
  for(let i=0;i<offset;i++)box.insertAdjacentHTML('beforeend','<div class="h-14 rounded-2xl bg-slate-50"></div>');
  for(let day=1;day<=total;day++){
    const ds=`${year}-${String(month+1).padStart(2,'0')}-${String(day).padStart(2,'0')}`;
    const due=tasks.some(t=>normalizeDateStr(t.dueDate)===ds);
    const event=(dailyEvents[ds]||[]).length>0,sel=ds===selectedDateStr;
    const style=sel?'bg-[#50D8FF] border-2 border-sky-400 text-slate-900':'bg-slate-50 border border-slate-100 text-slate-600 hover:border-sky-300';
    box.insertAdjacentHTML('beforeend',`<button onclick="selectDate('${ds}')" class="h-14 ${style} rounded-2xl p-1 flex flex-col items-center justify-between text-[11px] font-bold"><span>${day}</span><div class="flex gap-1 mt-0.5">${due?'<span class="w-1.5 h-1.5 bg-rose-500 rounded-full"></span>':''}${event?'<span class="w-1.5 h-1.5 bg-sky-600 rounded-full"></span>':''}</div></button>`);
  }
}

function selectDate(date){
  selectedDateStr=normalizeDateStr(date);if(!selectedDateStr)return;
  renderCalendar();
  const [y,m,d]=selectedDateStr.split('-').map(Number);
  document.getElementById('selected-date-label').textContent=`รายการวันที่ ${d} ${MONTH_NAMES_TH[m-1]} ${y+543}`;
  const ev=dailyEvents[selectedDateStr]||[];
  document.getElementById('event-list-view').innerHTML=ev.length?ev.map((x,i)=>`<div class="bg-white p-2 rounded-xl border border-sky-100 flex justify-between items-center text-xs"><span class="font-bold text-sky-950">${escapeHtml(x)}</span><button onclick="deleteSingleEvent(${i})" class="text-rose-500 px-2">ลบ</button></div>`).join(''):'<p class="text-xs text-slate-400">- ไม่มีกิจกรรมพิเศษ -</p>';
  const matched=tasks.filter(t=>normalizeDateStr(t.dueDate)===selectedDateStr||normalizeDateStr(t.assignDate)===selectedDateStr);
  document.getElementById('selected-task-count').textContent=`${matched.length} รายการ`;
  document.getElementById('selected-date-tasks').innerHTML=matched.length?matched.map(t=>createTaskCardHTML(t,t.isPinned)).join(''):'<div class="text-center py-3 text-xs text-slate-400 bg-white rounded-2xl">ไม่มีการบ้านสำหรับวันนี้</div>';
}

function changeMonth(offset){currentDate.setMonth(currentDate.getMonth()+offset);renderCalendar();}
function openEventModal(){
  switchTab('calendar');
  const [y,m,d]=selectedDateStr.split('-').map(Number);
  document.getElementById('modal-date-label').textContent=`วันที่เลือก: ${d} ${MONTH_NAMES_TH[m-1]} ${y+543}`;
  document.getElementById('daily-event-input').value='';
  document.getElementById('event-modal').classList.remove('hidden');
}
function closeEventModal(){document.getElementById('event-modal').classList.add('hidden');}

async function saveDailyEvent(){
  const input=document.getElementById('daily-event-input'),value=input.value.trim(),date=selectedDateStr;
  if(!value)return alert('กรุณากรอกชื่อกิจกรรม');
  const button=document.getElementById('save-event-btn');button.disabled=true;
  
  const next=[...(dailyEvents[date]||[]),value];
  dailyEvents[date]=next; cacheEvents();
  renderCalendar(); selectDate(date); closeEventModal();
  showSuccess('เพิ่มกิจกรรมสำเร็จ','บันทึกข้อมูลเรียบร้อย');

  try{
    const payload={action:'saveEvent',date:date,events:JSON.stringify(next),username:currentUsername};
    fetch(SCRIPT_URL,{method:'POST',mode:'no-cors',headers:{'Content-Type':'text/plain;charset=utf-8'},body:JSON.stringify(payload)});
    logActivity('add_event',`เพิ่มกิจกรรม ${value} วันที่ ${date}`);
  }catch(e){console.error(e);}
  finally{button.disabled=false;}
}

async function deleteSingleEvent(index){
  const date=selectedDateStr,old=[...(dailyEvents[date]||[])];
  if(!old[index]||!confirm('ต้องการลบกิจกรรมนี้ใช่หรือไม่?'))return;
  const removed=old[index],next=old.filter((_,i)=>i!==index);
  
  if(next.length) dailyEvents[date]=next; else delete dailyEvents[date];
  cacheEvents(); renderCalendar(); selectDate(date);

  try{
    const payload={action:'saveEvent',date:date,events:JSON.stringify(next),username:currentUsername};
    fetch(SCRIPT_URL,{method:'POST',mode:'no-cors',headers:{'Content-Type':'text/plain;charset=utf-8'},body:JSON.stringify(payload)});
    logActivity('delete_event',`ลบกิจกรรม ${removed} วันที่ ${date}`);
  }catch(e){console.error(e);}
}

function updateDatalists(){
  const codes=[...new Set([...subjectOptions.codes,...tasks.map(t=>String(t.code||'')).filter(Boolean)])];
  const subjects=[...new Set([...subjectOptions.subjects,...tasks.map(t=>String(t.subject||'')).filter(Boolean)])];
  document.getElementById('code-options').innerHTML=codes.map(v=>`<option value="${escapeHtml(v)}">`).join('');
  document.getElementById('subject-options').innerHTML=subjects.map(v=>`<option value="${escapeHtml(v)}">`).join('');
}

function renderTasks(){
  const pinned=tasks.filter(t=>t.isPinned),normal=tasks.filter(t=>!t.isPinned);
  document.getElementById('task-count').textContent=`${tasks.length} ITEMS`;
  document.getElementById('pinned-list').innerHTML=pinned.length?pinned.map(t=>createTaskCardHTML(t,true)).join(''):'<div class="text-center py-2 text-xs text-amber-600/60">ไม่มีรายการที่ปักหมุดไว้</div>';
  document.getElementById('task-list').innerHTML=normal.length?normal.map(t=>createTaskCardHTML(t,false)).join(''):(pinned.length?'':'<div class="text-center py-6 text-xs text-slate-400 bg-white rounded-2xl border">ไม่มีรายการงาน</div>');
  updateDatalists();
}

function createTaskCardHTML(item,pinned){
  const pin=currentRole==='admin'?`<button onclick="togglePin('${escapeHtml(item.id)}')" class="text-xs p-1 ${pinned?'text-amber-500':'text-slate-300'}"><i class="fa-solid fa-thumbtack"></i></button>`:'';
  const creator=item.createdByName?`<span class="text-[9px] text-slate-400">โดย: ${escapeHtml(item.createdByName)}</span>`:'';
  return `<div class="bg-white p-3.5 rounded-2xl border border-slate-100 shadow-sm space-y-1.5">
    <div class="flex justify-between items-start"><div class="flex items-center gap-1.5"><span class="bg-[#FF75BC] text-white text-[10px] font-bold px-2 py-0.5 rounded-full">${escapeHtml(item.code||item.subject||'งาน')}</span>${creator}</div><div class="flex gap-1">${pin}<button onclick="deleteTask('${escapeHtml(item.id)}')" class="text-xs p-1 text-slate-300 hover:text-rose-500"><i class="fa-solid fa-trash-can"></i></button></div></div>
    <h4 class="text-xs font-bold text-slate-800">${escapeHtml(item.detail)}</h4>
    <div class="flex flex-col items-end text-[10px] pt-1"><span class="text-slate-400">สั่ง: ${formatThaiShortDate(item.assignDate)}</span><span class="text-rose-500 font-bold">ส่ง: ${formatThaiShortDate(item.dueDate)}</span></div></div>`;
}

async function postWriteTurbo(payload){
  fetch(SCRIPT_URL,{
    method:'POST',mode:'no-cors',
    headers:{'Content-Type':'text/plain;charset=utf-8'},
    body:JSON.stringify(payload)
  });
  await new Promise(r=>setTimeout(r,400));
  fetchAllData(false);
}

async function togglePin(id){
  if(currentRole!=='admin')return alert('เฉพาะผู้ดูแลระบบเท่านั้นที่ปักหมุดได้');
  const task=tasks.find(t=>String(t.id)===String(id));if(!task)return;
  const newValue=!task.isPinned;
  
  task.isPinned=newValue; cacheTasks(); renderTasks();
  await postWriteTurbo({action:'togglePin',id,isPinned:newValue,username:currentUsername});
  logActivity('toggle_pin',`${newValue?'ปักหมุด':'ยกเลิกปักหมุด'} งาน ${id}`);
}

async function deleteTask(id){
  if(!confirm('คุณต้องการลบรายการนี้ใช่หรือไม่?'))return;
  
  tasks=tasks.filter(t=>String(t.id)!==String(id));
  cacheTasks(); renderTasks(); renderCalendar(); selectDate(selectedDateStr);
  showSuccess('ลบรายการแล้ว','ลบข้อมูลเรียบร้อย');

  await postWriteTurbo({action:'delete',id,username:currentUsername});
  logActivity('delete_task',`ลบการบ้าน ID ${id}`);
}

async function addTask(){
  const code=document.getElementById('task-code').value.trim();
  const subject=document.getElementById('task-subject').value.trim();
  const detail=document.getElementById('task-detail').value.trim();
  const assignDate=document.getElementById('assign-date-input').value;
  const dueDate=document.getElementById('due-date-input').value;
  if(!code||!subject||!detail||!assignDate||!dueDate)return alert('กรุณากรอกข้อมูลให้ครบทุกช่อง');
  if(assignDate>dueDate)return alert('วันที่สั่งงานไม่สามารถอยู่หลังวันที่ส่งงานได้');
  
  const btn=document.getElementById('add-task-btn');btn.disabled=true;
  const newTask={id:Date.now().toString(),code,subject,detail,assignDate,dueDate,isPinned:false,username:currentUsername,role:currentRole,createdByName:localStorage.getItem('userName')||currentUsername};

  tasks.unshift(newTask);
  cacheTasks(); renderTasks(); renderCalendar();
  selectedDateStr=dueDate; selectDate(selectedDateStr);
  
  ['task-code','task-subject','task-detail','assign-date-input','due-date-input'].forEach(id=>document.getElementById(id).value='');
  document.getElementById('admin-form').classList.add('hidden');
  showSuccess('เพิ่มงานสำเร็จ','บันทึกข้อมูลเรียบร้อย');
  btn.disabled=false;

  await postWriteTurbo({...newTask, action:'add'});
  logActivity('add_task',`เพิ่มการบ้าน ${code} ${subject}`);
}

function logActivity(activity,details){
  if(!currentUsername)return;
  const payload={action:'logActivity',activity,details,username:currentUsername,displayName:localStorage.getItem('userName')||currentUsername,role:currentRole,device:navigator.userAgent};
  fetch(SCRIPT_URL,{method:'POST',mode:'no-cors',headers:{'Content-Type':'text/plain;charset=utf-8'},body:JSON.stringify(payload)}).catch(e=>{});
}

async function openLogsModal(){
  if(currentRole!=='admin')return alert('ไม่มีสิทธิ์เข้าถึง');
  document.getElementById('logs-modal').classList.remove('hidden');
  await loadLogs();
}

async function loadLogs(){
  const status=document.getElementById('logs-status'),container=document.getElementById('logs-list');
  status.textContent='กำลังโหลดประวัติ...';container.innerHTML='';
  showLoading('กำลังดึงประวัติการใช้งาน...');
  try{
    const data=await fetchJSON(`${SCRIPT_URL}?action=getLogs&username=${encodeURIComponent(currentUsername)}&t=${Date.now()}`);
    hideLoading();
    if(!Array.isArray(data))throw new Error(data.message||'รูปแบบประวัติไม่ถูกต้อง');
    status.textContent=`แสดง ${data.length} รายการล่าสุด`;
    container.innerHTML=data.length?data.map(x=>`<div class="p-3 rounded-xl border bg-slate-50"><div class="flex justify-between gap-2"><b class="text-xs text-slate-800">${escapeHtml(x.displayName||x.username||'ผู้ใช้')}</b><span class="text-[10px] text-slate-400">${escapeHtml(x.timestamp||'')}</span></div><div class="text-xs text-purple-700 mt-1">${escapeHtml(x.action||'')}</div><div class="text-xs text-slate-600 mt-1">${escapeHtml(x.details||'')}</div><div class="text-[10px] text-slate-400 mt-1">${escapeHtml(x.role||'')} · ${escapeHtml(x.username||'')}</div></div>`).join(''):'<p class="text-xs text-slate-400">ยังไม่มีประวัติการใช้งาน</p>';
  }catch(e){
    hideLoading();
    status.textContent='โหลดประวัติไม่สำเร็จ: '+e.message;
  }
}

async function clearLogs(){
  if(currentRole!=='admin')return alert('ไม่มีสิทธิ์เข้าถึง');
  if(!confirm('คุณต้องการลบประวัติการใช้งานทั้งหมดใช่หรือไม่?'))return;
  
  showLoading('กำลังล้างประวัติ...');
  try{
    await fetch(SCRIPT_URL,{
      method:'POST',mode:'no-cors',
      headers:{'Content-Type':'text/plain;charset=utf-8'},
      body:JSON.stringify({action:'clearLogs',username:currentUsername})
    });
    await new Promise(r=>setTimeout(r,600));
    await loadLogs();
    hideLoading();
    showSuccess('ล้างประวัติสำเร็จ', 'ลบประวัติการใช้งานทั้งหมดเรียบร้อยแล้ว');
  }catch(e){
    hideLoading();
    alert('ล้างประวัติไม่สำเร็จ: '+e.message);
  }
}

async function openManageModal(){
  if(currentRole!=='admin')return alert('ไม่มีสิทธิ์เข้าถึง');
  document.getElementById('manage-modal').classList.add('hidden');
  loadOptions();
}

async function loadOptions(){
  try{
    const data=await fetchJSON(`${SCRIPT_URL}?action=getOptions&t=${Date.now()}`);
    if(data.status==='error')throw new Error(data.message);
    subjectOptions={
      codes:Array.isArray(data.codes)?data.codes.map(String):[],
      subjects:Array.isArray(data.subjects)?data.subjects.map(String):[]
    };
    updateDatalists();
    renderManageOptions();
  }catch(e){console.error(e);}
}

function renderManageOptions(){
  const makeList=(values,type)=>values.length?values.map(v=>`<div class="p-2 border rounded-xl bg-slate-50"><div class="text-xs font-bold break-words">${escapeHtml(v)}</div><button onclick="disableOption('${type}','${escapeHtml(v).replace(/'/g,"&#039;")}')" class="mt-1 text-[10px] bg-rose-50 text-rose-600 px-2 py-1 rounded-lg">ซ่อนตัวเลือก</button></div>`).join(''):'<p class="text-[11px] text-slate-400">ไม่มีตัวเลือก</p>';
  document.getElementById('manage-codes').innerHTML=makeList(subjectOptions.codes,'code');
  document.getElementById('manage-subjects').innerHTML=makeList(subjectOptions.subjects,'subject');
}

async function disableOption(type,value){
  if(currentRole!=='admin')return;
  if(!confirm(`ต้องการซ่อนตัวเลือก "${value}" ใช่หรือไม่?`))return;
  const status=document.getElementById('manage-status');status.textContent='กำลังบันทึก...';
  try{
    await fetch(SCRIPT_URL,{method:'POST',mode:'no-cors',headers:{'Content-Type':'text/plain;charset=utf-8'},body:JSON.stringify({action:'clearHistoryType',type,value,username:currentUsername})});
    await new Promise(r=>setTimeout(r,400));
    await loadOptions();
    status.textContent='ซ่อนตัวเลือกเรียบร้อย';
    logActivity('disable_option',`ซ่อน ${type}: ${value}`);
  }catch(e){status.textContent='ดำเนินการไม่สำเร็จ: '+e.message;}
}
