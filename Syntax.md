
**PHINODE**

所有 LLVM 指令都使用 SSA (Static Single Assignment，静态一次性赋值) 方式表示。意思是所有变量都只能被赋值一次，这样做主要是便于后期的代码优化。

```python
1 a = 1;
2 if (v < 10)
3     a = 2;
4 b = a;
```

假设 v 的值小于 10，变量 a 就要被赋值为 2，但 a 已经被赋值了一次，由于 SSA 性质的约束，只能赋值另外一个“a”。最后在给 b 赋值时，通过添加一个 PHI node，由其来决定选择哪个版本的 a 来给 b 赋值。

```python
1 a1 = 1;
2 if (v < 10)
3     a2 = 2;
4 b = PHI(a1, a2);
```

PHI node 根据控制流是从哪一个 block (“a1 = 1” or “a2 = 2”) 到达，来决定使用 a1 还是 a2 来给 b 赋值。

这些 PHI node 必须在 IR 中显示创建，llvm 指令集中有对应的 phi 指令。
