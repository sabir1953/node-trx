node-trx
========

This is a Node utility for generating test results file in the TRX format.  This format is compliant with the namespace http://microsoft.com/schemas/VisualStudio/TeamTest/2010, found in full spec in %VSINSTALLDIR%\xml\Schemas\vstst.xsd. 

This libary is a partial implementation of the XSD.

This library was designed to reduce domain knowledge needed to create TRX files via a simple, fluent interface. 

```javascript
var fs = require('fs')
  , TRX = require('../trx')
  , TestRun = TRX.TestRun
  , UnitTest = TRX.UnitTest
  , computerName = 'bmanci01'
  , run;


run = new TestRun({ 
    name: 'Sample TRX Import',
    runUser: 'Brian Mancini'
  })
  .addResult({ 
    test: new UnitTest({ name: 'test 1', methodName: 'test1', methodCodeBase: 'testing-framework', methodClassName: 'test1' }),
    computerName: computerName,
    outcome: 'Passed',
    duration: '00:00:44.7811567',
    startTime: '2010-11-16T08:48:29.9072393-08:00',
    endTime: '2010-11-16T08:49:16.9694381-08:00'
  })
  .addResult({
    test: new UnitTest({ name: 'test 2', methodName: 'test2', methodCodeBase: 'testing-framework', methodClassName: 'test2' }),
    computerName: computerName,
    outcome: 'Inconclusive',
    duration: '00:00:44.7811567',
    startTime: '2010-11-16T08:48:29.9072393-08:00',
    endTime: '2010-11-16T08:49:16.9694381-08:00'
  })
  .addResult({
    test: new UnitTest({ name: 'test 3', methodName: 'test3', methodCodeBase: 'testing-framework', methodClassName: 'test3' }),
    computerName: computerName,
    outcome: 'Failed',
    duration: '00:00:44.7811567',
    startTime: '2010-11-16T08:48:29.9072393-08:00',
    endTime: '2010-11-16T08:49:16.9694381-08:00',
    output: 'This is sample output for the unit test',
    errorMessage: 'This unit test failed for a bad reason',
    errorStacktrace: 'at test3() in c:\\tests\\test3.js:line 1'
  });


// output the json to the screen
console.log(run);

// write the output to exmample.trx
fs.writeFileSync('example.trx', run.toXml());
```


Error information can be provided by including `errorMessage` and `errorStacktrace` properties in the result. Stacktrace must conform to the syntax `at [method signature] in [file path]:[line number]` or it will be picked up by Visual Studio.

Standard output information can be included in the `output` property.