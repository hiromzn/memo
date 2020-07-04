fs.createReadStream(__dirname + '/public/test.csv')
  .pipe(csv.parse(function(err, data) {
 
        console.log(data);
 
  }));

fs.createReadStream(__dirname + '/public/test.csv')
  .pipe(csv.parse({columns: true}, function(err, data) {
 
        console.log(data);
 
  }));
