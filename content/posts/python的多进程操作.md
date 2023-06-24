---
title: python的多进程操作
hideSummary: True
hideMeta: True
---

# 对比`multiprocessing.Pool`常用的几个方法

| function    | Multi-args | Concurrence | Blocking | Ordered-results |
| ----------- | ---------- | ----------- | -------- | --------------- |
| map         | no         | yes         | yes      | yes             |
| apply       | yes        | no          | yes      | no              |
| map_async   | no         | yes         | no       | yes             |
| apply_async | yes        | yes         | no       | no              |

# 举个例子

```python
def func(num):
    # print("[func()]pid:", os.getpid())
    print(num, end=" ", flush=True)    # 增加flush=True才能体现出多进程和进程池
    # sys.stdout.flush()    # 增加这条语句才能体现出多进程和进程池
    # time.sleep(0.5)
    return num


results = []


def collect_result(result):    # NOTE: 只能带一个参数
    global results
    # print("[collect_result()]pid:", os.getpid())
    results.append(result)


def apply_map_async_demo():
    """
    带返回值的多进程
    """
    COUNT = 20

    print("\napply", "--" * 20)
    t = time.time()
    local_results = []
    with multiprocessing.Pool(5) as pool:
        for i in range(COUNT, 0, -1):
            # NOTE: pool.apply不支持并发: 但多个进程串行地执行每个任务(仍然是多进程执行)
            local_results.append(pool.apply(func, (i,)))
        pool.close()
    print("\n{}".format(local_results))
    print("apply time cost:", time.time() - t, "\n", "**"*30)

    print("\napply_async", "--" * 20)
    t = time.time()
    with multiprocessing.Pool(5) as pool:
        for i in range(COUNT, 0, -1):
            pool.apply_async(func, (i,), callback=collect_result)    # 并发. callback是可选的, 可以不使用该字段
        pool.close()
        pool.join()
    global results
    print("\n{}".format(results))
    """
    # NOTE: print的结果是无序的 
    20 19 18 17 16 14 13 15 12 11 10 9 7 6 8 5 4 3 2 1
    # NOTE: 返回的结果也是无序的(apply_async不保证返回值的顺序)
    [20, 18, 19, 16, 17, 14, 13, 12, 11, 10, 9, 7, 6, 5, 4, 3, 15, 2, 8, 1]
    """
    print("apply_async time cost:", time.time() - t, "\n", "**"*30)

    print("\nmap", "--" * 20)
    t = time.time()
    with multiprocessing.Pool(5) as pool:
        results = pool.map(func, range(COUNT, 0, -1))    # 并发
        pool.close()
    print("\n{}".format(results))
    """
    # NOTE: print的结果是无序的
    20 19 17 16 18 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1
    # NOTE: 但返回的结果是有序的(map能够保证返回值的顺序按照函数调用的顺序)
    [20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    """
    print("map:", time.time() - t, "\n", "**"*30)

    print("\nmap_async", "--" * 20)
    t = time.time()
    results = []
    with multiprocessing.Pool(5) as pool:
        pool.map_async(func, range(COUNT, 0, -1), callback=collect_result)    # 并发. callback是可选的, 可以不使用该字段
        pool.close()
        pool.join()
    print("\n{}".format(results))
    """
    # NOTE: print的结果是无序的
    20 19 17 18 16 15 13 14 10 12 9 8 7 6 11 5 4 3 2 1 
    # NOTE: 但返回的结果是有序的(map_async能够保证返回值的顺序按照函数调用的顺序)
    # map和map_async的结果是以list的形式返回的, 所以只会调用collect_result一次，因此结果是list of list
    [[20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]]
    """
    print("map_async:", time.time() - t, "\n", "**"*30)

    print("end")


if __name__ == "__main__":
    apply_map_async_demo()
```

输出结果：

```
apply ----------------------------------------
20 19 18 17 16 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1    # 不保证有序
[20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]    # 不保证有序
apply time cost: 0.13375353813171387 
 ************************************************************

apply_async ----------------------------------------
20 19 18 17 16 14 13 15 12 11 10 9 7 6 8 5 4 3 2 1    # 无序
[20, 18, 19, 16, 17, 14, 13, 12, 11, 10, 9, 7, 6, 5, 4, 3, 15, 2, 8, 1]    # 无序
apply_async time cost: 0.10675978660583496 
 ************************************************************

map ----------------------------------------
20 19 17 16 18 15 14 13 12 11 10 9 8 7 6 5 4 3 2 1    # 无序
[20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]    # 有序
map: 0.10820269584655762 
 ************************************************************

map_async ----------------------------------------
20 19 17 18 16 15 13 14 10 12 9 8 7 6 11 5 4 3 2 1    # 无序
[[20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1]]    # 有序
map_async: 0.11021280288696289 
 ************************************************************
end
```

# 总结

在Python多进程编程中，`multiprocessing.Pool`类是一个常用的工具，它可以方便地创建进程池，并行执行任务。其中，`map`和`apply_async`是两个常用的方法，它们的区别在于以下几个方面：

- `pool.map`是一个阻塞方法，会等待所有的任务执行完成并返回结果列表。而`pool.apply_async`是一个非阻塞方法，会立即返回AsyncResult对象，任务的执行结果需要通过调用该对象的get()方法获取。
- `pool.map`的任务数量必须确定，因为它会一次性把所有任务提交到进程池中执行。而`pool.apply_async`可以动态地提交任务，因为它是在后台异步执行任务。
- `pool.map`的任务必须是可迭代的对象，每个任务的参数需要在迭代器中提供。而`pool.apply_async`可以传递任意数量和类型的参数，任务的参数可以通过apply_async方法的可变参数列表传递。

因此，当需要并行执行多个任务时，如果任务数量已知且任务间没有依赖关系，可以使用`pool.map`方法，它更加简洁易用。如果任务数量不确定或需要动态调度任务，并且希望立即返回一个AsyncResult对象以便后续处理，可以使用`pool.apply_async`方法。

总之，选择`map`还是`apply_async`方法，取决于具体的使用场景和需求。需要根据实际情况进行选择和搭配。