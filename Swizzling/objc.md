# ObjC Swizzling

Don't forget about correct return type, when using initializer swizzling.

E.g., let's swizzle IOSuface constructor 'initWithProperties:'.

Do not forget to return __void*__ or __id__ (pointer on created surface).
```
static IMP __surface_init_original_Method;

void* __surface_init_swizzled_Method(id slf, SEL _cmd, id opt)
{
  return ((void*(*)(id,SEL,id))__surface_init_original_Method)(slf, _cmd, opt);
}

- (void) __swizzleSurface
{
  Class class = NSClassFromString(@"IOSurface");
  Method m = class_getInstanceMethod(class,
                                     @selector(initWithProperties:));
  __surface_init_original_Method = method_setImplementation(m, (IMP)__surface_init_swizzled_Method);
}

```