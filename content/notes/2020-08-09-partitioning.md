+++
+++
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Markmap</title>
<style>
* {
  margin: 0;
  padding: 0;
}
#mindmap {
  display: block;
  width: 100vw;
  height: 100vh;
}
</style>

</head>
<body>
<svg id="mindmap"></svg>
<script src="https://cdn.jsdelivr.net/npm/d3@5"></script><script src="https://cdn.jsdelivr.net/npm/markmap-lib@0.7.11/dist/browser/view.min.js"></script><script>((t,a,e,n)=>{const{Markmap:o,loadPlugins:s}=window.markmap;(a?a(s,e,n):Promise.resolve()).then(()=>{o.create("svg#mindmap",null,t)})})({"t":"heading","d":1,"v":"Partitioning","c":[{"t":"heading","d":2,"v":"Partition by","c":[{"t":"list_item","d":3,"v":"key range"},{"t":"list_item","d":3,"v":"hash of key"}]},{"t":"heading","d":2,"v":"Secondary index","c":[{"t":"list_item","d":3,"v":"partition by document"},{"t":"list_item","d":3,"v":"partition by term"}]},{"t":"heading","d":2,"v":"Skews &amp; Rebalancing","c":[{"t":"list_item","d":3,"v":"hot key"},{"t":"list_item","d":3,"v":"rebalance strategies","c":[{"t":"list_item","d":4,"v":"fixed # partitions"},{"t":"list_item","d":4,"v":"split &amp; merge based on #nodes"},{"t":"list_item","d":4,"v":"split &amp; merge based on partition size"}]}]}]})</script>
</body>
</html>
