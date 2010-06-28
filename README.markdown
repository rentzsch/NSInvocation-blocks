# NSInvocation+blocks

Inspired by [Jonathan Wight's `CInvocationGrabber`](http://toxicsoftware.com/grab-that-invocation/) and [Dave Dribin's `DDInvocationGrabber`](http://www.dribin.org/dave/blog/archives/2008/05/22/invoke_on_main_thread/), I realized Objective-C's newish support for blocks could make generating `NSInvocation`s *even easier*.

Let's have a bake-off!

**Manual:**

	NSInvocation *theInvocation = [NSInvocation invocationWithMethodSignature:[theString methodSignatureForSelector:@selector(insertString:atIndex:)]];
	[theInvocation setSelector:@selector(insertString:atIndex:)];
	[theInvocation setTarget:theString];
	NSString *theFirstArgument = "Hello World";
	[theInvocation setArgument:&theFirstArgument atIndex:2];
	unsigned theSecondArgument = 42;
	[theInvocation setArgument:&theFirstArgument atIndex:3];

*Ick.*

**CInvocationGrabber:**

	CInvocationGrabber *theGrabber = [CInvocationGrabber invocationGrabber];
	[[theGrabber prepareWithInvocationTarget:theString] insertString:@"Hello World" atIndex:42];
	NSInvocation *theInvocation = [theGrabber invocation];

*Much nicer.*

**NSInvocation+blocks:**

	NSInvocation *theInvocation = [NSInvocation jr_invocationWithTarget:theString block:^(id theString){
		[theString insertString:@"Hello World" atIndex:42];
	}];

*Butter.*