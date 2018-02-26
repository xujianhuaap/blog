---
title: javascript-typescript-2
date: 2018-02-26 20:21:39
tags: javascript_typescript
---
```
  1. intersection type （交集type)
     type LiHong =  Person & Teacher & Customer  
     LiHong 拥有多重特性（Person Teacher Customer)
  2. union type
     function padLeft(value: string, padding: string|number) {
	    if (typeof padding === "number") {
		return Array(padding + 1).join(" ") + value;
	    }
	    if (typeof padding === "string") {
		return padding + value;
	    }
	    throw new Error(`Expected string or number, got '${padding}'.`);
	} 
     padding 可以是string 或者number
 3. typeof 类型检查
    function isNumber(x: any): x is number {
	    return typeof x === "number";
	}
    只能是number string symbol boolean
 4. instanceOf 类型检查
	interface Padder {
	    getPaddingString(): string
	}

	class SpaceRepeatingPadder implements Padder {
	    constructor(private numSpaces: number) { }
	    getPaddingString() {
		return Array(this.numSpaces + 1).join(" ");
	    }
	}

	class StringPadder implements Padder {
	    constructor(private value: string) { }
	    getPaddingString() {
		return this.value;
	    }
        }	

	function getRandomPadder() {
	    return Math.random() < 0.5 ?
		new SpaceRepeatingPadder(4) :
		new StringPadder("  ");
	}

	// Type is 'SpaceRepeatingPadder | StringPadder'
	let padder: Padder = getRandomPadder();

	if (padder instanceof SpaceRepeatingPadder) {
	    padder; // type narrowed to 'SpaceRepeatingPadder'
	}
	if (padder instanceof StringPadder) {
	    padder; // type narrowed to 'StringPadder'
	}

5. 映射类型
   interface Person {
      name?:string;
      age?:number;
   }
   class ReadOnly<T>{
     readonly [ p keyof T]:T[p];
   }
```
