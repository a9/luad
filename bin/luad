const fs = require('fs');
const path = require('path');
const {spawnSync} = require('child_process');
const dir = path.resolve(process.argv[2]);
function fq(dir, callback) {
  const files = fs.readdirSync(dir);
  files.forEach(function (item) {
    const f = path.join(dir, item);
    const stat = fs.statSync(f);
    if (stat.isDirectory()) {
      fq(path.join(dir, item), callback);
      return true
    } else {
      const ext = path.extname(f);
      if (ext !== '.lua') {
        return true
      }
      callback(f)
    }
  })
}
const name = path.basename(dir);
const dirr = path.join(path.dirname(dir), name + '-dist');
if (!fs.existsSync(dirr)) {
  fs.mkdirSync(dirr);
}

fq(dir, function (file) {
  const q = file.replace(path.join(dir, '/'), '');
  const o = path.join(dirr, q);
  const od = path.dirname(o);
  if (!fs.existsSync(od)) {
    fs.mkdirSync(od)
  }

  console.log(file, o);

  const s = spawnSync(__dirname + '\\luadec.exe', [file]);
  fs.writeFileSync(o, s.output[1].toString());
});