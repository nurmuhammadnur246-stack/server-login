const CORS={'Access-Control-Allow-Origin':'*','Access-Control-Allow-Methods':'GET,POST,PUT,DELETE,OPTIONS','Access-Control-Allow-Headers':'Content-Type'};
const DEF=[{username:'DANZX',password:'DANZX441',role:'OWNER',expired:'2099-12-31',verified:true,crown:true}];
export default{async fetch(r,env){
  if(r.method==='OPTIONS')return new Response(null,{headers:CORS});
  const p=new URL(r.url).pathname;
  if(p==='/accounts'&&r.method==='GET'){const d=await env.DB.get('acc');return Response.json(d?JSON.parse(d):DEF,{headers:CORS});}
  if(p==='/accounts'&&r.method==='PUT'){await env.DB.put('acc',JSON.stringify(await r.json()));return Response.json({ok:true},{headers:CORS});}
  if(p==='/chat'&&r.method==='GET'){const d=await env.DB.get('chat');return Response.json(d?JSON.parse(d):[],{headers:CORS});}
  if(p==='/chat'&&r.method==='POST'){const m=await r.json();const d=await env.DB.get('chat');const c=d?JSON.parse(d):[];c.push({...m,ts:Date.now()});if(c.length>200)c.splice(0,c.length-200);await env.DB.put('chat',JSON.stringify(c));return Response.json({ok:true},{headers:CORS});}
  if(p.startsWith('/chat/')&&r.method==='DELETE'){const ts=parseInt(p.split('/')[2]);const d=await env.DB.get('chat');let c=d?JSON.parse(d):[];c=c.filter(m=>m.ts!==ts);await env.DB.put('chat',JSON.stringify(c));return Response.json({ok:true},{headers:CORS});}
  return new Response('404',{status:404});
}};# server-login
login
