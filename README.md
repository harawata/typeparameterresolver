[![Maven Central](https://maven-badges.herokuapp.com/maven-central/net.harawata.reflection/typeparameterresolver/badge.svg)](https://maven-badges.herokuapp.com/maven-central/net.harawata.reflection/typeparameterresolver)
[![License](https://img.shields.io/badge/Apache-2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GitHub CI](https://github.com/harawata/typeparameterresolver/actions/workflows/ci.yaml/badge.svg)](https://github.com/harawata/typeparameterresolver/actions/workflows/ci.yaml)

TypeParameterResolver: resolve actual types
-------------------------------------------

The library provides convenience API on top of Java Reflection so that you can resolve actual
types of fields, method parameters, and method return types.

The code is based on [TypeParameterResolver from mybatis-3](https://github.com/mybatis/mybatis-3/blob/mybatis-3.5.11/src/main/java/org/apache/ibatis/reflection/TypeParameterResolver.java).

Sample
------

```xml
<dependency>
  <groupId>net.harawata.reflection</groupId>
  <artifactId>typeparameterresolver</artifactId>
  <version>...</version>
</dependency>
```

```java
abstract class StringIntegerMap implements Map<String, Integer> {
}

@Test
void testEntry() throws NoSuchMethodException {
    Method get = StringIntegerMap.class.getMethod("get", Object.class);

    // Regular getReturnType returns Object since generic type is erased.
    assertEquals(Object.class, get.getReturnType());
    // getGenericReturnType returns type variable name, however, it does not
    // resolve to the actual type
    assertEquals("V", get.getGenericReturnType().toString());
    assertEquals("V", get.getAnnotatedReturnType().toString());

    // TypeParameterResolver comes to rescue, and it resolves the actual type
    // according to type arguments of the given class.
    Type getReturnType = TypeParameterResolver.resolveReturnType(get, StringIntegerMap.class);
    assertEquals(Integer.class, getReturnType);
}
```

APIs:

* `Type resolveFieldType(Field field, Type srcType)`
* `Type resolveReturnType(Method method, Type srcType)`
* `Type[] resolveParamTypes(Method method, Type srcType)`

License
-------

Apache-2.0

Similar projects
----------------

* https://github.com/leangen/geantyref
* https://github.com/spring-projects/spring-framework/blob/v6.0.2/spring-core/src/main/java/org/springframework/core/GenericTypeResolver.java#L44
* https://github.com/FasterXML/jackson-databind/blob/2.15/src/main/java/com/fasterxml/jackson/databind/introspect/MethodGenericTypeResolver.java#L23
* https://github.com/ljacqu/JavaTypeResolver
* https://github.com/mkarneim/generictype
* https://github.com/kptfh/generic-type-resolver
