#js
1. Factory pattern
2. Constructor pattern
3. Prototype pattern
4. Dynamic prototype pattern

### Factory pattern

```js
//Factory pattern for object Creation
var computerFactory = function (ram, hardDisk) {
    var computer = {}; //creating a new temporary object
    //Create class properties
    computer.ram = ram;
    computer.hardDisk = hardDisk;
    //Create class methods
    computer.AvailableMemory = function () {
        console.log('Hard-disk : ' + this.hardDisk);
        console.log('Ram : ' + this.ram);
    };
    return computer;
};

var computer1 = computerFactory(4,512);
```

### Constructor pattern

```js
var computer = function(ram, hardDisk) {
  //Create class properties
  this.ram = ram;
  this.hardDisk = hardDisk;
  //Create class methods
  this.AvailableMemory = function() {
    console.log('Hard-disk : ' + this.hardDisk);
    console.log('Ram : ' + this.ram);
  };
};

var computer1 = new computer(4,512);
```

### Prototype pattern

```js
var computer = function() {};
computer.prototype.ram = NaN;
computer.prototype.hardDisk = NaN;
computer.prototype.AvailableMemory = function() {
  console.log('Hard-disk : ' + this.hardDisk);
  console.log('Ram : ' + this.ram);
};
//Creating new object(s) for computer class
// by using Prototype Pattern
var computer1 = new computer(); 
//Empty object created with default values
computer1.ram = 4; //Assigning the actual value for object property
computer1.hardDisk = 512;
var computer2 = new computer();
computer2.ram = 16;
computer2.hardDisk = 1024;
//Accessing class methods using objects
computer1.AvailableMemory();
computer2.AvailableMemory();
```

### Dynamic prototype pattern

```js
var computerDynamicProto = function(ram, hardDisk) {
  this.ram = ram;
  this.hardDisk = hardDisk;
  if (typeof this.AvailableMemory !== 'function') {
    computerDynamicProto.prototype.AvailableMemory = function() {
      console.log('\nHarddisk : ' + this.hardDisk);
      console.log('Ram : ' + this.ram);
    };
  }
};
var computer1 = new computerDynamicProto(4,512);
computer1.AvailableMemory();
```

